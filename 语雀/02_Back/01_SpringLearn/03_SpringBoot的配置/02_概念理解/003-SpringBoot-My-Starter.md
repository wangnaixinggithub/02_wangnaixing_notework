# 自定义SpringBoot Starter

# 1、写自定义Starter代码

```yaml
#application.yaml
user:
  name: wangnaixing
  age: 25
  description: 王乃醒是一个大帅哥！
```

```java
@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
@ConfigurationProperties("user")
public class UserProperties {
    private String name;
    private Integer age;
    private String description;
}
```

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class UserService {
    private String name;
    private Integer age;
    private String description;
    public void say(){
        System.out.println(name+"===="+age+"+++++"+description);
    }
}
```

```java
@Configuration
@EnableConfigurationProperties(UserProperties.class)
@ConditionalOnClass(UserService.class)
public class UserServiceAutoConfig {
    @Autowired
    private UserProperties userProperties;
    @Bean
    public UserService userService(){
        return UserService.builder()
            .name(userProperties.getName())
            .age(userProperties.getAge())
            .description(userProperties.getDescription())
            .build();
    }
}
```

# 2、META-INF/spring.factories

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.wnx.mystarter.config.UserServiceAutoConfig
```

```
mvn clean 

mvn install
```

- 可以清晰的看到，系统将Maven工程放在了我们本地仓库（maven_repository）并以我们maven坐标，版本号为存放文件夹，放入了一个pom文件。并告知你构建成功和构建时总共花费的时间，于什么时间构建结束。

![](D:/my_workspace/01_my_notebook/%E8%AF%AD%E9%9B%80/01_SpringLearn/03_SpringBoot%E7%9A%84%E9%85%8D%E7%BD%AE/02_%E6%A6%82%E5%BF%B5%E7%90%86%E8%A7%A3/image-20220327100226044.png)

# 3、导入Starter

```xml
      <dependency>
            <groupId>com.wnx.mystarter</groupId>
            <artifactId>mystarter</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency
```

```yaml
#application.yaml覆盖原来user配置
user:
  name: wangnaixing
  age: 25
  description: 王乃醒是一个大帅哥hahaha！
```

```java
import com.wnx.mystarter.service.UserService;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@Slf4j
@SpringBootTest
class SpringBootLearnApplicationTests {
    @Autowired
    private UserService userService;
    @Test
    void contextLoads() {
        log.info("userService->{}",userService); //: userService->UserService(name=wangnaixing, age=25, description=王乃醒是一个大帅哥haha！)
        userService.say(); //wangnaixing====25+++++王乃醒是一个大帅哥haha！
    }
}
```

