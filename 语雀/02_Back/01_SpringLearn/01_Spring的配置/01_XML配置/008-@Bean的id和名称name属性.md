# @Bean的id和名称name属性

## 1、name注解会覆盖默认方法名为组件名

> name为组件别名。可以指定多个。另外还可以使用id属性，指定组件名。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">
    <bean id="user"  name="user1,user2,user3" class="com.wnx.spring.model.User"/>
</beans>
```

```java
    @Test
    public void test_bean_xml_name_property() {
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
          //can pass !!!
    }
```

## 2、默认全类名等价于beanName

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">
    <bean class="com.wnx.spring.model.User"/>
</beans>

```

```java
   @Test
    public void test_className_equals_beanName() {
        ApplicationContext context = new ClassPathXmlApplicationContext("classpath:/bean.xml");
        User user = (User) context.getBean("com.wnx.spring.model.User");
        //can Pass   
    
```





