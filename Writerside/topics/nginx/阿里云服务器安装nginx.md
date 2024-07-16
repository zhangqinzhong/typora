# 阿里云服务器安装nginx 配置https等

Nginx相关介绍不在本篇赘述。直接步入正题。

登录阿里云服务器

1. 新建nginx安装目录。并且切换到该目录下

```Bash
mkdir /usr/local/nginx

cd /usr/local/nginx
```
2. 使用yum 下载wget下装工具，使用wget下装nginx包

```Bash
yum -y install wget
```

3. 下载nginx源文件

```Bash
wget http://nginx.org/download/nginx-1.20.2.tar.gz
```

4. 解压

```Bash
tar -zxvf nginx-1.20.2.tar.gz 
```

5. 下载nginx需要的其他相关依赖

```Bash
yum install -y gcc pcre-devel openssl openssl-devel zlib-devel compat-libstdc++-33 libfreetype.i686 libXext.x86_64 pcre-devel
```

6. 编译安装 进入刚才解压后的目录里执行

<warning>
如果想支持ssl，https 编译的时候需要加参数

```Bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
```
</warning>


```Bash
# 切换路径
cd nginx-1.20.2

# 此时的路径为 /usr/local/nginx/nginx-1.20.2
# 普通编译安装 
./configure

# 增加ssl模块支持 看具体需求
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module

# 安装
make

# install
make install
```

7. 启动nginx

```Bash
# 切换到nginx执行启动文件的目录
cd /usr/local/nginx/sbin

# 运行
./nginx
```

8. 可以通过`ps aux | grep nginx`检查nginx的进程

## nginx其他相关问题记录

一. 防火墙问题

<include from="CentOS8防火墙.md" element-id="防火墙"/>

如果防火墙是开着的，可以选择关闭，也可以选择放行相关端口。看具体需要。
`nginx`需要放行`80`端口。

二. 阿里云安全组问题

`80`端口需要放行。`443`端口是配置`https`需要的端口

![阿里云ECS安全组.png](阿里云ECS安全组.png)

三. Nginx配置https

出现的问题：

> Nginx: [emerg] the “ssl“ parameter requires ngx_http_ssl_module in /usr/local/nginx/conf/nginx.conf

原因就是编译安装nginx的时候没有安装https模块。

需要重新编译`nginx`

```Bash
# 查看nginx 的参数
/usr/local/nginx/sbin/nginx -V

nginx version: nginx/1.20.2
...
configure arguments: 
```

configure arguments: 为空。说明不支持ssl

重新编译。替换现有的nginx

```Bash
# 切换路径
cd /usr/local/nginx/nginx-1.20.2

# 编译添加ssl模块支持
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module

# 编译
make
```

关闭nginx服务。

可以尝试多种关闭方式。
```Bash
# 使用systemctl（适用于使用systemd的系统，如最新的Ubuntu、Debian、CentOS版本）
sudo systemctl stop nginx
 
# 使用service（旧的SysVinit命令，适用于旧版本的系统）
sudo service nginx stop
 
# 使用init.d（另一种旧的SysVinit命令）
sudo /etc/init.d/nginx stop

# 通过nginx命令发送信号
sudo nginx -s stop

# 找到nginx主进程号
ps -ef | grep nginx
 
# 使用kill命令发送QUIT信号
sudo kill -s QUIT <master_process_id>
```

使用重新编译的nginx覆盖原有的nginx
```Bash
# 覆盖
cp /usr/local/nginx/nginx-1.20.2/objs/nginx /usr/local/nginx/sbin/

# 重新加载nginx
/usr/local/nginx/sbin/nginx -s reload
```




