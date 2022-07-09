# Spring组件属性注入

# 1、构建组件之间依赖

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
	//1.基本数据类型的包装类
    private Long id;
    private String name;
    private Integer age;
    private Double height;
    private Boolean isMarried;
	
    //2.引用数据类型
    private Cat cat;
	
    //3.数组
    private String[] hobbies;
    
    //4.集合
    private List<Cat> catList;
    
    //5.Map
    private Map<String,Object> map;
    
    //6.Properties
    private Properties properties;

}

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Cat implements Serializable {
        private String name;
        private String color;
}
```

# 2、基本数据类型包装类

```java
package com.wnx.spring.config;

import com.wnx.spring.model.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean
    public User user(){
        User user = new User();
        user.setId(1L);
        user.setName("王乃醒");
        user.setAge(23);
        user.setHeight(183.5);
        user.setIsMarried(false);
        return user;
    }
}

```

```java
@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
        //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=null, hobbies=null, catList=null, map=null, properties=null)
    }
}
```

# 3、对象

- spring.xml

```javascript
package com.wnx.spring.config;

import com.wnx.spring.model.Cat;
import com.wnx.spring.model.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean
    public User user(){
        User user = new User();
        user.setId(1L);
        user.setName("王乃醒");
        user.setAge(23);
        user.setHeight(183.5);
        user.setIsMarried(false);
        user.setCat(cat01());
        return user;
    }

    @Bean
    public Cat cat01(){
        Cat cat = new Cat();
        cat.setName("龙猫");
        cat.setColor("白色");
        return cat;
    }
}

```

```java
@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
        //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=null, catList=null, map=null, properties=null)
    }
}
```

# 4、数组

```java
package com.wnx.spring.config;


import com.wnx.spring.model.Cat;
import com.wnx.spring.model.User;
import org.apache.commons.lang3.StringUtils;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringConfig {
    @Bean
    public User user(){
        User user = new User();
        user.setId(1L);
        user.setName("王乃醒");
        user.setAge(23);
        user.setHeight(183.5);
        user.setIsMarried(false);
        user.setCat(cat01());
        user.setHobbies(StringUtils.split("写代码,跑步,打游戏"));
        return user;
    }

    @Bean
    public Cat cat01(){
        Cat cat = new Cat();
        cat.setName("龙猫");
        cat.setColor("白色");
        return cat;
    }
}

```

```javascript
@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
        //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码,跑步,打游戏], catList=null, map=null, properties=null)
    }
}
```

# 5、集合

```java
package com.wnx.spring.config;


import com.wnx.spring.model.Cat;
import com.wnx.spring.model.User;
import org.apache.commons.lang3.StringUtils;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.ArrayList;
import java.util.List;

@Configuration
public class SpringConfig {
    @Bean
    public User user(){
        User user = new User();
        user.setId(1L);
        user.setName("王乃醒");
        user.setAge(23);
        user.setHeight(183.5);
        user.setIsMarried(false);
        user.setCat(cat01());
        user.setHobbies(StringUtils.split("写代码,跑步,打游戏"));
        List<Cat> catList  = new ArrayList<>();
        catList.add(cat01());
        catList.add(cat02());
        catList.add(cat03());
        user.setCatList(catList);
        return user;
    }

    @Bean
    public Cat cat01(){
        Cat cat = new Cat();
        cat.setName("龙猫");
        cat.setColor("白色");
        return cat;
    }
    @Bean
    public Cat cat02(){
        Cat cat = new Cat();
        cat.setName("龙猫2");
        cat.setColor("白色2");
        return cat;
    }
    @Bean
    public Cat cat03(){
        Cat cat = new Cat();
        cat.setName("龙猫3");
        cat.setColor("白色3");
        return cat;
    }
}

