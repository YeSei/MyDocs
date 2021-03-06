### 【1】序列化serializable
- 序列化是将对象（实例）按照某种协议格式转化为二进制字节顺序；对应的，反序列化就是将二进制字节序列转化为对象。常用于以下三种情况：
    - 数据持久化：将对象持久化到外部存储。
    - 网络传输实现：将对象转为二进制序列传输到远端，并恢复对象表现像在机器本地行为。
    - RMI（远程方法接口）：通过反序列化对象，调用其方法实现RPC（远程过程调用）使其表现得像本地方法，。
- <u>注意，序列化保存的是对象的状态，即成员变量。不保存对象的方法和其类的静态变量和静态方法。</u>
- 除了默认的基本类型及集合类型，自定义的类对象要实现序列化必须实现java.io.Serializable接口。
    - 这个接口本身没有任何方法和变量，是个空接口，仅用来标识可序列化的语义。
- java序列化能够自动处理嵌套的对象，对一个简单对象直接用writeObject()写入流中。当遇到一个对象域时，迭代使用wriiteObject()直到对象域中对象可以直接被写入流中。
- 通过transient标记某些成员变量不会被序列化。
- 当可序列化对象当中包含不可序列化对象时，那么整个序列化行为会失败抛出NotSerializableException。可通过transite标记跳过该对象继续序列化。但是反序列化后，被transite标记的变量，若是基础类型则为0，对象则为null。
- 可以在要序列化的类中重定义writeObject和readObject方法就能实现自定义序列化，进行加密等操作。。
- 序列化具有继承关系，一个类实现序列化后，其所有子类都可以序列化。