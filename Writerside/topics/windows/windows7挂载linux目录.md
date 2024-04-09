# windows7挂载linux目录

windows挂载linux目录



首先操作linux

linux安装两个包nfs-utils和rpcbind：yum install -y nfs-utils rpcbind

编辑配置文件，允许共享主机IP：vim /etc/exports

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201229204210066.png" alt="image-20201229204210066" style="zoom:50%;" />

/home/suzhou/chip_scan

创建共享目录，给创建目录777权限：mkdir /home/nfstestdir
chmod 777 /home/nfstestdir

服务端启动rpcbind：systemctl start rpcbind

启动nfs：systemctl start nfs

设置开机自启动：systemctl enable nfs

关闭防火墙：systemctl stop firewalld





windows7操作如下：

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201229211220310.png" alt="image-20201229211220310" style="zoom:50%;" />

打开控制面板——程序——启用或关闭windows功能——点击NFS服务和适用于unix的windows子系统——确定

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201229211239811.png" alt="image-20201229211239811" style="zoom:50%;" />



<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201229211249799.png" alt="image-20201229211249799" style="zoom:50%;" />

cmd  

输入：mount 服务器端IP:/home/nfstestdir X: 

注意：X 代表windows 的盘符。大写。

显示 "命令已成功完成" 即可

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201229203800639.png" alt="image-20201229203800639" style="zoom:50%;" />



断开挂载，命令行输入：umount x: ,断开连接即可。



windows开机自启

若要开机自动挂载可以点击“计算机” → 点击“映射网络驱动器” → “输入网络共享文件路径“ → ”完成“

<img src="/Users/zhangqinzhong/Library/Application Support/typora-user-images/image-20201229211303180.png" alt="image-20201229211303180" style="zoom:50%;" />