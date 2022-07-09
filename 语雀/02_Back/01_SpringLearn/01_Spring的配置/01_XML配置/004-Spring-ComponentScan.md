# 自动化装配Bean 包扫描机制

# 1、spring.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
    <context:component-scan base-package="com.wnx.spring"/>

</beans>
```

# 2、UserController

```java
@Controller
public class UserController {

    @Autowired
    private UserService userService;

    public void findAll(){
        userService.findAll();
    }

}

```

# 3、UserService

```java
package com.wnx.spring.serivce;

import com.wnx.spring.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;


@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;

    public void findAll() {
        userMapper.findAll();
    }
}

```

# 4、UserMapper

```java
@Repository
public class UserMapper {
    public void findAll() {
        System.out.println("UserMapper，执行查询全部用户！");
    }
}

```

# 5、PackageScanTest

```java
package com.wnx.spring;

import com.wnx.spring.controller.UserController;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;


@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private UserController userController;

    @Test
    public void test() {
        userController.findAll(); //UserMapper，执行查询全部用户！

    }

}

```

