# Spring入门

> Spring解决了组件间的依赖问题，把对象的创建权交给了容器。由容器统一管理Bean的生命周期。—— IOC（权利反转）

## 1、Book组件

```javascript
@Getter
@Setter
public class Book implements Serializable {

    public Book() {
        System.out.println("Book对象的无参构造方法执行！！！");
    }

    private Integer id;
    private String name;
    private Double price;
}
```

## 2、在Spring.xml配置文件注册Book组件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="book" class="com.wnx.spring.model.Book"/>

</beans>
```

## 3、创建容器并从容器中获取Book组件

```java
public class Application {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("spring.xml");
        Book book = (Book) ctx.getBean("book");
        System.out.println(book);     
    }
}
```