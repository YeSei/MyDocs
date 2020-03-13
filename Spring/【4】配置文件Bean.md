# 【4】配置文件Bean
## 4.1 配置文件配置Bean的三种方式
- 默认构造方法创建
- 使用指定方法构建
- 使用某个类中静态方法构建
### 4.1.1 第一种方式：使用默认构造方法创建。
```
<bean id="accountService" class="com.itheima.AccountService.impl.AccountImpl"></bean>
```
### 4.1.2 第二种方式：对于jar包中的class对象，调用其提供的方法创建并返回bean。（只有在生成class字节码文件时才调用构造函数）
> 
```
<bean id="accountFactory" class="com.itheima.AccountService.impl.AccountImpl"></bean>
<bean id="accountService" factorybean="accountFactory" factory-method="getAccountService" ></bean>
```
### 4.1.3 第三种方式：调用某个类中的静态方法创建。
> 
```
<bean id="accountService" class="com.itheima.factory.StaticFactory" factory-method="getAccountService" ></bean>
```

## 4.2 Bean的作用范围控制（单例、多例）
通过bean标签中的`scope`属性控制。
> singleton:单例（默认的）
> prototype：多例的
> request：作用于web应用的请求范围
> session：作用于web应用的回话范围
> global-session：作用于集群环境的会话范围，当不是集群环境时，就是session。

## 4.3 Bean对象的生命周期
- 单例对象（对象class中需要有初始化方法和销毁方法，并将其赋予到bean标签的init-method，destroy-method方法）
    - 创建：当context容器创建时，也创建对象。
    - 生存：容器在，单例对象就在。
    - 销毁：单例对象的生命周期和容器相同。
- 多例对象
    - 创建：使用对象时，由context容器创建。
    - 生存：对象在使用过程中一直活着。
    - 销毁：当对象长时间不用时，由JAVA垃圾回收机制实现。