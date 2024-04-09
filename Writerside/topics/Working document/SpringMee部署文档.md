# **相关组件**

 spring-boot

kafka（版本 2.7.0）

elasticsearch（版本 6.7）

mysql （版本 5.6 or 5.7）

canal服务器

etcd（版本3.4.13）



# **配置**

## **mysql**

a. 当前的canal开源版本支持5.7及以下的版本(阿里内部mysql 5.7.13, 5.6.10, mysql 5.5.18和5.1.40/48)

  b. canal的原理是基于mysql binlog技术，所以这里一定需要开启mysql的binlog写入功能，并且配置binlog模式为row.

```
[mysqld]  
log-bin=mysql-bin #添加这一行就ok  
binlog-format=ROW #选择row模式  
server_id=1 #配置mysql replaction需要定义，不能和canal的slaveId重复  
```

  数据库重启后, 简单测试 my.cnf 配置是否生效:

```
mysql> show variables like 'binlog_format';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
mysql> show variables like 'log_bin';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_bin       | ON    |
+---------------+-------+
```

  如果 my.cnf 设置不起作用,请参考:  https://stackoverflow.com/questions/38288646/changes-to-my-cnf-dont-take-effect-ubuntu-16-04-mysql-5-6  https://stackoverflow.com/questions/52736162/set-binlog-for-mysql-5-6-ubuntu16-4

  c. canal的原理是模拟自己为mysql slave，所以这里一定需要做为mysql slave的相关权限 

```
CREATE USER canal IDENTIFIED BY 'Canal_dev@2021';    
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';   
FLUSH PRIVILEGES; 
```

  

 针对已有的账户可通过grants查询权限：

```
show grants for 'canal' 
```



## **kafka**

1. 首先创建一个canal的topic：

bin/kafka-topics.sh --create --topic canal --partitions 3 --replication-factor 1 --bootstrap-server localhost:9092 







1. 设置一下自动创建的topic的属性

```
num.partitions=3
default.replication.factor=1
```



## **canal**

动手修改canal配置之前，先参考文章：[Canal v1.1.4版本避坑指南](https://www.cnblogs.com/throwable/p/13449920.html)



首先，修改canal的配置文件

```
vim conf/canal.properties

============================================
# serverMode
canal.serverMode = kafka
# mq
canal.mq.flatMessage = true
# binlog filter
canal.instance.filter.query.dcl = true
# parallelThreadSize
canal.instance.parser.parallelThreadSize = 16
#disable tsdb
canal.instance.tsdb.enable = false
# kafka
kafka.bootstrap.servers = 192.168.199.48:9092
kafka.acks = all
kafka.linger.ms = 200
kafka.max.in.flight.requests.per.connection = 1
kafka.retries = 3
# destinations
canal.destinations = dev_01
```



然后，修改instance的配置文件：

```
vim conf/{instance-name}/instance.properties

============================================
# slave id
canal.instance.mysql.slaveId=10032
# mysql config
canal.instance.master.address = 192.168.199.97:3306
canal.instance.dbUsername = laso_canal
canal.instance.dbPassword = Laso@canal_2021

# mq config
canal.mq.topic=canal
# hash partition config
canal.mq.partitionsNum=3
# hash by database.table
canal.mq.partitionHash=.*\\..*

# disable tsdb
canal.instance.tsdb.enable=false

# 如果不是阿里云RDS环境，还需要注释掉相关配置，否则需要补全这些配置
#canal.instance.rds.accesskey=
#canal.instance.rds.secretkey=
#canal.instance.rds.instanceId=
```



## **etcd**

启动命令：

**nohup etcd --name s1 --data-dir /usr/local/etcd/data/s1.etcd --listen-client-urls http://0.0.0.0:2379  --advertise-client-urls http://192.168.199.97:2379 --listen-peer-urls http://0.0.0.0:2380 --initial-advertise-peer-urls http://192.168.199.97:2380 --initial-cluster s1=http://192.168.199.97:2380 --initial-cluster-token tkn --initial-cluster-state new >/tmp/etcd.log 2>&1 &**



## **spring-mee**

写入DataSource的配置以及Handler的配置。