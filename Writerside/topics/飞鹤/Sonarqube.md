## 一、规范要求

所有自建系统后端Java代码，上线前必须通过代码规范检查。需评审的代码，必须通过检查后方可做代码评审。SonarQube检查出的所有问题，如果不修复范围需要经过项目经理/技术经理确认。历史代码暂不做要求，各项目组自行安排历史代码修复计划

## 二、上传待检查代码

1. **编译项目；**

2. 在项目中的maven  settings.xml 配置文件下新增如下代码

3. ```xml
   <profiles>
   				<profile>
               <id>sonar</id>
               <activation>
                   <activeByDefault>true</activeByDefault>
               </activation>
               <properties>
                   <!-- 平台登录的账号的用户名
                         -->
                   <!-- SonarQube平台登录的账号的密码
                  <sonar.password>admin</sonar.password>-->
                   <!-- 全局配置的情况建议使用用户名密码的形式 -->
                   <sonar.login>65740ee698f1f4228dee7b01f10e08e35de5a6cb</sonar.login>
   
                   <!-- Optional URL to server. Default value is http://localhost:9000 -->
                   <sonar.host.url>
                       http://10.10.210.119:9000
                   </sonar.host.url>
               </properties>
           </profile>
   </profiles>
   
   <activeProfiles>
   		<activeProfile>sonar</activeProfile>
   </activeProfiles>	
   ```

   	# 启动命令 
   	mvn sonar:sonar --settings /Users/zhangqinzhong/IdeaProject/feihe/rainbow/settings-oms.xml

![img](1650608360126-4e5ccede-1b64-4d19-9f83-85b8a4b5ee90.png)

**注意：**

**初次使用请先上传master分支完整代码（需上传两次）。**

**后续使用请上传待评审或待上线代码所在的分支版本（不要上传与本次评审或上线无关的新代码）。**

**业务中台**

账号：ywzt

密码：ywzt@7721

token：65740ee698f1f4228dee7b01f10e08e35de5a6cb

## 三、查看检查结果

**SonarQube地址：**[**http://10.10.210.119:9000**](http://10.10.210.119:9000)

**访问地址，输入账号密码。**

**点击菜单栏项目—项目名称，如下图。**

![img](1650608360483-a106c65e-4fc7-4ae7-bc49-105960124c22.png)

**左侧是历史代码存在的问题，右侧是本次上传新代码存在的问题。**

![img](1650608360996-934488d9-8efd-4bc0-9c79-b330e0ccd43a.png)

**点击新增Bugs、新增漏洞、新增代码异味可查看具体问题列表，如下图。**

![img](1650608361564-f19f3e1b-4585-47d7-910a-70e1a82da041.png)

**点击列表问题查看具体代码，如下图。**

![img](1650608362034-fd70ea97-5c17-4366-8988-5c639cbdbe29.png)**代码中修复后，可把该问题标记为解决。标记为解决后，下次上传代码检查前不会再出现在列表中。**

![img](1650608362501-20fba0f4-eee0-41d9-868b-2fa0ff385bd0.png)

**代码修改完成后需再次上传代码检查，直至不存在新增Bugs、新增漏洞、新增代码异味。**

