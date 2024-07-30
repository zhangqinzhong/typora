# Java技术体系

## Java技术体系 

		从广义上讲，Kotlin、Clojure、JRuby、Groovy等运行于Java虚拟机上的编程语言及其相关的程序都属于Java技术体系中的一员。如果仅从传统意义上来看，JCP官方[1]所定义的Java技术体系包括了以 下几个组成部分:

*  Java程序设计语言
* 各种硬件平台上的Java虚拟机实现
* Class文件格式
* Java类库API
* 来自商业机构和开源社区的第三方Java类库

我们可以把Java程序设计语言、Java虚拟机、Java类库这三部分统称为JDK(Java Development Kit)，JDK是用于支持Java程序开发的最小环境，本书中为行文方便，在不产生歧义的地方常以JDK来代指整个Java技术体系[2]。可以把Java类库API中的Java SE API子集[3]和Java虚拟机这两部分统称为 JRE(Java Runtime Environment)，JRE是支持Java程序运行的标准环境。图1-2展示了Java技术体系所 包括的内容，以及JDK和JRE所涵盖的范围。

以上是根据Java各个组成部分的功能来进行划分，如果按照技术所服务的领域来划分，或者按照 技术关注的重点业务来划分的话，那Java技术体系可以分为以下四条主要的产品线:

* Java Card:支持Java小程序(Applets)运行在小内存设备(如智能卡)上的平台。

* <img src="java技术体系所包括的内容.png" alt="java技术体系所包括的内容" style="zoom:50%;" />

  																图1-2 Java技术体系所包括的内容

* Java M E(M icro Edition):支持Java程序运行在移动终端(手机、PDA)上的平台，对Java API 有所精简，并加入了移动终端的针对性支持，这条产品线在JDK 6以前被称为J2ME。有一点读者请勿 混淆，现在在智能手机上非常流行的、主要使用Java语言开发程序的Android并不属于Java M E。

* Java SE(Standard Edition):支持面向桌面级应用(如Windows下的应用程序)的Java平台，提 供了完整的Java核心API，这条产品线在JDK 6以前被称为J2SE。

* Java EE(Enterprise Edition):支持使用多层架构的企业应用(如ERP、MIS、CRM应用)的Java平台，除了提供Java SE API外，还对其做了大量有针对性的扩充[4]，并提供了相关的部署支持， 这条产品线在JDK 6以前被称为J2EE，在JDK 10以后被Oracle放弃，捐献给Eclipse基金会管理，此后被 称为Jakarta EE。

[1] JCP:Java Community Process，就是人们常说的“Java社区”，这是一个由业界多家技术巨头组成的 社区组织，用于定义和发展Java的技术规范。
[2] 本书将以OpenJDK/OracleJDK中的HotSpot虚拟机为主脉络进行讲述，这是目前业界占统治地位的 JDK和虚拟机，但它们并非唯一的选择，当本书中涉及其他厂商的JDK和其他Java虚拟机的内容时， 笔者会指明上下文中JDK的全称。

[3] Java SE API范围:https://docs.oracle.com/en/java/javase/12/docs/api/index.html。
[4] 这些扩展一般以javax.*作为包名，而以java.*为包名的包都是Java SE API的核心包，但由于历史原 因，一部分曾经是扩展包的API后来进入了核心包中，因此核心包中也包含了不少javax.*开头的包名。

