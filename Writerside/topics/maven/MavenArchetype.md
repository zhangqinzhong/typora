为什么会写这篇文章，因为公司先在构建项目骨架都是用的 maven archetype ，身为一个上进的渣渣猿，自己还是有必要了解下这个东西的。

## **Archetype介绍**

Archetype 是一个 Maven 项目模板工具包。原型被定义为原始模式或模型，从中创建所有其他相同类型的东西。这些名称适合我们尝试提供一个系统，该系统提供生成Maven项目的一致方法。Archetype 将帮助作者为用户创建 Maven 项目模板，并为用户提供生成这些项目模板的参数化版本的方法。-- [摘自官网](https://maven.apache.org/archetype/index.html)

## **Archetype创建**

接下来，我们创建一个我们自己的 archetype.

### **1.创建一个maven项目**

这里需要引入 maven-archetype-plugin

完整pom文件如下

<groupId>com.kevin</groupId>     <artifactId>kevin-test-demo</artifactId>     <packaging>pom</packaging>     <version>1.0-SNAPSHOT</version>     <organization>         <name>kevin-养码青年</name>         <url>https://www.cnblogs.com/zhenghengbin/</url>     </organization>     <properties>         <jdk.version>1.8</jdk.version>         <maven.archetype.version>3.0.1</maven.archetype.version>     </properties>     <dependencies>         <dependency>             <groupId>org.apache.maven.plugins</groupId>             <artifactId>maven-archetype-plugin</artifactId>             <version>${maven.archetype.version}</version>         </dependency>     </dependencies>     <build>         <plugins>             <plugin>                 <groupId>org.apache.maven.plugins</groupId>                 <artifactId>maven-compiler-plugin</artifactId>                 <configuration>                     <source>${jdk.version}</source>                     <target>${jdk.version}</target>                 </configuration>             </plugin>         </plugins>     </build> </project>

### **2、生成archetype**

打开cmd窗口，在刚才的maven项目的根目录中运行maven命令：

mvn archetype:create-from-project

### **3、发布**

进入 target/generated-sources/archetype 目录。执行 mvn install,当然也可以发布到私服，这里我没有私服，就只安装到本地

到此，我们自己的archetype 模板已经创建成功

## **Archetype 使用**

使用很简单，我们要指定我们archetype信息

mvn archetype:generate -DarchetypeGroupId=com.kevin -DarchetypeArtifactId=kevin-test-demo-archetype -DarchetypeVersion=1.0-SNAPSHOT -DgroupId=com.kevin.productName -DartifactId=projectName -Dpackage=com.kevin.productName.projectName -Dversion=1.0.0 -DappName=projectName

** 注意事项 **

- 上面语句是一条完整语句，不能有空格
- -D 前面都有个空格
- -DarchetypeArtifactId 注意后面有archetype

其中最后的5个参数根据实际的情况进行修改，基本规范如下：

- groupId：项目工程的groupId；
- artifactId：项目工程的artifactId；
- package：项目工程的顶级package；
- version：项目工程的版本号；
- appName：项目工程打成包时的名字，当基于tomcat插件进行调试时，此名称也作为ContextPath名称。

## **总结**

使用 archetype 构建项目，不用让我们在添加各种 pom 文件或者 copy 代码。构建项目骨架简单迅速。 玩的开心！