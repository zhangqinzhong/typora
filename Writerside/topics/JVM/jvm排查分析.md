# jstat

jstat命令命令格式：
jstat [Options] vmid [interval] [count]

> 例如：jstat -gc 30149 5000
>
> 说明：每5 秒一次显示进程号为30149的 java进成的 GC情况，结果如下图

![jstat](jstat.png)

命令参数说明：
Options，一般使用 -gcutil 或  -gc 查看gc 情况
pid，当前运行的 java进程号 
interval，间隔时间，单位为秒或者毫秒 
count，打印次数，如果缺省则打印无数次

Options 参数如下：
-gc：统计 jdk gc时 heap信息，以使用空间字节数表示
-gcutil：统计 gc时， heap情况，以使用空间的百分比表示
-class：统计 class loader行为信息
-compile：统计编译行为信息
-gccapacity：统计不同 generations（新生代，老年代，持久代）的 heap容量情况
-gccause：统计引起 gc的事件
-gcnew：统计 gc时，新生代的情况
-gcnewcapacity：统计 gc时，新生代 heap容量
-gcold：统计 gc时，老年代的情况
-gcoldcapacity：统计 gc时，老年代 heap容量
-gcpermcapacity：统计 gc时， permanent区 heap容量
————————————————

S0C：第一个幸存区的大小
S1C：第二个幸存区的大小
S0U：第一个幸存区的使用大小
S1U：第二个幸存区的使用大小
EC：伊甸园区的大小
EU：伊甸园区的使用大小
OC：老年代大小
OU：老年代使用大小
MC：方法区大小
MU：方法区使用大小
CCSC:压缩类空间大小
CCSU:压缩类空间使用大小
YGC：年轻代垃圾回收次数
YGCT：年轻代垃圾回收消耗时间
FGC：老年代垃圾回收次数
FGCT：老年代垃圾回收消耗时间
GCT：垃圾回收消耗总时间
单位：KB

Jvm 分析 jstack

netstat  -ntlp 查看java进程端口号

jstack -l 7 >1.txt 输出jstack 数据到文件

scp zqz@tmall:/home/zqz/1.txt /Users/zhangqinzhong/Downloads 下载服务器数据到本地
