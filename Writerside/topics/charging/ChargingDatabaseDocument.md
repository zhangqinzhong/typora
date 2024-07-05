# Charging Database Document

> Test Server Database

```yaml
datasource:
  url: 101.200.86.164:3306/charging
  username: zhang
  password: ZhangAlibabaMysql9.
```

| Table               | Comment     |
|:--------------------|:------------|
| alarm\_record       | 警报记录表       |
| alarm\_set          | 警报枚举表       |
| business\_manage    | 商家管理表       |
| card\_manage        | 储值卡管理表      |
| charging\_manage    | 充电站点管理表     |
| charging\_port      | 充电终端管理表     |
| charging\_station   | 充电站点详情表     |
| consumption\_record | 用户消费记录表     |
| control\_record     | 充放电控制记录表    |
| install\_package    | 安装包记录表      |
| invoice\_manage     | 发票管理记录表     |
| lion\_menu          | 权限记录表       |
| lion\_role          | 后台用户角色表     |
| lion\_role\_menu    | 后台角色权限关联表   |
| lion\_user          | 后台用户表       |
| lion\_user\_role    | 后台用户角色关联表   |
| monitor\_center     | 终端监视中心记录表   |
| operator\_manage    | 操作记录记录表     |
| order\_manage       | C端用户订单记录表   |
| platform\_blacklist | C端平台黑名单     |
| platform\_user      | C端平台用户表     |
| recharge\_record    | C端用户充值记录表   |
| repair\_manage      | 终端维护管理表     |
| site\_manage        | 站点管理用户表     |
| upgraded\_manage    | 升级终端设备管理记录表 |
