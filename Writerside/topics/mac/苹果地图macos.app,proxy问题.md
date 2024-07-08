# Apple Macbook地图.app 无法使用高德地图数据的问题

### 由于使用了Clashx proxy 导致地图app无法导航路线等问题。

解决办法是配置proxy规则。

打开Clashx的配置文件夹，找到当前正在使用的配置文件。编辑配置文件。

在rules下新增如下规则：

```yaml
- DOMAIN-SUFFIX,ls.apple.com,DIRECT
```



<img src="/Users/zhangqinzhong/Documents/typora/mac/images//image-20231101104742978.png" alt="image-20231101104742978" style="zoom:50%;" />

保存配置好的配置文件。点击Clashx的重载配置文件。

重启地图app，即可使用高德地图数据，并且可以规划路线导航。