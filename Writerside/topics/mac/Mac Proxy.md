# Mac Proxy

> 写一篇关于mac proxy port 的随机吧

先描述一下我出现的问题吧，以及后面会一点点分析解决。



我想在我的笔记本电脑上（老款 intel mac book pro 2018）用brew（mac电脑的一款包管理器）安装一些依赖。brew的安装参考其他博客。我已经装好brew了。

然后我想用brew安装python 等其他依赖报错如下：

```bash
Failed to connect to 127.0.0.1 port 7890: Connection refused
```

看到这个127.0.0.1 端口号7890我想到了是proxy的问题。

然后我查看电脑proxy的端口号。

在设置里就能看到或者查看自己的proxy软件的界面（如果有界面的话。）

在设置-> 网络-> WIFI -> 详细信息-> 就能找到

![image-20230418134743495](image-20230418134743495.png)

我的端口是63888，下面的https，socks也都是一样的端口。（是我自己改了proxy的端口。默认应该都是7890）

然后通过百度查到brew的命令,可以查看brew的配置

```bash
brew config
```

<img src="/Users/zhangqinzhong/Documents/typora/mac/images//image-20230418142237383.png" alt="image-20230418142237383" style="zoom:50%;" />

可以看到all_proxy,http_proxy,https_proxy 都走的是7890，但是我系统设置里的proxy端口是63888，那肯定不通。

至此有2种办法。

1. 修改proxy软件的端口。改成7890.
2. 修改brew的配置.

第一种肯定是能改的，但是由于7890跟我的某款软件端口号有冲突，所以我不能用7890端口。所以我尝试更改brew的proxy端口。

但是搜索了一圈还是没找到brew的配置文件。

最后是通过修改系统proxy端口实现的。通过查阅资料，知道brew是使用的系统级别的proxy。

放上修改方式。

首先查看本地的shell使用的是什么。使用命令：

```bash
echo $0
#或者
ps -ef | grep `echo $$` | grep -v grep | grep -v ps
```

输出

![image-20230419101046174](image-20230419101046174.png)

我本地用的是zsh。所以修改zsh的配置文件。文件所处位置是家目录。文件名是 **.zshrc **没有这个文件就新建一个。

编辑它填入如下内容：

```bash
# proxy
alias proxy='export https_proxy=http://127.0.0.1:63888 http_proxy=http://127.0.0.1:63888 all_proxy=socks5://127.0.0.1:63888'
alias unproxy="unset https_proxy; unset http_proxy; unset all_proxy;"
```

记得换成自己的proxy端口号。

然后再依次执行命令。

```bash
source ~/.zshrc
unproxy
proxy
```

然后再执行命令查看brew的proxy配置情况。

```bash
brew config
```

![image-20230419102054896](image-20230419102054896.png)

搞定。
