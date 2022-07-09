# 自动化装配Bean 包扫描机制

# 1、SpringConfig

```java
package com.wnx.spring.config;


import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = "com.wnx.spring")
public class SpringConfig {

}

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

import com.wnx.spring.config.SpringConfig;
import com.wnx.spring.controller.UserController;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;

@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
    @Autowired
    private UserController userController;

    @Test
    public void test() {
        userController.findAll();

    }

}
```

