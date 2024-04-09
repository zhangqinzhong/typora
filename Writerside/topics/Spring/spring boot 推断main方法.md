# SpringBoot源码解析之main方法推断

 

SpringBoot首先会推断应用类型。在推断出应用类型之后，SpringBoot又进行了main方法的推断。

在进行main方法的推断时，主要使用了堆栈信息一层层的判断，来获得main方法。具体源代码如下：

```html
private Class<?> deduceMainApplicationClass() {
		try {
			StackTraceElement[] stackTrace = new RuntimeException().getStackTrace();
			for (StackTraceElement stackTraceElement : stackTrace) {
				if ("main".equals(stackTraceElement.getMethodName())) {
					return Class.forName(stackTraceElement.getClassName());
				}
			}
		}
		catch (ClassNotFoundException ex) {
			// Swallow and continue
		}
		return null;
	}
```

 

基本流程就是创建一个运行时异常，然后获得堆栈数组，遍历StackTraceElement数组，判断方法名称是否为“mian”，如果过是则通过Class.forName()方法创建Class对象。

方法很简单，功能也很简单，但通过读源代码，竟然发现SpringBoot将我们日常视而不见的内容竟然用出新的花样。或许这就是读源代码的魅力所在吧。