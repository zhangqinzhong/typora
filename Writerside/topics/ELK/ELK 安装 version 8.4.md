# 虚拟机centos 8.4安装ELK 8.4版本

## 下载ElasticSearch以及kibana安装包

> 官方网址：
>
> https://www.elastic.co/cn/downloads/elasticsearch
>
> https://www.elastic.co/cn/downloads/kibana

##### 将下载好的文件传输到虚拟机。

![1](1.png)

##### 登录到虚拟机，略

```shell
ssh centos
```

##### 新建一个目录用于存放压缩包 留着备用

```shell
mkdir compressed_package
```

##### 移动压缩包到压缩包目录

```shell
mv elasticsearch-8.4.0-linux-x86_64.tar.gz kibana-8.4.0-linux-x86_64.tar.gz compressed_package/
```

![3](3.png)

##### 分别解压到指定目录

```shell
tar -zxvf elasticsearch-8.4.0-linux-x86_64.tar.gz -C ../
tar -zxvf kibana-8.4.0-linux-x86_64.tar.gz -C ../
```

![2](2.png)

##### 设置文件所有者，由于elasticsearch高版本要求非root用户启动 所以需要切换一个用户

##### 创建用户 略 

##### 我这里新建的用户叫 **parallels** {id="parallels_1"}

```shell
chown -R parallels:parallels elasticsearch-8.4.0/ kibana-8.4.0/
```

![image-20220831173557073](image-20220831173557073.png)

##### 查看防火墙状态

```shell
systemctl status firewalld
```

![image-20220915020121326](image-20220915020121326.png)

##### 打开防火墙端口号 后面方便宿主机访问

```shell
firewall-cmd --permanent --zone=public --add-port=80/tcp
# elasticsearch默认端口
firewall-cmd --permanent --zone=public --add-port=9200/tcp
# kibana默认端口
firewall-cmd --permanent --zone=public --add-port=5601/tcp
```

##### 切换用户到parallels

```shell
su - parallels
```

##### 在项目路径下执行启动命令：

```shell
./bin/elasticsearch
```

##### 此时elasticsearch会启动 并且在控制台输出内容，注意如下内容: {id="elasticsearch_1"}

```
✅ Elasticsearch security features have been automatically configured!
✅ Authentication is enabled and cluster connections are encrypted.

ℹ️ Password for the elastic user (reset with `bin/elasticsearch-reset-password -u elastic`):
  LoRfruz_ZXlZvX+lWCDi

ℹ️ HTTP CA certificate SHA-256 fingerprint:
  4bfc01c24639bc6e68e629cc7d0ceb6c797104bd8940a48f86e9879cbc3457dd

ℹ️ Configure Kibana to use this cluster:
• Run Kibana and click the configuration link in the terminal when Kibana starts.
• Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjQuMCIsImFkciI6WyIxMC4yMTEuNTUuNDo5MjAwIl0sImZnciI6IjRiZmMwMWMyNDYzOWJjNmU2OGU2MjljYzdkMGNlYjZjNzk3MTA0YmQ4OTQwYTQ4Zjg2ZTk4NzljYmMzNDU3ZGQiLCJrZXkiOiJmMEZoTllNQnZmdkNlNG1Oay1heTozNWxTLXhWclF0eXBHTzZxd3RSaUp3In0=

ℹ️ Configure other nodes to join this cluster:
• On this node:
  ⁃ Create an enrollment token with `bin/elasticsearch-create-enrollment-token -s node`.
  ⁃ Uncomment the transport.host setting at the end of config/elasticsearch.yml.
  ⁃ Restart Elasticsearch.
• On other nodes:
  ⁃ Start Elasticsearch with `bin/elasticsearch --enrollment-token <token>`, using the enrollment token that you generated.
```

##### 通过命令可以修改Elasticsearch的密码：

```shelll
bin/elasticsearch-reset-password -u elastic
```

![image-20220913152337876](image-20220913152337876.png)

##### 重置后的密码：e88Qe=bDg7mAPbIdSeIM

##### 使用浏览器访问elastic，测试是否正常启动

![image-20220913152743204](image-20220913152743204.png)

##### 可以通过如下命令重新生成token 或 修改密码：(注：部分命令需要在elasticsearch运行状态下)

```shell
You can complete the following actions at any time:
Reset the password of the elastic built-in superuser with
'bin/elasticsearch-reset-password -u elastic'.

Generate an enrollment token for Kibana instances with
'bin/elasticsearch-create-enrollment-token -s kibana'.

Generate an enrollment token for Elasticsearch nodes with
'bin/elasticsearch-create-enrollment-token -s node'.
```

##### Run as a daemon 以守护方式运行

```shell
./bin/elasticsearch -d -p pid
```

##### 接下来操作kibana

```shell
#执行命令重新生成elasticsearch token
bin/elasticsearch-create-enrollment-token -s kibana
```

![image-20220915005306517](image-20220915005306517.png)

##### 复制token到剪贴板待会儿会用到

##### 启动kibana之前先简单配置一下kibana 方便我们在宿主机访问kibana

```shell
server.port: 5601
server.host: "127.0.0.1"
# 国际化
i18n.locale: "zh-CN"
```

##### 启动kibana

```shell
./bin/kibana
```

![image-20220915014528850](image-20220915014528850.png)

##### 访问控制台输出的链接打开kibana web端

![image-20220915014635564](image-20220915014635564.png)

##### 粘贴刚才的token令牌给kibana

注：如果出现`Generate a new enrollment token or configure manually.` 需要重新生成elasticsearch的授权token

```shell
bin/elasticsearch-create-enrollment-token -s kibana
```

![image-20220915014718110](image-20220915014718110.png)

![image-20220915014838890](image-20220915014838890.png)

![image-20220915014853375](image-20220915014853375.png)

##### 输入elasticsearch的账号和密码 注意是es的，没记密码的重置下es的密码就行

```shell
bin/elasticsearch-reset-password -u elastic
```



![image-20220915014955662](image-20220915014955662.png)

##### 点自己浏览，瞅瞅，就进入home页了

![image-20220915015630560](image-20220915015630560.png)

##### 点击左侧导航栏，滑动到最下面，点击开发工具就可以使用命令操作es了

![image-20220915015726521](image-20220915015726521.png)

