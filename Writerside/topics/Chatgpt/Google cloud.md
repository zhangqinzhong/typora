# 免费|申请谷歌云服务器

免费试用申请链接地址：

```tiki wiki
https://cloud.google.com/free/
```

## **前提准备**

已经注册了谷歌账号；

有visa等双币信用卡；

![image-20230407100731958](image-20230407100731958.png)

点击后登录谷歌账号。

填写账号信息

![申请 Google Cloud谷歌云](c9a46a1eca45668a610bc558ad8a13b7-1024x689.png)

国家/地区 选择美国，与自己的proxy节点保持一致。

第二个选项：下面哪一项与您的组织或需求最相符？

选择 个人项目即可

![申请 Google Cloud谷歌云](32ed2c8b19064d1f23dc966766896217.png)

有这一步就正常填写，没有就往下继续

![申请 Google Cloud谷歌云](238f324b1f9d79458d181de1b36b4f4d.png)

创建付款资料，需要真实美国地址，通过如下地址获取。

```
https://haoweichi.com 
```

![image-20230407102043278](image-20230407102043278.png)

填写国内VISA双币卡即可。

安全码为信用卡背后白色字体后三位。

继续下一步。

![申请 Google Cloud谷歌云](5d2fda4a62564525c54f087002c9689f-1024x926.png)

申请通过后即可进入google cloud控制台。

您的免费试用含 $300 赠金，可在未来 90 天内使用。

## Google Cloud 谷歌云服务器 配置

Google Cloud谷歌云申请成功之后第一件事需要做的就是配置谷歌云，首先创建一个虚拟机实例，安装你需要的操作系统，Google Cloud谷歌云免费用户不支持Windows server系统的安装，除此之外Linux系列的操作系统都没问题，例如CentOS，Ubuntu，Debian等。

### Google Cloud Platform 创建虚拟机实例

在Google Cloud谷歌云平台左侧的导航菜单中，点击计算下面的Compute Engine菜单，选择虚拟机实例。

![Google Cloud Platform 创建虚拟机实例](7a0b9f38925d2e1808389ce6768ad0a2-1024x837.png)

初次访问，需要启用Compute Engine API，点击启用。

![Google Cloud Platform 创建虚拟机实例](d5796f5e358200894d66183c395cadad-1024x730.png)

点击蓝色按钮创建实例。

![Google Cloud Platform 创建虚拟机实例](9bb06a3538711fbcba1644a67767c701-1024x533.png)

下面进入到虚拟机实例的创建，你可以选择虚拟机实例的所在区域和虚拟机硬件配置，右面是虚拟机的每月估算费用。如何选择区域和可用区，参考：https://cloud.google.com/compute/docs/regions-zones

![Google Cloud Platform 创建虚拟机实例](6cdb0c298065ba24371ce632956e3611-1024x622.png)

首选地区中国台湾节点，其次是中国香港节点，日本节点和新加坡节点。亚太地区：（联通电信速度快）（推荐使用日本东京、中国台湾、中国香港）

虚拟机实例的区域选择完毕后，机器配置默认通用的E2系列就可以，如果你需要更高的配置，可以在机器配置中选择，最高支持160个vCPU和3.75TB内存。

机器配置选择完毕后，进行虚拟机实例的操作系统安装，在启动磁盘选项中点击更改。

![Google Cloud Platform 创建虚拟机实例](50b7c685a2fab4247b82be88dabf51a9.png)

在默认的公共映像下面选择安装操作系统，操作系统包含比较流行的CentOS，Debian，Ubuntu等。

推荐CentOS

![Google Cloud Platform 创建虚拟机实例](2f9ed3fc1033122f010cb456468b51f7-1024x740.png)

![Google Cloud Platform 创建虚拟机实例](e56c4630e1dd1cda998c23dac1138ce3.png)

其他的选项自己看着选。

虚拟机实例基本配置已经全部配置完毕，接下来点击最下面的蓝色按钮，创建虚拟机实例。

![image-20230407103425206](image-20230407103425206.png)

可以点击如图ssh 在web页面中登录google 虚拟机。

如果有这个界面就正常登录谷歌账号。

![image-20230407110354099](image-20230407110354099.png)

进入后 键入cd /etc/ssh 进入ssh目录

![image-20230407110510800](image-20230407110510800.png)

通过如下命令编辑sshd_config 文件

```bash
sudo vim sshd_config
```

![image-20230407110940408](image-20230407110940408.png)

找到这三个配置 改为yes。

保存。

然后输入 reboot 回车重启实例。

接下来配置ssh免密登录。

![image-20230407103705304](image-20230407103705304.png)

往下滑 找到虚拟机元数据。

![image-20230407103741648](image-20230407103741648.png)

点击SSH密钥，点击修改，添加本地的公钥。

比如我的笔记本电脑是mac book pro。

在终端中进入家目录。再进入 .ssh 目录下。

![image-20230407104531115](image-20230407104531115.png)

本地的公钥在 家目录的 .ssh 目录下。

查看公钥：**cat id_rsa.pub** 或者 **vim id_rsa.pub**

如果没有这个目录，或者没有公钥就通过如下命令生成。

```bash
ssh-keygen
```

- 输入命令后接着会确认存放公钥的地址，默认就是上面说的路径，直接enter键确认
- 接着会要求输入密码和确认密码，直接enter键确认

完成后复制 id_rsa.pub里的公钥到google cloud 元数据中。

继续配置公钥。

![image-20230407105359464](image-20230407105359464.png)

在虚拟机实例中，点击具体的实例。

![image-20230407105521930](image-20230407105521930.png)

点击修改。

![image-20230407105621308](image-20230407105621308.png)

往下滑找到SSH密钥，再次将本地的公钥复制到这里。

配置完成后在本地机器执行如下命令。

```bash
# 172.16.3.2 为实例公网IP
ssh-copy-id root@172.16.3.2
```

配置完成后

```bash
ssh root@172.16.3.2
```

还可以配置本地便捷登录

![image-20230407111735193](image-20230407111735193.png)

本地编辑 config 文件。

![image-20230407111925543](image-20230407111925543.png)

* Host：随便填 要便捷登录的名称
* HostName：google cloud 虚拟机实例公网IP
* Port：端口号默认22
* User：root

配置完成后。

终端连接测试。

```bash
ssh google
```

登录成功。
