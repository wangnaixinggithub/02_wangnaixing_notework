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

> Java配置中并没有@Bean注解并没有parent属性，所以只能使用之前方式注入。

```java
package com.wnx.spring.config;

import com.wnx.spring.model.Dept;
import com.wnx.spring.model.Emp;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {

    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDid(1);
        dept.setDname("开发一部");
       return dept;

    }

    @Bean
    public Emp emp1(Dept dept){
        Emp emp = new Emp();
        emp.setEid(1);
        emp.setEname("杨川庆");
        emp.setDept(dept);
        return emp;

    }


    public Emp emp2(Dept dept){
        Emp emp = new Emp();
        emp.setEid(2);
        emp.setEname("杨卫波");
        emp.setDept(dept);
        return emp;

    }

    @Bean
    public Emp emp3(Dept dept){
        Emp emp = new Emp();
        emp.setEid(3);
        emp.setEname("王乃醒");
        emp.setDept(dept);
        return emp;

    }
}

```

```java
@Log4j2
@SpringJUnitConfig(SpringConfig.class)
public class SpringJavaConfigTest {

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
[2022-05-06 23:48:36:237] [INFO] - com.wnx.spring.SpringJavaConfigTest.test(SpringJavaConfigTest.java:25) - emp1 =>Emp(eid=1, ename=杨川庆, dept=Dept(did=1, dname=开发一部))
[2022-05-06 23:48:36:237] [INFO] - com.wnx.spring.SpringJavaConfigTest.test(SpringJavaConfigTest.java:26) - emp2 =>Emp(eid=2, ename=杨卫波, dept=Dept(did=1, dname=开发一部))
[2022-05-06 23:48:36:237] [INFO] - com.wnx.spring.SpringJavaConfigTest.test(SpringJavaConfigTest.java:27) - emp3 =>Emp(eid=3, ename=王乃醒, dept=Dept(did=1, dname=开发一部))
```