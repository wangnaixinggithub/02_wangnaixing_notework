# 	Spring-GenericApplicationContext

# 1、函数式编程的ApplicationContext

```java
public class SpringTest {
@Test
public void test() {
    GenericApplicationContext applicationContext = new GenericApplicationContext();
    applicationContext.refresh();
    applicationContext.registerBean("user1", User.class,() -> new User(1,"王乃醒","健康"));
    User user1 = applicationContext.getBean("user1", User.class);
    System.out.println(user1);
}
}
```

# 2、源码

```java
package org.springframework.context.support;
public class GenericApplicationContext extends AbstractApplicationContext implements BeanDefinitionRegistry {
	public final <T> void registerBean(
			@Nullable String beanName, Class<T> beanClass, BeanDefinitionCustomizer... customizers) {}
}   
```

![](005-Spring-GenericApplicationContext-%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B.assets/image-20220511215824831.png)
