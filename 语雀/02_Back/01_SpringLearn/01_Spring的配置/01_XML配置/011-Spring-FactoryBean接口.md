# FactoryBean

## 1、某工厂Bean，用来创建某Bean的。

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Course implements Serializable {
    private Integer cid;
    private String cname;
}

@Setter
public class CourseFactoryBean implements FactoryBean<Course> {
    private Integer cid;
    private String cname;


    @Override
    public Course getObject() throws Exception {
        Course course = new Course();
        course.setCid(cid);
        course.setCname(cname);
        return course;
    }

    @Override
    public Class<?> getObjectType() {
        return Course.class;
    }
}
```

## 2、把工厂Bean注册到IOC

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="courseFactoryBean" class="com.wnx.spring.model.CourseFactoryBean">
            <property name="cid"  value="#{1}"/>
            <property name="cname" value="#{'王乃醒学习Spring'}"/>
        </bean>
</beans>
```

3、装配的是Bean，而不是工厂Bean.

```java
@Log4j2
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private Course course;

    @Test
    public void test_factoryBean_instance() {
        log.info("Course => {}",course);
        
    }

}
```

```c#
[2022-05-06 22:15:42:507] [INFO] - com.wnx.spring.SpringTest.test_factoryBean_instance(SpringTest.java:23) - Course => Course(cid=1, cname=王乃醒学习Spring)
```