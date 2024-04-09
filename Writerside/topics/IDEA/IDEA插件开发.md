报错：

```
10:06:31: Executing 'runIde'...

> Task :compileKotlin UP-TO-DATE
> Task :compileJava UP-TO-DATE
> Task :patchPluginXml UP-TO-DATE
> Task :processResources UP-TO-DATE
> Task :classes UP-TO-DATE
> Task :instrumentCode UP-TO-DATE
> Task :postInstrumentCode
> Task :inspectClassesForKotlinIC UP-TO-DATE
> Task :jar UP-TO-DATE
> Task :prepareSandbox UP-TO-DATE

> Task :runIde
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by com.intellij.ui.JreHiDpiUtil to method sun.java2d.SunGraphicsEnvironment.isUIScaleEnabled()
WARNING: Please consider reporting this to the maintainers of com.intellij.ui.JreHiDpiUtil
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
2023-05-06 10:06:33,759 [   1309]   WARN - .intellij.util.EnvironmentUtil - can't get shell environment 
java.lang.RuntimeException: command [/bin/zsh, -l, -i, -c, '/Users/zhangqinzhong/.gradle/caches/modules-2/files-2.1/com.jetbrains.intellij.idea/ideaIC/2021.1/a810cecccb23abd2dbe737c1353af97b9a2ecb7f/ideaIC-2021.1/bin/printenv.py' '/var/folders/5z/g4clgnrs4wq2kbny4lvlp__40000gn/T/intellij-shell-env.13339292567116601766.tmp']
	exit code:127 text:0 out:env: python: No such file or directory
	at com.intellij.util.EnvironmentUtil$ShellEnvReader.runProcessAndReadOutputAndEnvs(EnvironmentUtil.java:285)
	at com.intellij.util.EnvironmentUtil$ShellEnvReader.doReadShellEnv(EnvironmentUtil.java:250)
	at com.intellij.util.EnvironmentUtil.getShellEnv(EnvironmentUtil.java:200)
	at com.intellij.util.EnvironmentUtil.lambda$loadEnvironment$0(EnvironmentUtil.java:107)
	at java.base/java.util.concurrent.CompletableFuture$AsyncSupply.run(CompletableFuture.java:1700)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1$1.run(Executors.java:668)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1$1.run(Executors.java:665)
	at java.base/java.security.AccessController.doPrivileged(Native Method)
	at java.base/java.util.concurrent.Executors$PrivilegedThreadFactory$1.run(Executors.java:665)
	at java.base/java.lang.Thread.run(Thread.java:834)
Warning: the fonts "Times" and "Times" are not available for the Java logical font "Serif", which may have unexpected appearance or behavior. Re-enable the "Times" font to remove this warning.
Warning: the fonts "Times" and "Times" are not available for the Java logical font "Serif", which may have unexpected appearance or behavior. Re-enable the "Times" font to remove this warning.
2023-05-06 10:06:34,581 [   2131]   WARN - i.mac.MacOSApplicationProvider - No URL bundle (CFBundleURLTypes) is defined in the main bundle.
To be able to open external links, specify protocols in the app layout section of the build file.
Example: args.urlSchemes = ["your-protocol"] will handle following links: your-protocol://open?file=file&line=line 
2023-05-06 10:06:34,641 [   2191]   WARN - j.internal.DebugAttachDetector - Unable to start DebugAttachDetector, please add `--add-exports java.base/jdk.internal.vm=ALL-UNNAMED` to VM options 
Warning: the fonts "Times" and "Times" are not available for the Java logical font "Serif", which may have unexpected appearance or behavior. Re-enable the "Times" font to remove this warning.
```

默认是使用 `python` 命令去获取 shell 环境变量的，即使我们使用 `xcode-select --install` 命令安装过开发者命令行工具，使用

```bash
ll /Library/Developer/CommandLineTools/usr/bin/python*
```

命令查看，并没有 `python` 命令.

 我们可以使用命令 

```bash
sudo ln -s /Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.9/bin/python3 /Library/Developer/CommandLineTools/usr/bin/python
```

 创建一个软链接，让 `python` 实际调用 `python3`。