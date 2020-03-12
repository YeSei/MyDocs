### 【2】java是如何反序列化final字段的
#### 2.1 通过java的反射
- 获取对象的类；
- 通过getDeclaredField获取所有定义字段；
- 通过setAccessible（true）绕过字段限制；
- 利用set方法设置字段值。
> 但是这样利用反射的方式需要经过多重安全检查，效率不高。

#### 2。2 利用JDK特制的强大工具sun.misc.Unsafe
它可以让Java开发人员直接操作内存内容。 此低级API允许我们完全绕过Java的访问限制。 Unsafe的API不考虑字段。 相反，它适用于对象和内存偏移量。 没有字段名称，没有访问修饰符（例如公共或私有）； 仅在内存中偏移。
> 这才是序列化在JVM层面设置字段值。

【现实问题：】
一个对象test中，存在`private final String str=“a”；`成员变量。当将final修饰字段值`private final String str=“a”；`在序列化之后修改为：`private final String str=“b”；`。反序列化后发现这个对象的str值输出为b。
><u>问题：</u>按照上述不管哪种方式（实际按照Unsafe方式）进行反序列化，最后的结果都应该绕过final限制修改字段值。

* * *
> <u>解答：</u>因为`private final String str = “b”；`是编译器常量，其中的“b”字符串存入常量池中，直接加入操作数栈中。而Unsafe和反射方式都是针对该反序列化对象test进行的（即在堆内存中），如果使用反射压制访问限制后获取该字段值，会发现其值依然为`“a”`。
