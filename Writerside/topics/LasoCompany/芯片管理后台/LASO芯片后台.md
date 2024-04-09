# LASO芯片后台

## 需求背景

> https://shimo.im/docs/kdkkCqHvCGdcRCrD/read

## 一、384孔板列表功能要点

1. 列表展示已经录入到系统的384孔板。

2. 支持录入384孔板，录入方式为导入Excel并生成384孔板条形码编号。状态为待下单。

3. 支持批量下单给供应商.~~发送邮件给供应商。修改384孔板状态为待签收。（邮件是否需要手动ACK）~~

**下单以及获取供应商抽样质检数据方式如下：**

**下单：**

Plan1：和供应商对接API

Plan2：我们手动发邮件给供应商

Plan3：发邮件给供应商

**获取供应商抽样质检数据：**

Plan1：与供应商对接API。

Plan2：供应商发邮件给我们。

Plan3：开放特殊账号给供应商，登录我们芯片管理后台，上传固定格式抽样质检数据Excel。例如：上传的Excel文件名携带384孔板barcode编号。

**并且获取到供应商抽样质检数据后需要和我们数据库规定的标准进行判断，分子量和纯度只要有一个不合格就钉钉通知我们。**

4. 批量签收384孔板。扫描收到的384孔板上的条形码编号。由弹窗顶部选择签收的冰箱编号，冰箱层数或冰箱区域编号（待定），也可以修改具体某一版的签收冰箱编号，以及层数等。~~定时检测固定邮箱账户中的邮件，录入供应商质检报告数据。签收时校验是否已经存在供应商的质检报告数据，不存在则报警通知，并且在列表上呈现出此状态。（是否可以继续向下进行实验？）。~~

   签收时校验是否已经存在供应商的质检报告，不存在则钉钉通知我们。

**冰箱编号 下拉框可选**

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027103434449.png" alt="image-20201027103434449" style="zoom:50%;" />

5. 操作。勾选一个或多个384孔板，进入384孔板实验操作页面。

   （列表允许勾选多个384孔板同时进行实验。可以再来一个列表 专门用来勾选做实验的384孔板。）

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027104308387.png" alt="image-20201027104308387" style="zoom:50%;" />

## 1. <font color=red>抽样质检</font>

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201023150623981.png" alt="image-20201023150623981" style="zoom:50%;" />

1. 黄色代表供应商质检过的孔位。
2. 绿色代表我们要进行的默认质检

<font color=red>如果供应商质检了A1 ~ A12中的孔位，那我们默认质检的孔位往后顺延。实验员也可以自己指定要质检的孔位。</font>

**抽样质检的判断依据主要是分子量。**只要有一个分子量不达标，表示此次实验结束。钉钉报警通知我们。~~【并且发邮件给供应商进行重新下单，邮件内容为固定格式的Excel，写明具体的质检不合格的版号以及具体孔位。（系统也会记录重寄的版号以及孔位）。】~~

**纯度为次要判断依据**。纯度不达标>=3个孔位时，处理方式同上。低于三个继续进行实验。

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201026120321556.png" alt="image-20201026120321556" style="zoom:50%;" />

3. 可以点击查看具体孔位的详细信息。也可以进行操作。比如记录分子量，以及纯度。

## 2.<font color=red>生产环节</font>

目前还是以384孔板整体去做实验。不进行换板。

**支持同时选中多个孔位进行操作记录。（Shift+ctrl + L + 鼠标左键，具体实现方式以前端为准。）**

**点击具体孔位，展示孔位所有属性以及备注信息。**

抽样质检跟生产环节的实验步骤区分开。

步骤进度条标明当前正在进行的步骤，后面没做的步骤用灰色标识。

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027112307080.png" alt="image-20201027112307080" style="zoom:50%;" />

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027112335228.png" alt="image-20201027112335228" style="zoom:50%;" />

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201023210111786.png" alt="image-20201023210111786" style="zoom:50%;" />

## 3.<font color=red>全部质检</font>

全部质检，由我们进行，以整个孔板为单位进行。在具体芯片的某部分区域做实验。

选中384孔板，然后录入芯片编号，选择芯片涂抹区域。

多个384孔板可以对应一个芯片。

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027114325672.png" alt="image-20201027114325672" style="zoom:50%;" />

**96孔芯片**

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027114455005.png" alt="image-20201027114455005" style="zoom:50%;" />

一个孔板上所有孔位的状态一致（未质检、正在质检和质检完毕，这里可能需要分为更详细的状态记录，如stage1杂交完成、stage1扫描完成、stage1洗脱完成、stage2……），

**质检结果是以孔位为单位储存的。**

从384孔板的每一个孔里取一点出来，糅合在一起铺到芯片上做实验。记录芯片版号和384孔板对应关系。

需要记录芯片上具体的记录96格子芯片的，如：R01 C01

同生产环节，会进行很多步骤，每一步都会有一些操作。也会做一些记录。

#### 1. 杂交

#### 2. 扫描完成

#### 3. OmniScan 

扫描中间过程中 会有机器扫描的过程 在这个扫描等待过程中 不允许点下一步。倒计时展示提示。同生产环节清洗。

#### 4. 洗脱

#### 5. ......

#### N. 数据分析

全部质检完成的数据。有不合格的就通知我们自己人。数据分析结束，发钉钉通知具体的实验员。

可以退回到主界面进行其他操作。

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027133659227.png" alt="image-20201027133659227" style="zoom:50%;" />

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027133548699.png" alt="image-20201027133548699" style="zoom:50%;" />



384孔板A  --------芯片A

384孔板B  -------- 芯片B

## 二、芯片管理

【需要一个展示界面来库存及iso：

1. 对于每个芯片SKU（芯片类型），记录其基本性质，如sample数量、探针数量、所有探针编号（即oligo编号），库存量

1. 对于每张芯片，记录其属于哪种芯片SKU、解码状态、dLoc文件存放位置，生产人，生产日期

需要一个界面去展示我们成品芯片的库存。】



芯片库存展示！

没有任何操作功能。

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201027114831416.png" alt="image-20201027114831416" style="zoom:50%;" />

