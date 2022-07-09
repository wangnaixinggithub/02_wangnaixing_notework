# Bean-Xml-autowire

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Dept implements Serializable {
    private Integer did;
    private String dname;
}

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Emp implements Serializable {
    private Integer eid;
    private String ename;
    private Dept dept;
}
```

- bean标签的autowire自动装配属性，可以根据属性名去查询IOC有没有beanName和它匹配的，有就装配进去。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="emp" class="com.wnx.spring.model.Emp" autowire="byName">
        <property name="eid"  value="#{1}"/>
        <property name="ename" value="#{'王乃醒'}"/>
    </bean>
    <bean id="dept" class="com.wnx.spring.model.Dept">
        <property name="did" value="#{1}"/>
        <property name="dname" value="#{'开发一组'}"/>
    </bean>
</beans>

```

```java
@Log4j2
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private Emp emp;
    @Test
    public void test() {
     log.info("Emp=> {}",emp);
    }
}
```

```c#
[2022-05-06 22:38:02:976] [INFO] - com.wnx.spring.SpringTest.test(SpringTest.java:23) - Emp=> Emp(eid=1, ename=王乃醒, dept=Dept(did=1, dname=开发一组))
```

- 同理，byType也可以，此时IOC会查询有没有和需要注入类型匹配的Bean，有就装配进去。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="emp" class="com.wnx.spring.model.Emp" autowire="byType">
        <property name="eid"  value="#{1}"/>
        <property name="ename" value="#{'王乃醒'}"/>
    </bean>
    <bean id="dept" class="com.wnx.spring.model.Dept">
        <property name="did" value="#{1}"/>
        <property name="dname" value="#{'开发一组'}"/>
    </bean>
</beans>
```

