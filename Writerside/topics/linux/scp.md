# scp

### 1. 从本地复制到远程

在本地服务器上将/root/lk目录下所有的文件传输到服务器43.224.34.73的/home/lk/cpfile目录下，命令为：

scp -r /root/lk root@43.224.34.73:/home/lk/cpfile

### 2. 从远程复制到本地

在本地服务器上操作，将服务器43.224.34.73上/home/lk/目录下所有的文件全部复制到本地的/root目录下，命令为：

scp -r root@43.224.34.73:/home/lk /root