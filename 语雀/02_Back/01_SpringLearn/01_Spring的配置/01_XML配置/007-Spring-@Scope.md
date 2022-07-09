# Bean的作用域

# 1、默认为单例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

 <bean id="user" class="com.wnx.spring.bean.User"/>


</beans>
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
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

 <bean id="user" class="com.wnx.spring.bean.User" scope="prototype"/>


</beans>
```

# 3、区别

- 设置scope="singleton"的时候，当加载spring.xml文件的时候，就会创建这个bean，加入到容器中。
- 设置scope="prototype"的时候，当加载spring.xml文件的时候，不会，记下，不会创建这个bean！而在调用的时候创建。调用一次创建一次新的。

