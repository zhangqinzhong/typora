# JENV

mac jdk版本管理工具

Mac 安装jenv可以使用brew

```bash
brew install jenv
```

配置jenv

zsh配置方式：

```zsh
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc
```

bash配置方式：

```bash
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(jenv init -)"' >> ~/.bash_profile
```

使用方式为：

```bash
jenv add /Library/Java/JavaVirtualMachines/corretto-17.0.7/Contents/Home
```

查看已经安装的版本：

```bash
jenv versions
```

![image-20230525091151680](image-20230525091151680.png)

配置快速切换JKD版本

zsh配置方式：

```zsh
vim ~/.zshrc
```

```bash
#jenv start
export PATH="$HOME/.jenv/bin:$PATH"
eval "$(jenv init -)"

alias jdk8='jenv global 1.8'
alias jdk13='jenv global 13.0'
alias jdk17='jenv global 17.0'
alias jdk8_tmp='jenv local 1.8'
alias jdk13_tmp='jenv local 13.0'
alias jdk20_tmp='jenv local 17.0'

# jenv END
```

编辑完.zshrc配置文件别忘记使用source命令使它生效。

```bash
source ~/.zshrc
```

bash配置方式：

```bash
vim ~/.bash_profile
```

配置内容同理。

同样需要使配置文件生效。

```bash
source ~/.bash_profile
```

使用配置好的快捷命令：

```bash
jdk8
```

即可快速切换本机jdk

通过命令查看当前jdk版本

```bash
java -version
```

![image-20230525091545266](image-20230525091545266.png)

切换到jdk17, 键入快捷命令：

```
jdk17
```

通过命令查看当前jdk版本

```bash
java -version
```

![image-20230525091653258](image-20230525091653258.png)

至此完成。



补充一下信息。

当我使用 jenv管理JDK版本的时候，使用maven出现了问题。

由于已经采用了jenv管理JDK版本，我删除了电脑上原本的JAVA_HOME环境变量。

当我执行maven命令的时候报错如下：

```bash
The JAVA_HOME environment variable is not defined correctly,
this environment variable is needed to run this program.
```

maven配置如下：

```bash
#maven
M2_HOME=/Users/root/maven/apache-maven-3.6.2
PATH=$PATH:$M2_HOME/bin

export M2_HOME

alias maven='mvn'
#maven END
```

此时需要开启jenv的maven插件支持。

使用jenv的命令：

```bash
jenv enable-plugin maven
```

![image-20230704114153003](image-20230704114153003.png)

再次执行mvn -version即可。

注意，我在上面配置了maven的命令别名为 mvn。