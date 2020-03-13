# 【7】Spring整合Junit
## 7.1 问题分析：
> Junit不会加载IoC容器，即使使用`@Autowired`也无法直接使用Bean对象。

## 7.2 问题解决：
> 1. 导入Spring整合junit依赖`spring-test`，需要junit 4.12+版本。
> 2. 使用Junit提供的一个注解`@Runwith`把原有的main方法替换，
> 3. `@ContextConfiguration`告知spring的运行器，IoC容器得创建是基于xml还是注解类。分别使用location和class指定xml或注解类字节码的位置。`@ContextConfiguration(location="classpath:bean.xml"`或`@ContextConfiguration(class=config.class`。