# maven archetype

为什么会写这篇文章，因为公司先在构建项目骨架都是用的 maven archetype ，身为一个上进的渣渣猿，自己还是有必要了解下这个东西的。

# 使用我们的maven archetype生成项目模板

# 方式一（推荐）：

使用maven命令行：

mvn archetype:generate -DgroupId=<GROUP_ID> -DartifactId=<ARTIFACT_ID> -Dpackage=<BASE_PACKAGE> -Dproject=<PROJECT_NAME> -DarchetypeGroupId=com.lasogene.tools -DarchetypeArtifactId=laso-ms-archetype -DarchetypeVersion=1.0.2-SNAPSHOT -U



将需要生成的项目的groupId、artifactId、package和project分别替换上述命令中的<GROUP_ID>、<ARTIFACT_ID>、<BASE_PACKAGE>、<PROJECT_NAME>后，运行命令即可。

**自定义属性package和project，其中package指的是生成项目的基础包名，project是项目在bitbucket中的名称。**



示例：

mvn archetype:generate -DgroupId=com.laso.chip -DartifactId=laso_chip_manager -Dpackage=com.laso.chip -Dproject=laso_chip_manager -DarchetypeGroupId=com.lasogene.tools -DarchetypeArtifactId=laso-ms-archetype -DarchetypeVersion=1.0.2-SNAPSHOT -U



## **Archetype介绍** {id="archetype_1"}

Archetype 是一个 Maven 项目模板工具包。原型被定义为原始模式或模型，从中创建所有其他相同类型的东西。这些名称适合我们尝试提供一个系统，该系统提供生成Maven项目的一致方法。Archetype 将帮助作者为用户创建 Maven 项目模板，并为用户提供生成这些项目模板的参数化版本的方法。-- [摘自官网](https://maven.apache.org/archetype/index.html)

## **Archetype创建** {id="archetype_2"}

接下来，我们创建一个我们自己的 archetype.

### **1.创建一个maven项目**

这里需要引入 maven-archetype-plugin

完整pom文件如下
```xml

<project>
    <groupId>com.kevin</groupId>
    <artifactId>kevin-test-demo</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <jdk.version>1.8</jdk.version>
        <maven.archetype.version>3.0.1</maven.archetype.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-archetype-plugin</artifactId>
            <version>${maven.archetype.version}</version>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${jdk.version}</source>
                    <target>${jdk.version}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

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