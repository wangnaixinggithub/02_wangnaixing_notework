# 为什么能实现自动化配置？

- MyBatis，他同样需要SpringBoot的自动配置的Jar包，以及通过了spring.factories文件。其目的是，让导入MybatisStarter的SpringBoot项目。
- 使用MybatisLanguageDriverAutoConfiguration（MyBatis语音驱动自动化配置类）和MybatisAutoConfiguration（MyBatis自动化配置类）。

<img src="109-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%83%BD%E5%AE%9E%E7%8E%B0%E8%87%AA%E5%8A%A8%E5%8C%96%E9%85%8D%E7%BD%AE.assets/image-20220327102100308.png" alt="image-20220327102100308"  />

# 1、META-INF/spring.factories

```properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.mybatis.spring.boot.autoconfigure.MybatisLanguageDriverAutoConfiguration,\
org.mybatis.spring.boot.autoconfigure.MybatisAutoConfiguration
```

# 2、@EnableAutoConfiguration

- 我们通过在启动类上加@SpringBoot注解来标识这是一个SpringBoot的应用，并提供main方法，一行代码来启动SpringBoot项目,@SpringBoot注解是一个复合注解。进入@SpringBoot注解，发现@EnableAutoConfiguration开启自动配置的注解！

```java
@SpringBootApplication
public class SpringBootLearnApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootLearnApplication.class, args);
    }
}
```

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication{}
```

- 这个注解使得容器会注册`AutoConfigurationImportSelector` 自动配置导入选择器。他会找到spring.factories进行装配。

