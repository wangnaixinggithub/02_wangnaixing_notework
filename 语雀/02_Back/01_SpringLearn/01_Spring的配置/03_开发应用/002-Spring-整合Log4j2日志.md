# 002-Spring-整合-log4j2

# 1、配置

```xml
  		<!-- log4j落地实现 -->
		<dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.17.2</version>
        </dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration status="INFO" monitorInterval="30">
    <appenders>
        <console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="[%d{yyyy-MM-dd HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </console>
    </appenders>
    <loggers>
        <root level="info">
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
```

# 2、使用

```java
@Service
@Log4j2
public class UserService {
    private final UserDao userDao;

    @Autowired
    public UserService(UserDao userDao) {
        this.userDao = userDao;
    }

    public void addUser() {
        log.info("UserService...execute...");
        userDao.addUser();
    }
}
```

