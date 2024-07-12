# java的final关键字详解

在Java中，`final`关键字可用于修饰类、方法和变量。下面是每种情况的详解：
1.`final` 类：当一个类被声明为 `final` 时，表示它不能被继承。换句话说，没有其他类可以继承该 `final` 类的任何特性。例如：

```Java

final class MyFinalClass {
       // 类的代码
   }

```
2.`final` 方法：当你声明一个方法为 `final`，表示这个方法不能被子类所重写（override）。但是，继承该方法的子类可以继承这个方法。例如：

```Java

class MyClass {
   final void myFinalMethod() {
       // 方法的代码
   }
}

```

3.`final` 变量：当变量被声明为 `final`，这就表示它的值一旦被定义后，就不能被修改。这对于创建常量会非常有用，例如：

```Java
   final int MYCONSTANT = 10;
```

这里，`MYCONSTANT` 就是一个常量，我们不能在程序中改变它的值。

对于用`final`修饰的局部变量和参数，如果修饰的是基本数据类型，那么在初始化之后其值就不能改变；

如果修饰的是引用类型变量，那么在初始化之后其引用不能改变，即不能指向其他对象，但是对象的内容是可以改变的。

在Java中使用 `final`关键字可以增加性能。对 `final` 变量的读取比普通变量更快，因为在编译阶段已经将结果放到常量池了。
底层读取常量池的速度要比从内存或者寄存器读取要快。

另外，使用`final`关键字能有效地帮助你设计更好的程序，它们用于表示“这是不能被改变的”。

使用`final`关键字的注意事项。

比如有一些数据需要程序启动的时候就初始化，并且在程序运行期间不能更改就可以用`final`来修饰它。

对于引用类型的变量。比如`List`，`map` 等。我们可以使用包装类。

1.使用 `Collections` 类
```Java

List<String> originalList = new ArrayList<>(Arrays.asList("one", "two", "three"));
List<String> unmodifiableList = Collections.unmodifiableList(originalList);

Map<String, Integer> originalMap = new HashMap<>();
originalMap.put("one", 1);
originalMap.put("two", 2);
Map<String, Integer> unmodifiableMap = Collections.unmodifiableMap(originalMap);

```

值得注意的是，上面的代码创建的是一个内容不可变的列表或映射。这意味着，你不能添加、删除或更改列表、集合或映射中的元素。
然而，如果列表、集合或映射中的元素本身是可变的（例如，列表中的元素是可变的列表），那么这些元素的内容仍然是可以更改的。

另外，Collections.unmodifiableList方法和类似的方法返回的是原始列表的只读视图，这意味着，如果原始列表被修改，这些修改将在只读视图中反映出来。
如果你想创建一个完全独立的、内容不可变的副本，可能需要首先对原始数据进行一份深拷贝。