# Bean-Xml-depends-on-attr

```java
@Setter
public class Book implements Serializable {
      public Book() {
            System.out.println("Book被创建出来了..."); 
      }

      private String bookName;
      private String author;
      private String published;
        
}
@Setter
public class Dept implements Serializable {
    public Dept() {
        System.out.println("Dept被创建出来了...");
    }

    private Integer did;
    private String dname;
}
@Setter
public class Emp implements Serializable {
    public Emp() {
        System.out.println("Emp被创建出来了....");
    }

    private Integer eid;
    private String ename;
    private Dept dept;
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">
    <!--
        告诉Spring，在创建Emp组件前，你先创建book组件和dept组件
    -->
    <bean id="user" class="com.wnx.spring.model.User" depends-on="book,dept">
        <property name="username" value="王乃醒"/>
    </bean>
    <bean id="dept" class="com.wnx.spring.model.Dept">
        <property name="deptName" value="开发部"/>
    </bean>
    <bean id="book" class="com.wnx.spring.model.Book">
        <property name="bookName" value="JAVA入门"/>
        <property name="author" value="王乃醒"/>
        <property name="publisher" value="人民日报出版社"/>
    </bean>
</beans>
```

```java
    @Log4j2
    @SpringJUnitConfig(locations = "classpath:/bean.xml")
    public class SpringXmlConfigTest {
        @Test
        public void test() {

        }


    }
```

```c#
Book被创建出来了...
Dept被创建出来了...
Emp被创建出来了....
```

