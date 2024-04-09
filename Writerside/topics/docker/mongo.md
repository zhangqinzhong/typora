db.createUser({ user:'zhangqinzhong',pwd:'zhangqinzhong',roles:[ { role:'dbOwner', db: 'yapi'}]});

db.createUser({ user:'admin',pwd:'zhangqinzhong',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});



**Mongodb未授权访问漏洞修复**（为MongoDB添加认证）：

1、创建超级用户admin，授予在所有数据库上读写数据的权限

use admin

db.createUser({user:"admin",pwd:"123456",roles:["root"]})

2、查看用户集合

db.system.users.find()

3、验证用户

db.auth(“admin”, “123456”)

4、创建yapi数据库用户

use yapi

db.createUser({user:"root",pwd:"123456",roles:[{role:"dbOwner",db:"yapi"}]})

5、查看用户集合

use admin

db.system.users.find()

6、验证用户

use yapi

db.auth("root","123456")

**删除服务**

sc delete MongoDB

 

**删除指定用户**

db.dropUser(“user_name”)

 

**删除当前库所有用户**

db.dropAllUser()



