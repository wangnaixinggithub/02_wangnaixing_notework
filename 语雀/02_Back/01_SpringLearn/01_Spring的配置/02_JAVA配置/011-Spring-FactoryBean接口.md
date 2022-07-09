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
@Configuration
public class SpringConfig {
    @Bean
    public CourseFactoryBean courseFactoryBean(){
        CourseFactoryBean courseFactoryBean = new CourseFactoryBean();
        courseFactoryBean.setCid(1);
        courseFactoryBean.setCname("王乃醒学习Spring");
        return courseFactoryBean;
    }
}
```

3、装配的是Bean，而不是工厂Bean.

```java
@Log4j2
@SpringJUnitConfig(SpringConfig.class)
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

