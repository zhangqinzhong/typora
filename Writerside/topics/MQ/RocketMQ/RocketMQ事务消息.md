# RocketMQ事务消息

> RocketMQ事务消息

一. 事务消息的定义
   事务消息可以认为是一个两阶段的提交消息实现，以确保分布式事务的最终一致性。事务性消息确保本地事务的执行和消息的发送可以原子执行。
   两阶段提交主要保证了分布式事务的原子性：即所有结点要么全做要么全不做，所谓的两个阶段是指：第一阶段：准备阶段；第二阶段：提交阶段。
   事务消息有三种状态：
1. TransactionStatus.CommitTransaction: 提交事务，表示允许消费者消费该消息。
2. TransactionStatus.RollbackTransaction: 回滚事务，表示该消息将被删除，不允许消费。
3. TransactionStatus.Unknow: 中间状态，表示需要MQ回查才能确定状态。

二. 事务消息的实现流程
![fc76abf0-4cdb-37f7-5f09-c1aecd2dcfb5](fc76abf0-4cdb-37f7-5f09-c1aecd2dcfb5.png)

1. 生产者发送half消息，broker接收到half消息并回复half消息。
2. 生产者调用 TransactionListener.executeTransaction() 方法执行本地事务。
3. 生产者获得本地事务执行状态，提交给broker。如果状态是COMMIT_MESSAGE状态的话则broker会将消息推送给消费者。如果状态是ROLLBACK_MESSAGE状态的话则broker会丢弃此消息。如果状态是中间状态UNKNOW状态则broker会回查本地事务状态。
4. 生产者调用 TransactionListener.checkLocalTransaction() 方法回查本地事务执行状态，并再次执行5,6,7三步骤，若回查次数超过15次则丢弃。

使用限制：
1. 事务性消息没有调度和批处理支持。
2. 为避免单条消息被检查次数过多，导致半队列消息堆积，我们默认单条消息的检查次数限制为15次，但用户可以通过更改 transactionCheckMax 来更改此限制，如果一条消息的检查次数超过 transactionCheckMax 次，broker默认会丢弃这条消息，同时打印错误日志。用户可以重写 AbstractTransactionCheckListener 类来改变这种行为。
3. 事务消息将一定时间后检查，该时间由代理配置中的参数 transactionTimeout 确定。并且用户也可以在发送事务消息时通过设置用户属性 CHECK_IMMUNITY_TIME_IN_SECONDS 来改变这个限制，这个参数优先于 transactionMsgTimeout 参数。
4. 一个事务性消息会被检查或消费不止一次。
5. 事务性消息的生产者ID不能与其他类型消息的生产者ID共享，与其他类型的消息不同，事务性消息允许向后查询。MQ服务器通过其生产者ID查询客户端。
6. 提交给用户目标主题的消息reput可能会失败，目前它取决于日志记录，高可用是由RocketMQ本身的高可用机制来保证的。如果要保证事务消息不丢失，保证事务完整性，推荐使用同步双写机制。

三. 事务消息的实现示例

3.1. 事务消息的消费者
事务消息的消费者与普通消息的消费者基本相同，也就是说事务消息是控制生产者端和broker端。

```Java
DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("my_transaction_consumer");
    consumer.setNamesrvAddr("172.31.184.89:9876");
    consumer.subscribe("TransactionTopic", "*");
    // 4.创建一个回调函数
    consumer.registerMessageListener((MessageListenerConcurrently) (msgs, context) -> {
      // 5.处理消息
      for (MessageExt msg : msgs) {
        System.out.println(msg);
        System.out.println("收到的消息内容：" + new String(msg.getBody()));
      }
      // 返回消费成功的对象
      return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
    });
    // 6.启动消费者
    consumer.start();
    System.out.println("消费者已经启动");
```

