# SpringBoot-Merge-jasypt

```xml
 <!--配置文件加密-->
 <dependency>
     <groupId>com.github.ulisesbocchio</groupId>
     <artifactId>jasypt-spring-boot-starter</artifactId>
     <version>2.1.0</version>
 </dependency>
```

配置文件加入秘钥配置项`jasypt.encryptor.password`，这是我的密钥wangnaixing666。为并将需要脱敏的`value`值替换成预先经过加密的内容`ENC(mVTvp4IddqdaYGqPl9lCQbzM3H/b0B6l)`。

这个格式我们是可以随意定义的，比如想要`GXUWZ[s86ah4jBCGuXMdCzY7dTNQ==]`格式，只要配置前缀和后缀即可。

```yaml
jasypt:
  encryptor:
    password: wangnaixing666
    property:
      prefix: "GXUWZ["
      suffix: "]"
```

`ENC(XXX）`格式主要为了便于识别该值是否需要解密，如不按照该格式配置，在加载配置项的时候`jasypt`将保持原值，不进行解密。

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/jasypt?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai&useSSL=true
    username: root
    password: ENC(s86ah4jBCGuXMdCzY7dTNQ==)
    type: com.alibaba.druid.pool.DruidDataSource

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml


# 秘钥
jasypt:
  encryptor:
    password: wangnaixing666
```

预先生成的加密值，可以通过代码内调用`API`生成

```java
@Autowired
private StringEncryptor stringEncryptor;

public void encrypt(String content) {
    String encryptStr = stringEncryptor.encrypt(content);
    System.out.println("加密后的内容：" + encryptStr);
}
```

