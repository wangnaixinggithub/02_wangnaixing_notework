# Spring-createBean-use-MyBeanFactory

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Book implements Serializable {
    private String bookName;
    private String bookAuthor;
}
```

# 1、使用自定义静态工厂

```java

public class BookStaticFactoryBean {
    public static Book getBook(String bookName,String bookAuthor){
        return Book.builder().bookName(bookName).bookAuthor(bookAuthor).build();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="book" class="com.wnx.spring.factory.BookStaticFactoryBean" factory-method="getBook" >
        <constructor-arg name="bookName" value="JAVA大神的成长"/>
        <constructor-arg name="bookAuthor" value="王乃醒"/>
    </bean>
</beans>
```

```java
    @Log4j2
    @SpringJUnitConfig(locations = "classpath:/bean.xml")
    public class SpringXmlConfigTest {
        @Autowired
        private Book book;
        @Test
        public void test() {
           log.info("Book=>{}",book);
        }

    }
```

```
[2022-05-08 23:15:26:017] [INFO] - com.wnx.spring.SpringXmlConfigTest.test(SpringXmlConfigTest.java:21) - Book=>Book(bookName=JAVA大神的成长, bookAuthor=王乃醒)	
```



# 2、使用自定义实例工厂

```java
public class BookStaticFactoryBean {
    public  Book getBook(String bookName,String bookAuthor){
        return Book.builder().bookName(bookName).bookAuthor(bookAuthor).build();
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="bookStaticFactoryBean" class="com.wnx.spring.factory.BookStaticFactoryBean"/>
    <bean id="book" class="com.wnx.spring.model.Book" factory-bean="bookStaticFactoryBean" factory-method="getBook"  >
        <constructor-arg name="bookName" value="JAVA大神的成长"/>
        <constructor-arg name="bookAuthor" value="王乃醒"/>
    </bean>
</beans>

```

```java
    @Log4j2
    @SpringJUnitConfig(locations = "classpath:/bean.xml")
    public class SpringXmlConfigTest {
        @Autowired
        private Book book;
        @Test
        public void test() {
           log.info("Book=>{}",book);
        }

    }
```

```
[2022-05-08 23:18:42:308] [INFO] - com.wnx.spring.SpringXmlConfigTest.test(SpringXmlConfigTest.java:21) - Book=>Book(bookName=JAVA大神的成长, bookAuthor=王乃醒)
```

> `JavaConfig`配置没有办法使用自定义的工厂。
