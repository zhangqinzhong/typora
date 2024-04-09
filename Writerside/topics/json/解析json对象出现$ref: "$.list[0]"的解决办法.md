问题分析：
循环引用：当一个对象包含另一个对象时，fastjson就会把该对象解析成引用。引用是通过$ref标示的，下面介绍一些引用的描述
"$ref":".." 上一级
"$ref":"@" 当前对象，也就是自引用
"$ref":"$" 根对象
"$ref":"$.children.0" 基于路径的引用，相当于 root.getChildren().get(0)

解决方案：

fastjson提供了多种json转换方案，有兴趣的同学可以自己看看源码，这里我们可以采用禁止循环引用的方案：

String s = JSON.toJSONStringWithDateFormat(0,"yyyy-MM-dd HH:mm:ss",SerializerFeature.DisableCircularReferenceDetect);

其中：SerializerFeature.DisableCircularReferenceDetect就是禁止循环引用的方案，我们可以通过枚举类SerializerFeature来查看到底有多少种方式：