```

```java
@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
        //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码,跑步,打游戏], catList=[Cat(name=龙猫, color=白色), Cat(name=龙猫2, color=白色2), Cat(name=龙猫3, color=白色3)], map=null, properties=null)
    }
}
```

# 6、Map

```java
package com.wnx.spring.config;


import com.wnx.spring.model.Cat;
import com.wnx.spring.model.User;
import org.apache.commons.lang3.StringUtils;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Configuration
public class SpringConfig {
    @Bean
    public User user(){
        User user = new User();
        user.setId(1L);
        user.setName("王乃醒");
        user.setAge(23);
        user.setHeight(183.5);
        user.setIsMarried(false);
        user.setCat(cat01());
        user.setHobbies(StringUtils.split("写代码,跑步,打游戏"));
        List<Cat> catList  = new ArrayList<>();
        catList.add(cat01());
        catList.add(cat02());
        catList.add(cat03());
        user.setCatList(catList);
        Map<String,Object> map = new HashMap<>();
        map.put("name","王乃醒");
        map.put("age",18);
        map.put("description","大帅哥！");
        user.setMap(map);
        
        return user;
    }

    @Bean
    public Cat cat01(){
        Cat cat = new Cat();
        cat.setName("龙猫");
        cat.setColor("白色");
        return cat;
    }
    @Bean
    public Cat cat02(){
        Cat cat = new Cat();
        cat.setName("龙猫2");
        cat.setColor("白色2");
        return cat;
    }
    @Bean
    public Cat cat03(){
        Cat cat = new Cat();
        cat.setName("龙猫3");
        cat.setColor("白色3");
        return cat;
    }
}

```

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
//User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码,跑步,打游戏], catList=[Cat(name=龙猫, color=白色), Cat(name=龙猫2, color=白色2), Cat(name=龙猫3, color=白色3)], map=null, properties=null)
    }
}
```

# 7、Properties

```java
package com.wnx.spring.config;


import com.wnx.spring.model.Cat;
import com.wnx.spring.model.User;
import org.apache.commons.lang3.StringUtils;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.*;

@Configuration
public class SpringConfig {
    @Bean
    public User user(){
        User user = new User();
        user.setId(1L);
        user.setName("王乃醒");
        user.setAge(23);
        user.setHeight(183.5);
        user.setIsMarried(false);
        user.setCat(cat01());
        user.setHobbies(StringUtils.split("写代码,跑步,打游戏"));
        List<Cat> catList  = new ArrayList<>();
        catList.add(cat01());
        catList.add(cat02());
        catList.add(cat03());
        user.setCatList(catList);
        Map<String,Object> map = new HashMap<>();
        map.put("name","王乃醒");
        map.put("age",18);
        map.put("description","大帅哥！");
        user.setMap(map);
        Properties properties = new Properties();
        properties.setProperty("username","root");
        properties.setProperty("password","root");
        properties.setProperty("url","jdbc:mysql://localhost:3306/spring");
        properties.setProperty("driverClassName","com.jdbc.mysql.Driver");
        user.setProperties(properties);
        return user;
    }

    @Bean
    public Cat cat01(){
        Cat cat = new Cat();
        cat.setName("龙猫");
        cat.setColor("白色");
        return cat;
    }
    @Bean
    public Cat cat02(){
        Cat cat = new Cat();
        cat.setName("龙猫2");
        cat.setColor("白色2");
        return cat;
    }
    @Bean
    public Cat cat03(){
        Cat cat = new Cat();
        cat.setName("龙猫3");
        cat.setColor("白色3");
        return cat;
    }
}

```

```java
@SpringJUnitConfig(classes = SpringConfig.class)
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
        //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码,跑步,打游戏], catList=[Cat(name=龙猫, color=白色), Cat(name=龙猫2, color=白色2), Cat(name=龙猫3, color=白色3)], map={name=王乃醒, description=大帅哥！, age=18}, properties={url=jdbc:mysql://localhost:3306/spring, password=root, driverClassName=com.jdbc.mysql.Driver, username=root})
    }
}
```