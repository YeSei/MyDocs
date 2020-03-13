# 【6】Spring注解的使用
## 6.1 使用注解创建Bean
> 第一步：使用@Component，@Controller，@Service，@Respository标志要创建Bean的类。
> 第二步：若使用xml配置文件，创建IoC容器，则在xml加入<context:component-scan>指定创建容器时要扫描的包。

## 6.2 使用注解管理容器（代替XML文件）
> 第一步：使用`@Configuration`标志此类为配置容器类；
> 第二步：使用`@ComponentScan(s)`指定创建容器时要扫描的包。等同于在xml中配置<context:component-scan>；
> 第三步：使用`@Bean`把当前方法的返回值作为Bean对象存入IoC容器中。用`name`指定id。对于方法有参数，则去IoC容器中去寻找Bean，查找方式按照`@autowired`一样。
> 第四步：对于Bean作用范围使用`@Scope`指定。
> 第五步：可用`@import`指定导入其他配置类，属性设置为其他配置类字节码。
> 第六步：可用`@Value`为配置类的成员变量指定值。
> 第七步：`@PropertySource`指定properties文件位置。可在value中用关键字classpath表示类路径下。`@PropertySource("calsspath：ContextConfig.properties")`