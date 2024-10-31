# java中的双冒号访问

### 记录一下使用java双冒号引用遇到的问题

#### 问题复现如下

##### 有抽象类A，子类B 如下，子类和父类不在同一个包下

```Java
public abstract class A {

    /**
    *
    protected abstract List<Consumer<List<String>>> v();

    /**
     * 普通方法
     *
     * @param x 参数
     */
    protected void x(List<String> x) {
        x.forEach(System.out::println);
    }
    
    /**
     * 测试方法
     */
    public void test() {
        List<Consumer<List<String>>> v = v();

        List<String> list = new ArrayList<>();
        list.add("aaa");

        v.forEach(x -> x.accept(list));
    }
}
```

```Java
@Component
public class B extends A {
    @Override
    public void test() {
        super.test();
    }

    @Override
    protected List<Consumer<List<String>>> v() {

        //写法1 关注这一行 也就是使用双冒号访问父类的方法引用
        Consumer<List<String>> x = super::x;
        
        //写法2 子类调用父类方法
        //Consumer<List<String>> x = (y)-> super.x(y);

        return Arrays.asList(x);
    }
}
```

```Java
@RequestMapping
@RestController
@Slf4j
public class TestController {

    @Autowired
    private B childClass;

    @GetMapping("/test/child/class")
    public String childClass() {
        childClass.test();
        return "OK";
    }
}
```

#### 启动项目报错 编译失败 子类中采用 `super::x` 方式访问父类方法

> java: incompatible types: invalid method reference
x(java.util.List<java.lang.String>) has protected access in xxx

问题描述：

子类父类不在同一个包下，父类普通方法通过`protected`修饰。子类中使用双冒号访问父类的`protected`修饰的方法。就会报错。

#### 解决办法

1. 采用写法2，`Consumer<List<String>> x = (y)-> super.x(y);`就不会报错，可以正常通过编译。

2. 将父类方法改为public。也可以正常编译通过。

3. 子类父类放在同一个包下也能正常编译通过。

> 下面是我查询到的一些资料。

**`::`**（方法引用）是比较特殊的，它在Java 8引入，允许将方法作为对象来传递。方法引用的背后依赖了Java 8引入的 **Lambda表达式** 和 **函数式接口** 机制，这使得方法本身可以在运行时作为一个“对象”来引用和传递。

##### 方法引用 **`::`** 是什么？

在Java中，方法引用是Lambda表达式的简化形式，提供了一种直接引用方法而不需要显式调用的方式。通过`::`引用方法，可以直接将现有方法赋值给函数式接口（如`Consumer`、`Supplier`等），从而在函数式编程的场景中使用已有方法。

##### 方法引用的引入：Java 8

* `::`方法引用以及Lambda表达式在 Java 8 中首次引入。
* Java 8带来了`java.util.function`包，包含大量函数式接口，如`Consumer`、`Supplier`、`Function`等，这些接口为方法引用提供了具体类型。
* 方法引用是Lambda表达式的一种特殊形式，本质上也是对函数式接口的实现。

##### 方法引用如何将方法作为对象传递

在Java中，方法引用的背后依赖于`Lambda Metafactory`机制，这个机制会将方法引用或Lambda表达式转化为一个动态生成的实现类实例，从而在运行时通过对象引用来执行方法。

###### JVM中的实现原理

方法引用通过JVM的`InvokeDynamic`指令及其背后的`Lambda Metafactory`机制来实现。这是一个较为复杂的过程，简要步骤如下：

1. 编译时生成`InvokeDynamic`指令：
    * 当编译器遇到Lambda表达式或方法引用时，并不会直接生成具体实现类，而是生成`InvokeDynamic`指令。
    * `InvokeDynamic`指令是Java 7引入的指令，最早用于动态语言的调用支持。Java 8开始用于Lambda表达式和方法引用的实现。
2. 运行时动态生成实现类：
    * 当JVM执行到`InvokeDynamic`指令时，会触发`Lambda Metafactory`。
    * `Lambda Metafactory`会动态生成一个实现了目标函数式接口的类实例，并通过“方法句柄”（`MethodHandle`）绑定到具体的实例方法或静态方法上。
    * 这个生成的类实例充当了方法的“对象化引用”，从而可以像对象一样进行传递和调用。
3. 将方法作为对象传递：

    * 生成的实现类实例在运行时是一个对象，且具有对方法的引用能力。因此，在使用时，方法引用的调用等效于在生成的实例上调用方法，从而实现了“方法对象化”。

### 小结
Java 8 引入了方法引用 **`::`**，为函数式编程提供了简洁的写法。
方法引用依赖 `InvokeDynamic` 和 `Lambda Metafactory`，在运行时动态生成实现类，以将方法转换为对象。
这种机制使得Java的Lambda表达式和方法引用无需生成大量匿名类，也使得代码更简洁，性能更高。
因此，**`::`** 方法引用背后的机制确保了方法可以在Java中以对象的方式进行传递和操作，这也是Java 8提升性能和代码简洁度的重要改进之一。


### 不同包下的限制
如果Child类位于另一个包，例如`com.another`包中，那么即使Child是Parent的子类，也不能通过 **`::`** 方法引用访问`protected`方法。因为在这种情况下，方法引用相当于“将方法公开”，超出了`protected`的权限范围。不同包的子类访问`protected`成员时只能在继承链内部直接调用，不能公开引用。