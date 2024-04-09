# MybatisGenerate插件使用指南

Rainbow项目中

com.alibaba.rainbow.core.dal.generator.MybatisGenerator#main



```xml
<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                connectionURL="jdbc:mysql://127.0.0.1:3306/test"
                userId="root"
                password="zqzmysql">
    <property name="nullCatalogMeansCurrent" value="true"/>
</jdbcConnection>
```



```properties
driverLocation=/Users/zhangqinzhong/.m2/repository/mysql/mysql-connector-java/8.0.30/mysql-connector-java-8.0.30.jar
model.package=com.alibaba.rainbow.core.dal.trade.model
mapper.package=com.alibaba.rainbow.core.dal.trade.mapper
sql.package=mapper.trade
javaModel.path=/Users/zhangqinzhong/IdeaProject/feihe/rainbow/rainbow-core/rainbow-core-dal/src/main/java
sql.path=/Users/zhangqinzhong/IdeaProject/feihe/rainbow/rainbow-core/rainbow-core-dal/src/main/resources
```



```properties

```
