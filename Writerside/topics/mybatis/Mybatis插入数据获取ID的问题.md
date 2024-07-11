# 记一次Mybatis Insert获取ID的问题

异常描述
```Console
No setter found for the keyProperty'id' in 'java.lang.Long'
```

通常集成Mybatis后可以通过如下方式获取Mysql数据库自增ID。

Mapper

```Java

public interface UserMapper {

    // 插入新用户
    void insertUser(@Param("user") User user);

}

```

```xml
<!-- 在 Mapper XML 文件中配置插入语句 -->
<insert id="insertUser" parameterType="User" useGeneratedKeys="true" keyProperty="id">
    <!-- useGeneratedKeys="true" 表示使用数据库自动生成的主键 -->
    <!-- keyProperty="id" 指定将自动生成的主键值赋给对象的哪个属性 -->
    INSERT INTO users (username, password) VALUES (#{username}, #{password})
</insert>
```

```Java
User user = new User();
user.setUsername("john");
user.setPassword("password");

userMapper.insertUser(user); // 调用插入操作

// 现在可以访问 user.getId() 来获取自动生成的主键值
System.out.println("Inserted user ID: " + user.getId());
```

为什么会出现异常呢？

问题出在`UserMapper`的`interface` `@Param("user")` 注解上

```Java

public interface UserMapper {

    //问题在这个@Param("user") 注解上 加了注解后 xml里面需要使用 user.id 来设置keyProperty映射

    // 插入新用户
    void insertUser(@Param("user") User user);

}

```

解决问题的方法是

1.去掉 insert 方法的 `@Param` 注解

2.在xml中设置 **keyProperty="user.id"**

```xml
<!-- 在 Mapper XML 文件中配置插入语句 -->
<insert id="insertUser" parameterType="User" useGeneratedKeys="true" keyProperty="user.id">
    <!-- useGeneratedKeys="true" 表示使用数据库自动生成的主键 -->
    <!-- keyProperty="id" 指定将自动生成的主键值赋给对象的哪个属性 -->
    INSERT INTO users (username, password) VALUES (#{username}, #{password})
</insert>
```

问题解决后探究一下mybatis的原理。

`@Param` 注解通常用于 Mapper 接口方法的参数上，用来指定参数的名称。例如：
```Java
public interface UserMapper {
    User getUserById(@Param("id") Long id);
}
```

在这个例子中，`@Param("id")` 指定了参数 id 的名称，这样在 XML 映射文件中可以使用 `#{id}` 来引用这个参数。

```xml
<select id="getUserById" resultType="User">
  SELECT * FROM user WHERE id = #{id}
</select>
```

不使用 `@Param` 的话，MyBatis 是不能识别参数名称的，它只会使用 `#{param1}`，`#{param2}` 等形式来引用参数。

那么 `@Param` 如何实现这个功能的呢？

`MyBatis` 在调用 `mapper` 方法时，会通过 `MapperMethod` 类来封装和处理方法调用。在 `MapperMethod` 的 `execute` 方法中，会通过 `paramNameResolver.getNamedParams(args)` 来获取参数名和参数值的映射。
`ParamNameResolver` 是一个专门用来解析方法参数名的类，在它的 `getNamedParams` 方法中，会遍历每个参数，如果参数使用了 `@Param` 注解，那么就会把注解的 `value` 作为参数名，参数值放入到一个 map 中，这个 map 就是 ParamMap。
而在生成 SQL 语句时，`MyBatis` 会使用这个 ParamMap 来替换 SQL 语句中的参数引用。这部分的代码位于 `SqlSourceBuilder` 类的 `build` 方法中，`SqlSourceBuilder` 会通过 OGNL 表达式库来分析和处理 SQL 语句，如果遇到 #{} 格式的参数引用，就会在 ParamMap 中查找对应的参数值。

mybatis源码路径
> org.apache.ibatis.reflection.ParamNameResolver

```Java
/**
   * <p>
   * A single non-special parameter is returned without a name.
   * Multiple parameters are named using the naming rule.
   * In addition to the default names, this method also adds the generic names (param1, param2,
   * ...).
   * </p>
   *
   * @param args
   *          the args
   * @return the named params
   */
  public Object getNamedParams(Object[] args) {
    final int paramCount = names.size();
    if (args == null || paramCount == 0) {
      return null;
    } else if (!hasParamAnnotation && paramCount == 1) {
      Object value = args[names.firstKey()];
      return wrapToMapIfCollection(value, useActualParamName ? names.get(0) : null);
    } else {
      final Map<String, Object> param = new ParamMap<>();
      int i = 0;
      for (Map.Entry<Integer, String> entry : names.entrySet()) {
        param.put(entry.getValue(), args[entry.getKey()]);
        // add generic param names (param1, param2, ...)
        final String genericParamName = GENERIC_NAME_PREFIX + (i + 1);
        // ensure not to overwrite parameter named with @Param
        if (!names.containsValue(genericParamName)) {
          param.put(genericParamName, args[entry.getKey()]);
        }
        i++;
      }
      return param;
    }
  }


public ParamNameResolver(Configuration config, Method method) {
    this.useActualParamName = config.isUseActualParamName();
    final Class<?>[] paramTypes = method.getParameterTypes();
    final Annotation[][] paramAnnotations = method.getParameterAnnotations();
    final SortedMap<Integer, String> map = new TreeMap<>();
    int paramCount = paramAnnotations.length;
    // get names from @Param annotations
    for (int paramIndex = 0; paramIndex < paramCount; paramIndex++) {
      if (isSpecialParameter(paramTypes[paramIndex])) {
        // skip special parameters
        continue;
      }
      String name = null;
      for (Annotation annotation : paramAnnotations[paramIndex]) {
        if (annotation instanceof Param) {
          hasParamAnnotation = true;
          name = ((Param) annotation).value();
          break;
        }
      }
      if (name == null) {
        // @Param was not specified.
        if (useActualParamName) {
          name = getActualParamName(method, paramIndex);
        }
        if (name == null) {
          // use the parameter index as the name ("0", "1", ...)
          // gcode issue #71
          name = String.valueOf(map.size());
        }
      }
      map.put(paramIndex, name);
    }
    names = Collections.unmodifiableSortedMap(map);
  }
```

The End!