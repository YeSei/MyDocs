# 【2】Spring核心容器
> 
> 加入dependencies：
```
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.0.RELEASE</version>
</dependency>
```
> 则会导入如下几个包：
> ![54bbb2df6e7346761720fbb8b7b41ed6](【2】Spring核心容器构成.resources/147DABF4-321D-48DB-9A37-338B2F842E4B.png)
> 查看依赖关系：
> ![48b3cd2b0556d7d0d037e59113db4313](【2】Spring核心容器构成.resources/24BD75BF-6BFC-4DDE-9732-B34EE11A218C.png)
> 这些依赖即为Spring得核心容器。
> ![f7f4f9d80716a8d1ee2eb920b909b521](【2】Spring核心容器构成.resources/20191120222458489.jpg)

## 2.1核心容器的组成
> **Core**：实现了SpringIoC功能，用配置的方式管理Beans之间的依赖关系，BeanFactory接口是Spring框架的核心接口，它实现了容器许多核心的功能。
* * *
> **Beans**：用户编写的类，由容器创建和管理。代表可重用的组件。
* * *
> **SpEL**：表达式语言用于查询和管理运行期的对象，支持设置和获取对象属性，调用对象方法、操作数组、集合等。还提供了逻辑表达式运算、变量定义等功能·使用它就可以方便地通过表达式串和 Spring IOC 容器进行交互。
* * *
> **Context**：构建于Core模块之上，扩展了 BeanFactory 的功能，添加了 i18n 国际化、 Bean生命周期控制、框架事件体系、资源加载透明化等多项功能。此外，该模块还提供了许多企业级服务的支持，如邮件服务、任务调度、 JNDI 定位、 EJB 集成、远程访门等。

