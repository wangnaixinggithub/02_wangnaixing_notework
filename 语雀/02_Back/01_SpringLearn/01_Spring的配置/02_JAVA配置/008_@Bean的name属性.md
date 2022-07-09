# @Bean的id和名称name属性

## 1、name注解会覆盖默认方法名为组件名

> name为组件别名。可以指定多个。原来会把方法名作为组件beanName，如果`@Bean` name属性不是一个空数组，则方法名不再作为默认的组件beanName,如再使用方法名获取会抛出异常。

```java
package com.wnx.spring.config;

import com.wnx.spring.bean.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean(name = {"user1","user2","user3"})
    public User user(){
        return new User();
    }
}

```

```java
    @Test
    public void test_bean_anno_name_property() {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("com.wnx.spring");
        User user = (User) context.getBean("user1");
        User user2 = (User) context.getBean("user2");
        User user3 = (User) context.getBean("user3");
        //输入都是ture.
        log.info(user == user2);
        log.info(user2 == user3);
        log.info(user == user3);
    }
```

```java
  @Test
    public void test_bean_anno_existing_name_property_can_throw_exception() {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("com.wnx.spring");
        User user = (User) context.getBean("user");
        //throw exception!!!
        //org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'user' available
    }
```

## 2、默认方法名等价于beanName

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
   @Test
    public void default_case_methodName_equals_beanName() {
        ApplicationContext ioc = new AnnotationConfigApplicationContext("com.wnx.spring");
        User user = ioc.getBean("user",User.class);
        //can pass !!!

    }
```







