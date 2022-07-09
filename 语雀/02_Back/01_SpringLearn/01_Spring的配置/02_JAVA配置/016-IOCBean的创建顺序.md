# IOCBean的创建顺序

- Bean的创建顺序决定于写配置类方法顺序前后有关。 

```java
package com.wnx.spring.config;

import com.wnx.spring.model.Dept;
import com.wnx.spring.model.Emp;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean
    public Emp emp1(){
        Emp emp = new Emp();
        emp.setEid(1);
        emp.setEname("王乃醒");
        emp.setDept(null);
        return emp;

    }

    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDid(1);
        dept.setDname("开发一部");
        return dept;

    }

}

```

```java
package com.wnx.spring;

import com.wnx.spring.config.SpringConfig;
import lombok.extern.log4j.Log4j2;
import org.junit.jupiter.api.Test;
import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;

import java.sql.SQLException;

@Log4j2
@SpringJUnitConfig(SpringConfig.class)
public class SpringJavaConfigTest {
    
    @Test
    public void test() throws SQLException {

    }
}

```

```c#
Emp被创建出来了....
Dept被创建出来了...
```

- 切换顺序,再启动测试

```java
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
    public Emp emp1(){
        Emp emp = new Emp();
        emp.setEid(1);
        emp.setEname("王乃醒");
        emp.setDept(null);
        return emp;

    }
}

```

```c#
Dept被创建出来了...
Emp被创建出来了....
```