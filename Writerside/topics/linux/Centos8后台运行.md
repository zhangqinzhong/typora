# Centos8后台运行

```bash
nohup yarn install && yarn dev > chatgpt.log 2>&1 & 
```

**& 是一个描述符，如果1或2前不加&，会被当成一个普通文件。**

**1>&2 意思是把标准输出重定向到标准错误.**

**2>&1 意思是把标准错误输出重定向到标准输出。**

**&>filename 意思是把标准输出和标准错误输出都重定向到文件filename中**
