# SpringBoot-Use-@ConfigurationProperties

# 1、@ConfigurationProperties

```yaml
# application.yaml
user:
  id: 1
  username: wangnaixing
  nickName: 王乃醒
  password: root
  sex: 男
  email: 827376239@qq.com
  hobby:
    - 篮球
    - 足球
    - 代码
```

```java
@Component
@NoArgsConstructor
@AllArgsConstructor
@Data
@ConfigurationProperties("user") //指定属性前缀
public class User implements Serializable {
    private Integer id;
    private String username;
    private String password;
    private String nickName;
    private String sex;
    private String email;
    private List<String>  hobby;
}
```

```java
@Slf4j
@SpringBootTest
class SpringWebfluxApplicationTests {
    @Autowired
    private User user;
    @Test
    void contextLoads() {
        log.info("{}",user);
         //User(id=1, username=wangnaixing, password=root, nickName=王乃醒, sex=男, email=827376239@qq.com, hobby=[篮球, 足球, 代码])
    }
}
```

# 2、注入对象集合中

```yaml
# application.yaml
redis:
  redisConfigs:
    - host: 192.168.1.1
      port: 6379
    - host: 192.168.1.2
      port: 6380
```

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class RedisConfig implements Serializable {
    private String host;
    private Integer port;
}

@Data
@AllArgsConstructor
@NoArgsConstructor
@ConfigurationProperties("redis")
@Component
public class RedisCluster implements Serializable {
    private List<RedisConfig> redisConfigs;
}
```

```java
@Slf4j
@SpringBootTest
public class SpringTest {
    @Autowired
    private RedisCluster redisCluster;
    @Test
    public void test(){
      log.info("{}",redisCluster);
        //RedisCluster(redisConfigs=[RedisConfig(host=192.168.1.1, port=6379), RedisConfig(host=192.168.1.2, port=6380)])
    }
}
```

# 3、Or user.properties

```properties
# book.properties
user.id=1
user.username=root
user.password=root
user.email=827376239@qq.com
user.sex=female
user.nickName=wangnaixing
```

```java
@Component
@NoArgsConstructor
@AllArgsConstructor
@Data
@PropertySource("classpath:book.properties") 
@ConfigurationProperties("user") //指定属性前缀
public class User implements Serializable {
    private Integer id;
    private String username;
    private String password;
    private String nickName;
    private String sex;
    private String email;
    private List<String>  hobby;
}
```

```java
@Slf4j
@SpringBootTest
class SpringWebfluxApplicationTests {
    @Autowired
    private User user;
    @Test
    void contextLoads() {
        log.info("{}",user);
         //User(id=1, username=wangnaixing, password=root, nickName=王乃醒, sex=男, email=827376239@qq.com, hobby=[篮球, 足球, 代码])
    }
}
```

