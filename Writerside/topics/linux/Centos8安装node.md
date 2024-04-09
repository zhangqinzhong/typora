# 升级node

```bash
# 查看当前版本
node -v
# 清理本地包缓存
npm cache clean -f
# 安装
npm i -g n
# 查看n是否安装成功
n -V
# 升级node
n stable // 把当前系统的 Node 更新成最新的 “稳定版本”
n lts // 长期支持版
n latest // 最新版
n 16.13.1 // 指定安装版本
# 手动刷新
hash -r
rehash

node -v
```

