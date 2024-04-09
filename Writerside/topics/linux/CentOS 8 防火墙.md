# Centos8

#### 查看防火墙状态

```shell
systemctl status firewalld
```

![image-20220915020039671](image-20220915020039671.png)

> active表示当前防火墙处于开启状态 inactive表示关闭状态

```shell
# 关闭防火墙
systemctl stop firewalld 
# 开启防火墙
systemctl start firewalld 
# 禁止防火墙自启动
systemctl disable firewalld
# 防火墙随系统开启启动
systemctl enable firewalld 
# 开放不同的端口 注意 加了--permanent参数的是永久新增 需要重新防火墙后生效
firewall-cmd --permanent --zone=public --add-port=80/tcp
# elasticsearch默认端口
firewall-cmd --permanent --zone=public --add-port=9200/tcp
# kibana默认端口
firewall-cmd --permanent --zone=public --add-port=5601/tcp
# 重启防火墙
firewall-cmd --reload
# 删除开放的端口
firewall-cmd --permanent --remove-port=80/tcp
# 查询所有开放的端口列表
firewall-cmd --list-ports 
```


切换到root用户

sudo su -



修改root 密码

passwd root



通过ssh命令登录

ssh <用户名>@<IP地址>



配置免密登录 

通过命令 ssh-keygen -t rsa  生成秘钥对 将id_rsa.pub 的内容复制到远端 /root/.ssh/authorized_keys 文件中

