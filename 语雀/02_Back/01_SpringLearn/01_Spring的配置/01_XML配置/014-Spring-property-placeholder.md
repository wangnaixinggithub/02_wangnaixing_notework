# property-placeholder

- 数据库必要连接参数

```properties
#jdbc.properties
jdbc.username=root
jdbc.password=root
jdbc.url=jdbc:mysql://localhost:3306/spring
jdbc.driverClassName=com.mysql.jdbc.cj.Driver
```

- 使用属性加载器读取jdbc.properties

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
         https://www.springframework.org/schema/context/spring-context.xsd">

    <context:property-placeholder location="classpath:/jdbc.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="driverClassName" value="${jdbc.driverClassName}"/>
    </bean>
</beans>

```

```java
@Log4j2
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
  @Autowired
  private DataSource dataSource;
    @Test
    public void test() throws SQLException {
     log.info("DataSource=> {}",dataSource);
    }
}
```

```c#
[2022-05-06 22:58:32:904] [INFO] - com.wnx.spring.SpringTest.test(SpringTest.java:23) - DataSource=> {
	CreateTime:"2022-05-06 22:58:32",
	ActiveCount:0,
	PoolingCount:0,
	CreateCount:0,
	DestroyCount:0,
	CloseCount:0,
	ConnectCount:0,
	Connections:[
	]
}
[2022-05-06 22:58:32:914] [INFO] - com.alibaba.druid.support.logging.Log4j2Impl.info(Log4j2Impl.java:106) - {dataSource-0} closing ...
```