3.2. 本地事务的实现
事务消息最关键的地方是生产者本地事务的实现，生产者本地事务实现 TransactionListener 接口，并实现该接口中的executeLocalTransaction方法和checkLocalTransaction方法。
其中，executeLocalTransaction 方法的作用是执行本地事务。它在生产者每次发送half消息的时候被调用，

1. 如果调用此方法返回LocalTransactionState.COMMIT_MESSAGE状态，则此消息会被消费者消费到。
2. 如果返回 LocalTransactionState.ROLLBACK_MESSAGE 状态，则此消息会被broker丢弃
3. 如果返回 LocalTransactionState.UNKNOW  状态，即中间状态，则broker会调用checkLocalTransaction方法进行回查，最多回查15次。
   
checkLocalTransaction方法的作用是检查本地事务， 它是生产者发送完所有消息的时候调用，主要是针对的是中间状态的消息进行调用。同样的如果调用此方法返回前面提到的三种状态，broker也会做出相同的处理。

```Java
public class TransactionListenerImpl implements TransactionListener {
  /**
   * 执行本地事务
   * 当事务half消息发送成功，这个方法将被执行
   * 事务的half消息是发到 RMQ_SYS_TRANS_OP_HALF_TOPIC 的topic中
   *
   * @param msg 消息
   * @param arg arg 自定义业务参数
   * @return {@link LocalTransactionState}
   */
  @Override
  public LocalTransactionState executeLocalTransaction(Message msg, Object arg) {
    String tags = msg.getTags();
    System.out.println("============执行executeLocalTransaction方法，;消息内容是="+new String(msg.getBody()));
    if (StringUtils.contains(tags, "tagA")) {
      return LocalTransactionState.COMMIT_MESSAGE;
    } else if (StringUtils.contains(tags, "tagB")) {
      return LocalTransactionState.ROLLBACK_MESSAGE;
    }
    return LocalTransactionState.UNKNOW;
  }
  /**
   * 检查本地事务
   * 回查本地事务状态，当half消息没响应时调用。
   *
   * @param msg 消息
   * @return {@link LocalTransactionState}
   */
  @Override
  public LocalTransactionState checkLocalTransaction(MessageExt msg) {
    String tags = msg.getTags();
    System.out.println("============执行checkLocalTransaction方法，;消息内容是="+new String(msg.getBody()));
    if (StringUtils.contains(tags, "tagC")) {
      return LocalTransactionState.COMMIT_MESSAGE;
    } else if (StringUtils.contains(tags, "tagD")) {
      return LocalTransactionState.ROLLBACK_MESSAGE;
    }
    return LocalTransactionState.UNKNOW;
  }
}
```

3.3. 事务消息的生产者
事务消息的生产者与普通消息的生产者最核心的区别是事务消息的生产者需要事务监听器，并且是调用sendMessageInTransaction 方法发送 half 消息。

```Java
//1.定义事务监听器
    TransactionListener transactionListener = new TransactionListenerImpl();
    //2.定义生产者
    TransactionMQProducer producer = new TransactionMQProducer("transaction_produce_group");
    producer.setNamesrvAddr("172.31.184.89:9876");
    //3.定义线程池
    ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(2, 5, 10, TimeUnit.SECONDS,
        new ArrayBlockingQueue<>(100), (runnable, executor) -> {
      BlockingQueue<Runnable> queue = executor.getQueue();
      try {
        queue.put(runnable);
      } catch (InterruptedException e) {
        throw new RuntimeException(e);
      }
    });
    //4.设置线程池
    producer.setExecutorService(threadPoolExecutor);
    //5.设置事务监听器
    producer.setTransactionListener(transactionListener);
    // 启动生产者
    producer.start();
    String[] tags = {"tagA", "tagB", "tagC", "tagD","tagE"};
    //发送10条half消息，消费者是收不到half消息的
    for (int i = 0; i < 10; i++) {
      Message message = new Message("TransactionTopic", tags[i % tags.length],
          "key" + i, ("飞哥测试事务消息" + tags[i % tags.length]+"_"+i).getBytes(StandardCharsets.UTF_8));
      TransactionSendResult transactionSendResult = producer.sendMessageInTransaction(message, null);
      System.out.println("本次发送的消息是=" + new String(message.getBody()));
      System.out.printf("%s%n", transactionSendResult);
      Thread.sleep(10);
    }
    System.out.println("==========所有消息发送完成======");
```

