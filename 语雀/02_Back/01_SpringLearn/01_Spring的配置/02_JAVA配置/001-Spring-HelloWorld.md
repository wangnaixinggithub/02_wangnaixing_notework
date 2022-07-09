# Spring入门

> Spring解决了组件间的依赖问题，把对象的创建权交给了容器。由容器统一管理Bean的生命周期。—— IOC（权利反转）

## 1、Book组件

```javascript
@Data
@AllArgsConstructor
public class Book implements Serializable {

    public Book() {
        System.out.println("Book对象的无参构造方法执行！！！");
    }

    private Integer id;
    private String name;
    private Double price;
}
```

## 2、在Spring.xml配置类注册Book组件

```java
@Configuration
public class SpringConfig {

    /**
     * beanName 就是方法名
     */
    @Bean
    public Book book() {
        return new Book();
    }
}
```

## 3、创建容器并从容器中获取Book组件

```java
public class Application {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext("com.wnx.spring.config");
        Book book = ctx.getBean("book", Book.class);
    }
}
```