# SpringBoot-Merge-HTTPs

1、将证书复制到resource目录下，这里的证书采用JDK的keytool工具来自动生成！

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/112-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-%E5%9C%A8SpringBoot%E4%B8%AD%E9%9B%86%E6%88%90Https.assets/image-20220327115742375.png)

```yaml
#application.yaml
server:
  ssl:
    key-store: classpath:wangnaixing.p12
    key-alias: tomcathttps
    key-store-password: wzxy1234
```

```java
@RestController
@SpringBootApplication
public class SpringBootLearnApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootLearnApplication.class, args);
    }
    //提供接口
    @GetMapping("/hello")
    public String hello(){
        return "hello";
    }
}
```

- 如果没有配置之前，通过http://localhost:8080/hello就可以返回hello响应体数据。

- 因为配值了安全连接HTTPS，所以浏览器无法发起HTTP无安全请求了

- 告诉我们浏览器需要做TLS认证，进行HTTPS请求才可以。

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/112-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-%E5%9C%A8SpringBoot%E4%B8%AD%E9%9B%86%E6%88%90Https.assets/image-20220327120016971.png)

- https://localhost:8080/hello，再次请求，就可以了！因
- 为这个证书是我们自己的，官方没有授权认可，所以就显示不安全。

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/112-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-%E5%9C%A8SpringBoot%E4%B8%AD%E9%9B%86%E6%88%90Https.assets/image-20220327120204482.png)

