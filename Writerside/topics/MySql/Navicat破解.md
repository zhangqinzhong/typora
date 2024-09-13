# Navicat破解

> Navicat Premium 16

一、卸载原有navicat（没装过的跳过这一步）

1. 控制面板里卸载原有navicat
2. 找一下原来navicat的安装目录 删干净
3. 删除Navicat的注册表信息
   1. 按下Win+R键，然后输入`regedit`并按Enter键来打开注册表编辑器。
   2. 在注册表编辑器中，导航到以下路径：`计算机\HKEY_CURRENT_USER\Software\PremiumSoft`。
   3. 在这个路径下，你会找到与Navicat相关的注册表项。右键点击`PremiumSoft`文件夹，然后选择’删除’来删除整个文件夹。
   4. 检查`\HKEY_CURRENT_USER\SOFTWARE\Classes\CLSID下`的`Info`文件夹，虽然通常在卸载后会被自动删除，但还是建议手动确认并删除。

二、官网下载最新版Navicat，下载安装完不要打开！

Navicat官网中文版下载地址：https://www.navicat.com.cn/download/navicat-premium

官网下载地址默认下载的都是最新版本，本教程使用的就是最新的 Navicat Premium 16 V16.0.11 中文版64位 安装包，安装包和激活工具已经放到网盘了，需要的自取。

三、使用注册机激活

1. 先打开注册机程序，Patch显示的目录为Navicat的安装目录【C:\Program Files\PremiumSoft\Navicat Premium 16】 请确实是否正确，如果不正确请选择到你的Navicat的安装目录。
然后勾选 HOSTS(该选项会修改hosts文件，添加 `127.0.0.1 activate.navicat.com` 达到屏蔽Navicat激活联网) , 然后点击 Patch! 按钮，弹框如下界面代表成功。

![image.png](img_1.png)

2. 使用注册机生成序列号和激活码

Version 选择 v16， Production 选择 Premium， Language 选择 Simplified Chinese， 点击 Generate! 按钮，生成序列号

断开网络(如果你第1步勾选了HOSTS，且HOSTS被修改成功了，可直接打开Navicat)打开 Navicat Premium 16 把注册机生成的序列号填写到Navicat中

![image.png](img_2.png)

3. 点击 激活，一般来说在线激活肯定会失败，这时候Navicat会询问你是否手动激活，直接选吧。

![image.png](img_3.png)

4. 在 手动激活 窗口你会得到一个请求码，复制它并把它粘贴到注册机里的 Request Code 里，
UserName 和 Organization 你可以自定义下，我这里就默认了，可以请随便填写，但不要太长！
然后点击 注册机的 Generate Activtion Code!按钮，生成 Activtion Code，把 Activtion Code 粘贴到 Navicat 的 激活码 输入框中点击 激活 即可大功告成！！！

![image.png](img_4.png)

5. 点击 激活按钮 ，弹框提示 Navicat Premium 现已激活。

![image.png](img_5.png)

6. 激活信息如下：

![image.png](img_6.png)