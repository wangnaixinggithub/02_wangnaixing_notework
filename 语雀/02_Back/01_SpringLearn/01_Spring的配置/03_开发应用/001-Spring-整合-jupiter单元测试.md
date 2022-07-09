# Spring整合jupiter单元测试

> 开发中，我们通常使用Junit5进行单元测试，Spring提供Spring-Test模块，可以让单元测试自带有SpringIoC环境。

```xml
 	 	<!--1.jupiter的依赖-->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>
        <!--2. Spring test的依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.2.6.RELEASE</version>
        </dependency>
```

## 1、Xml配置

```java
@SpringJUnitConfig(locations = "classpath:spring.xml")
public class SpringTest {
    @Autowired
    private User user;
    @Test
    public void test(){
        System.out.println(user);
    }
}
```

## 2、JavaConfig配置

```java
@SpringJUnitConfig(SpringConfig.class)
public class SpringTest {
    @Autowired
    private User user;
    @Test
    public void test(){
        System.out.println(user);
    }
}
```

