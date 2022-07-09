# Bean-JavaConfig-autowire

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

- bean属性的autowire自动装配属性，可以根据属性名去查询IOC有没有beanName和它匹配的，有就装配进去。

```java
@Configuration
public class SpringConfig {
    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDid(1);
        dept.setDname("开发一组");
        return dept;
    }
    
    @Bean(autowire = Autowire.BY_NAME )
    public Emp emp(){
        Emp emp = new Emp();
        emp.setEid(1);
        emp.setEname("王乃醒");
        return emp;

    }
}
```

```java
@Log4j2
@SpringJUnitConfig(SpringConfig.class)
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

```java
@Configuration
public class SpringConfig {
    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDid(1);
        dept.setDname("开发一组");
        return dept;
    }

    @Bean(autowire = Autowire.BY_TYPE )
    public Emp emp(){
        Emp emp = new Emp();
        emp.setEid(1);
        emp.setEname("王乃醒");
        return emp;

    }
}
```

> 而在5.1的版本，该属性已经放弃使用了

![image-20220506225018235](013-Spring-Bean-anno-autowire-use-ByXml.assets/image-20220506225018235.png)