# Bean标签的属性 - parent

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

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="dept" class="com.wnx.spring.model.Dept">
         <property name="did" value="1"/>
         <property name="dname" value="开发一部"/>
    </bean>

    <bean id="emp1" class="com.wnx.spring.model.Emp">
        <property name="eid"  value="1"/>
        <property name="ename" value="杨川庆"/>
        <property name="dept" ref="dept"/>
    </bean>
    <bean id="emp2" class="com.wnx.spring.model.Emp" parent="emp1">
        <property name="eid"  value="1"/>
        <property name="ename" value="杨卫波"/>
    </bean>

    <bean id="emp3" class="com.wnx.spring.model.Emp" parent="emp1">
        <property name="eid"  value="1"/>
        <property name="ename" value="王乃醒"/>
    </bean>
</beans>

```

```java
@Log4j2
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringXmlConfigTest {
    @Resource(name = "emp1")
    private Emp emp1;
    @Resource(name = "emp2")
    private Emp emp2;
    @Resource(name = "emp3")
    private Emp emp3;

    @Test
    public void test() throws SQLException {
      log.info("emp1 =>{}",emp1);
      log.info("emp2 =>{}",emp2);
      log.info("emp3 =>{}",emp3);
    }
}

```

```c#
[2022-05-06 23:32:14:162] [INFO] - com.wnx.spring.SpringXmlConfigTest.test(SpringXmlConfigTest.java:28) - emp1 =>Emp(eid=1, ename=杨川庆, dept=Dept(did=1, dname=开发一部))
[2022-05-06 23:32:14:162] [INFO] - com.wnx.spring.SpringXmlConfigTest.test(SpringXmlConfigTest.java:29) - emp2 =>Emp(eid=1, ename=杨卫波, dept=Dept(did=1, dname=开发一部))
[2022-05-06 23:32:14:162] [INFO] - com.wnx.spring.SpringXmlConfigTest.test(SpringXmlConfigTest.java:30) - emp3 =>Emp(eid=1, ename=王乃醒, dept=Dept(did=1, dname=开发一部))
```

