# Web唤起本地应用程序

## 例如：在任意浏览器输入 tencent:// 既可唤起QQ

windows参考文献：

https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/platform-apis/aa767914(v=vs.85)

用网页调用本地应用程序的思路是，先进行注册表注册自定义一个URL Protocol协议，再利用URL Protocol实现网页调用本地应用程序。

1.先写一个注册表文件，将其保存为.reg后缀的注册表执行文件：


Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\LasoWebPrint] 

"URL Protocol"="C:\\Program Files (x86)\\LasoWebPrint\\LasoPrint.exe” 

@="LasoPrintProtocol” 

[HKEY_CLASSES_ROOT\LasoWebPrint\DefaultIcon] 

@="C:\\Program Files (x86)\\LasoWebPrint\\LasoPrint.exe,1” 

[HKEY_CLASSES_ROOT\LasoWebPrint\shell] 

[HKEY_CLASSES_ROOT\LasoWebPrint\shell\open] 

[HKEY_CLASSES_ROOT\LasoWebPrint\shell\open\command] 

@="\"C:\\Program Files (x86)\\LasoWebPrint\\LasoPrint.exe\" \"%1\""


一行行来解释：

（1）表示注册表工具的版本信息；

（2）PWFileVersion表示的时注册表的HKEY_CLASSES_ROOT下新增一个PWFileVersion树（理解为在HKEY_CLASSES_ROOT下新增一个文件夹就可以了）

（3）你在网页中要调用打开的程序绝对路径，记得一定要是exe文件

（4）协议名称，可以是任意字符串，后面不会用到

（5）在PWFileVersion下新增一个分支，不用管

（6）地址和（3）中保持一致，1照抄

（7）（8）（9）和（5）一样，新增分支而已

（10）向要调用的程序内传递参数。前面的地址与（3）保持一致，后面的%1表示参数。敲黑板，这里面的/千万不要有所遗漏！本人在这个坑上蹲了很久- -；

运行reg文件，进行注册表注册。

这时候在浏览器输入：

pwfileversion://即可调用该程序

[pwfileversion://argument](pwfileversion://argument)随便什么字符串，即可将参数传入该程序

扩展：

WebAssembly前端技术 

下面链接可以让Win95和Win2000跑在浏览器里：

https://bellard.org/jslinux/vm.html?url=https://bellard.org/jslinux/win2k.cfg&mem=192&graphic=1&w=1024&h=768

https://win95.ajf.me/

（太狠了）

github ：  https://github.com/wangzuohuai/WebRunLocal

参考：

[利用URL Protocol实现网页调用本地应用程序](http://blog.csdn.net/zssureqh/article/details/25828683)

[从网页Web上调用本地应用程序（.jar、.exe）的主流处理方法](http://blog.csdn.net/fkepgydhbyuan/article/details/52918163)