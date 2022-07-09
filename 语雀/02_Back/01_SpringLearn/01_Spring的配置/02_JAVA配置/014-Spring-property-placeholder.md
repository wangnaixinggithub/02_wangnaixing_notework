# property-placeholder

- 数据库必要连接参数

```properties
#jdbc.properties
jdbc.username=root
jdbc.password=root
jdbc.url=jdbc:mysql://localhost:3306/spring
jdbc.driverClassName=com.mysql.jdbc.cj.Driver
```

- 使用@PropertySource注解读取jdbc.properties

```java
@Configuration
@PropertySource("classpath:/jdbc.properties")
public class SpringConfig {
    @Bean
    public DataSource dataSource(@Value("jdbc.username")String username,
                                 @Value("jdbc.password") String password,
                                 @Value("jdbc.driverClassName") String driverClassName,
                                 @Value("jdbc.url") String url){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(driverClassName);
        return dataSource;
    }
}

```

```java
@Log4j2
@SpringJUnitConfig(SpringConfig.class)
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
[2022-05-06 23:03:42:895] [INFO] - com.wnx.spring.SpringTest.test(SpringTest.java:24) - DataSource=> {
	CreateTime:"2022-05-06 23:03:42",
	ActiveCount:0,
	PoolingCount:0,
	CreateCount:0,
	DestroyCount:0,
	CloseCount:0,
	ConnectCount:0,
	Connections:[
	]
}
[2022-05-06 23:03:42:906] [INFO] - com.alibaba.druid.support.logging.Log4j2Impl.info(Log4j2Impl.java:106) - {dataSource-0} closing ...
```