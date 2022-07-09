# Bean的作用域

# 1、默认为单例

```java
@Configuration
public class SpringConfig {

    @Bean
    public User user(){
        return new User();
    }
}
```



```java
public class Application {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        User user = (User) context.getBean("user");
        User user2 = (User) context.getBean("user");
        User user3 = (User) context.getBean("user");
        System.out.println(user == user2); //true
        System.out.println(user2 == user3); //true
    }
}

```

# 2、使用@Scope注解

```java
package com.wnx.spring.config;

import com.wnx.spring.bean.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

/**
 * @author by wangnaixing
 * @Description
 * @Date 2022/3/3 22:11
 */
@Configuration
public class SpringConfig {

    @Bean
    @Scope("prototype")
    public User user(){
        return new User();
    }
}

```
# 3、区别

- 设置scope="singleton"的时候，当加载spring.xml文件的时候，就会创建这个bean，加入到容器中。
- 设置scope="prototype"的时候，当加载spring.xml文件的时候，不会，记下，不会创建这个bean！而在调用的时候创建。调用一次创建一次新的。

