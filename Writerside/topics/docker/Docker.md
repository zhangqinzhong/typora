Dockerfile

1、FROM

定制的镜像都是基于 FROM 的镜像。

```docker
FROM nginx:latest
```

2、ARG

用于构建镜像时的参数，ARG 设置的环境变量仅对 Dockerfile 内有效，也就是说只有 docker build 的过程中有效，构建好的镜像内不存在此环境变量。
构建命令 docker build 中可以用 --build-arg <参数名>=<值> 来覆盖。

ARG  VERSION=latest
FROM nginx:${VERSION}

3、ENV

设置环境变量，构建好的镜像内也会存在此环境变量。

ENV PACKAGE=nginx VERSION=latest

4、COPY

用于将本地文件拷贝到容器中。
格式：

```docker
COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
```

[–chown=:]：可选参数，用户改变复制到容器内文件的拥有者和属组。
<源路径>：源文件或者源目录，这里可以是通配符表达式，其通配符规则要满足 Go 的 filepath.Match 规则，如：

通配符
COPY hom* /mydir/
COPY hom?.txt /mydir/

相对路径，实际是 <WORKDIR>/relativeDir/
COPY test.txt relativeDir/

绝对路径
COPY test.txt /absoluteDir/

5、ADD

ADD 和COPY 的功能使用是类似的，比COPY多了解压功能。
当源文件格式为gzip, bzip2 以及 xz 时，会自动复制并解压到目标路径下。

6、RUN

用于执行命令

shell 格式：

RUN <命令>
RUN echo '<h1>Hello</h1>' >> /usr/share/nginx/html/index.html

exec 格式：

RUN ["可执行文件", "参数1", "参数2"]
RUN ["/bin/bash", "-c", "echo '<h1>World</h1>' >> /usr/share/nginx/html/index.html"]

RUN可以写多个，每一个指令都会建立一层，所以正确写法应该是以 && 符号连接命令，这样执行后就只创建 1 层镜像。

7、CMD

容器启动命令，为启动的容器指定默认要运行的程序，CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖；如果 Dockerfile 中如果存在多个 CMD 指令，仅最后一个生效。
CMD 在docker run 时运行。
RUN 是在 docker build时运行。

格式：

CMD <shell 命令> 
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
CMD ["<param1>","<param2>",...]  # 该写法是为 ENTRYPOINT 指令指定的程序提供默认参数

8、ENTRYPOINT

类似于 CMD 指令，但其不会被 docker run 的命令行参数指定的指令所覆盖，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序。
如果运行 docker run 时使用了 --entrypoint 选项，将覆盖 CMD 指令指定的程序。
如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。

格式：

ENTRYPOINT ["<executeable>","<param1>","<param2>",...]

9、VOLUME

用于定义匿名数据卷。默认会自动挂载到匿名卷，可以通过启动镜像时添加 -v 参数修改挂载点。

```docker
VOLUME ["/data"]
```

10、WORKDIR

用于指定工作目录，作为执行命令时的默认路径，RUN、CMD、ENTRYPOINT、ADD、COPY等命令都会在该目录下执行，可以通过docker run -w参数覆盖构建时所设置的工作目录。

```dockerfile
WORKDIR <工作目录路径>
```

11、EXPOSE

仅仅只是用于声明端口，在使用 docker run -P 时，会自动随机映射 EXPOSE 的端口。

```dockerfile
EXPOSE <端口1> [<端口2>...]
```

12、USER

用于指定执行后续命令的用户和用户组，其后的RUN、CMD、ENTRYPOINT命令都将使用该用户，可以通过docker run -u参数来覆盖所指定的用户。这个用户必须是事先建立好的，否则无法切换。

```dockerfile
USER <用户名>
USER <用户名>[:<用户组>]
```

13、LABEL

用来给镜像添加一些元数据信息。

```dockerfile
LABEL multi.label1="value1" multi.label2="value2" other="value3"
```

14、HEALTHCHECK

用于指定某个程序或者指令来监控 docker 容器服务的运行状态。

```dockerfile
HEALTHCHECK [参数] CMD <命令>：设置检查容器健康状况的命令
HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令
```

参数：

--interval=<间隔> ：两次健康检查的间隔， (默认: 30s)
--timeout=<时长> ：健康检查命令运行超时时间(默认: 30s)
--start-period=<延时>：容器初始化时，在此时间内检测失败不计入最大重试次数(默认: 0s)
--retries=<次数> ：当连续失败指定次数后，则将容器状态视为 unhealthy (默认: 3)

HEALTHCHECK --interval=5m --timeout=3s  CMD curl -f http://localhost/ || exit 1


官方文档：https://docs.docker.com/engine/reference/builder/