大概思路就是下单时，开启事务消息，然后开启本地事务，在数据库记录扩展字段里存放消息ID，最大支持9个消息ID。根据本地事务结果来判断，是需要回滚，还是继续让消费者可见。让消费者消费。
RocketMQ回查的时候查数据库，判断是否存在相应的消息ID。存在则消费，不存在则回滚。

```Java
@Override
    public void sendTransactionMessage() {

        if (CollectionUtils.isEmpty(messageEntities)) {
            //不需要发消息则退化为简单的本地事务提交
            doCommit();
            
        } else if (messageEntities.size() == 1) {
            //单个消息直接走普通事务消息发送接口,不需要用多线程的方式
            sendSingleTransactionMsg();
            
        } else {
            sendMultiTransactionMsgs();
        }


    }

    private void sendMultiTransactionMsgs() {
        Object lock = new Object();

        CountDownLatch messagesLatch = new CountDownLatch(messageEntities.size());
        CountDownLatch dbLatch = new CountDownLatch(1);

        final TransactionStatus[] transactionStatus = new TransactionStatus[]{TransactionStatus.Unknow};

        TransactionExecutor transactionExecutor = new TransactionExecutor();
        transactionExecutor.setTransactionExecutor((LocalTransactionExecuter) (msg, arg) -> {

            //加锁后把msgId打到订单上,防止多线程并发的时候覆盖
            synchronized (lock) {
                int maxMsgNum = messageEntities.size() > TpMessageHelper.MAX_MSG_NUM
                        ? messageEntities.size()
                        : TpMessageHelper.MAX_MSG_NUM;
                this.persistEntities.forEach(workUnit -> TpMessageHelper.addMessageIdIfIdMatch(msg.getKey(), msg.getMsgID(), workUnit.getSdo(), maxMsgNum));
                this.modifyEntities.forEach(workUnit -> TpMessageHelper.addMessageIdIfIdMatch(msg.getKey(), msg.getMsgID(), workUnit.getSdo(), maxMsgNum));
            }

            //打完msgId后计数器减少
            messagesLatch.countDown();

            //等db操作的计数器到0才进行二阶段的反馈
            try {
                log.info(String.format(String.format("dbLatch.await begin, msgid:%s", msg.getMsgID())));
                if (dbLatch.await(10, TimeUnit.SECONDS)) {
                    log.info(String.format("数据库操作完成, 等待到dbLatch,msgid:%s, transactionStatus[0]:%s",msg.getMsgID(), transactionStatus[0]));
                    return transactionStatus[0];
                }
                return TransactionStatus.Unknow;
            } catch (InterruptedException e) {
                //正常情况, 目前外部不会进行打断, 万一打断了, 返回unknow, 依赖mq的回查
                log.error(String.format("transaction execute error,persistEntities: %s , modifyEntities: %s , messages: %s !",
                        persistEntities.toString(), modifyEntities.toString(), messageEntities.toString()), e);
                return TransactionStatus.Unknow;
            }

        });

        List<Future> futures = new ArrayList<>();
        for (MessageEntity msg : messageEntities) {
            Future<TradeResponse<SendResult>> future = executorService.submit(TtlCallable.get(() -> {
                try {
                    TradeResponse<SendResult> sendResultTradeResponse = msgSendRepository.sendTransactionMsg(msg, transactionExecutor, null);
                    return sendResultTradeResponse;
                } catch (Exception e) {
                    mqLog.error(String.format("msgSendRepository send exception, message entity is : %s", msg), e);
                    return TradeResponse.ofFail("", "");
                }
            }));
            futures.add(future);
        }
        try {
            log.info(String.format("准备保存数据库!"));
            if (messagesLatch.await(5, TimeUnit.SECONDS)) {
                doCommit();
                transactionStatus[0] = TransactionStatus.CommitTransaction;
                log.info(String.format("数据库落库成功!"));
            } else {
                log.info(String.format("等待消息发送超时, 没有执行数据库保存!"));
                transactionStatus[0] = TransactionStatus.RollbackTransaction;
                throw new RepoException("保存数据库失败!");
            }
        } catch (InterruptedException e) {
            log.error(e.getMessage(), e);
            transactionStatus[0] = TransactionStatus.RollbackTransaction;
            throw new RuntimeException(e);
        } catch (Exception e) {
            log.error("执行数据库操作发生异常!", e);
            transactionStatus[0] = TransactionStatus.RollbackTransaction;
            throw e;
        } finally {
            dbLatch.countDown();
            log.info(String.format("dbLatch.countDown();"));
        }
    }

    private void sendSingleTransactionMsg() {
        TransactionExecutor singleTransactionExecutor = new TransactionExecutor();
        singleTransactionExecutor.setTransactionExecutor((LocalTransactionExecuter) (msg, arg) -> {

            try {
                this.persistEntities.forEach(workUnit -> TpMessageHelper.addMessageIdIfIdMatch(msg.getKey(), msg.getMsgID(), workUnit.getSdo()));
                this.modifyEntities.forEach(workUnit -> TpMessageHelper.addMessageIdIfIdMatch(msg.getKey(), msg.getMsgID(), workUnit.getSdo()));
                doCommit();
            } catch (Throwable e) {
                log.error("保存数据库变更失败", e);
                return TransactionStatus.RollbackTransaction;
            }

            return TransactionStatus.CommitTransaction;

        });
        try {
            msgSendRepository.sendTransactionMsg(messageEntities.get(0), singleTransactionExecutor, null);
        } catch (Exception e) {
            log.error("发送事务消息或执行数据库操作失败!", e);
            throw new RepoException("保存数据失败!");
        }
    }

    private void doCommit() {
        try {
            transactionTemplate.execute(transactionStatus -> {

                List<TradeOrderSDO> saveOrders = Nullable.stream(persistEntities)
                        .filter(workUnit -> Objects.nonNull(workUnit.getSdo()))
                        .map(EntityWorkUnit::getSdo).collect(Collectors.toList());
                tradeOrderUpdateRepository.batchSaveOrder(saveOrders);


//                tradeOperateRecordRepository.batchSaveTradeOperateRecord(tradeOperateRecords);

                List<TradeOrderSDO> updateOrders = Nullable.stream(modifyEntities)
                        .filter(workUnit -> Objects.nonNull(workUnit.getSdo()))
                        .map(EntityWorkUnit::getSdo).collect(Collectors.toList());
                tradeOrderUpdateRepository.batchUpdateTradeOrder(updateOrders);

                List<TradeOperateRecordSDO> tradeOperateRecords = Nullable.stream(tradeOperateRecordEntities)
                        .filter(workUnit -> Objects.nonNull(workUnit.getSdo()))
                        .map(EntityWorkUnit::getSdo).collect(Collectors.toList());
                TradeOperateRecordMsg tradeOperateRecordMsg = new TradeOperateRecordMsg();
                tradeOperateRecordMsg.setTradeOperateRecords(tradeOperateRecords);
                //发送mq
                if (CollectionUtils.isNotEmpty(tradeOperateRecords)) {
                    sendOperateRecordMq(tradeOperateRecordMsg);
                }
                return null;
            });
        } catch (Exception ex) {
            log.error("database commit error: {}", ex.getMessage(), ex);
            throw new RuntimeException(ex.getMessage(), ex);
        } finally {
            threadLocalWork.remove();
        }
    }
```

文章部分内容转载自：https://developer.aliyun.com/article/1558282