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

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
  <bean id="user01" class="com.wnx.spring.model.User">
        <property name="id"  value="1"/>
        <property name="name" value="王乃醒"/>
        <property name="age" value="23"/>
        <property name="height" value="183.5"/>
        <property name="isMarried" value="false"/>
    </bean>
    
</beans>    
```

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
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
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user01" class="com.wnx.spring.model.User">
        <property name="id"  value="1"/>
        <property name="name" value="王乃醒"/>
        <property name="age" value="23"/>
        <property name="height" value="183.5"/>
        <property name="isMarried" value="false"/>
        <property name="cat" ref="cat01"/>
    </bean>
    <bean id="cat01" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫"/>
        <property name="color" value="白色"/>
    </bean>

</beans>
```

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
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

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">

    <bean id="user01" class="com.wnx.spring.model.User">
        <property name="id"  value="1"/>
        <property name="name" value="王乃醒"/>
        <property name="age" value="23"/>
        <property name="height" value="183.5"/>
        <property name="isMarried" value="false"/>
        <property name="cat" ref="cat01"/>
        <property name="hobbies">
            <array>
                <value>写代码</value>
                <value>跑步</value>
                <value>打游戏</value>
            </array>
        </property>
    </bean>
    <bean id="cat01" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫"/>
        <property name="color" value="白色"/>
    </bean>
    
</beans>
```

```javascript
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
     //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码, 跑步, 打游戏], catList=null, map=null, properties=null)
    }
}
```

# 5、集合

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">

    <bean id="user01" class="com.wnx.spring.model.User">
        <property name="id"  value="1"/>
        <property name="name" value="王乃醒"/>
        <property name="age" value="23"/>
        <property name="height" value="183.5"/>
        <property name="isMarried" value="false"/>
        <property name="cat" ref="cat01"/>
        <property name="hobbies">
            <array>
                <value>写代码</value>
                <value>跑步</value>
                <value>打游戏</value>
            </array>
        </property>
        <property name="catList">
            <list>
                <ref bean="cat01"/>
                <ref bean="cat02"/>
                <ref bean="cat03"/>
            </list>
        </property>
    </bean>
    
    <bean id="cat01" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫"/>
        <property name="color" value="白色"/>
    </bean>
    <bean id="cat02" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫2"/>
        <property name="color" value="白色2"/>
    </bean>
    <bean id="cat03" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫3"/>
        <property name="color" value="白色3"/>
    </bean>
    
</beans>
```

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
     //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码, 跑步, 打游戏], catList=[Cat(name=龙猫, color=白色), Cat(name=龙猫2, color=白色2), Cat(name=龙猫3, color=白色3)], map=null, properties=null)
    }
}

```

# 6、Map

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">

    <bean id="user01" class="com.wnx.spring.model.User">
        <property name="id"  value="1"/>
        <property name="name" value="王乃醒"/>
        <property name="age" value="23"/>
        <property name="height" value="183.5"/>
        <property name="isMarried" value="false"/>
        <property name="cat" ref="cat01"/>
        <property name="hobbies">
            <array>
                <value>写代码</value>
                <value>跑步</value>
                <value>打游戏</value>
            </array>
        </property>
        <property name="catList">
            <list>
                <ref bean="cat01"/>
                <ref bean="cat02"/>
                <ref bean="cat03"/>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="name" value="王乃醒"/>
                <entry key="age" value="18"/>
                <entry key="description" value="大帅哥！"/>
            </map>
        </property>
    </bean>
    
    <bean id="cat01" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫"/>
        <property name="color" value="白色"/>
    </bean>
    <bean id="cat02" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫2"/>
        <property name="color" value="白色2"/>
    </bean>
    <bean id="cat03" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫3"/>
        <property name="color" value="白色3"/>
    </bean>
    
</beans>

```

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
     //User(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码, 跑步, 打游戏], catList=[Cat(name=龙猫, color=白色), Cat(name=龙猫2, color=白色2), Cat(name=龙猫3, color=白色3)], map={name=王乃醒, age=18, description=大帅哥！}, properties=null)
    }
}
```

# 7、Properties

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">

    <bean id="user01" class="com.wnx.spring.model.User">
        <property name="id"  value="1"/>
        <property name="name" value="王乃醒"/>
        <property name="age" value="23"/>
        <property name="height" value="183.5"/>
        <property name="isMarried" value="false"/>
        <property name="cat" ref="cat01"/>
        <property name="hobbies">
            <array>
                <value>写代码</value>
                <value>跑步</value>
                <value>打游戏</value>
            </array>
        </property>
        <property name="catList">
            <list>
                <ref bean="cat01"/>
                <ref bean="cat02"/>
                <ref bean="cat03"/>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="name" value="王乃醒"/>
                <entry key="age" value="18"/>
                <entry key="description" value="大帅哥！"/>
            </map>
        </property>
        <property name="properties">
            <props>
                <prop key="username">root</prop>
                <prop key="password">root</prop>
                <prop key="url">jdbc:mysql://localhost:3306/spring</prop>
                <prop key="driverClassName">com.jdbc.mysql.Driver</prop>
            </props>
        </property>
    </bean>
    
    <bean id="cat01" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫"/>
        <property name="color" value="白色"/>
    </bean>
    <bean id="cat02" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫2"/>
        <property name="color" value="白色2"/>
    </bean>
    <bean id="cat03" class="com.wnx.spring.model.Cat">
        <property name="name" value="龙猫3"/>
        <property name="color" value="白色3"/>
    </bean>
    
</beans>

```

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class SpringTest {
    @Autowired
    private User user;

    @Test
    public void test() {
        System.out.println(user);
     //ser(id=1, name=王乃醒, age=23, height=183.5, isMarried=false, cat=Cat(name=龙猫, color=白色), hobbies=[写代码, 跑步, 打游戏], catList=[Cat(name=龙猫, color=白色), Cat(name=龙猫2, color=白色2), Cat(name=龙猫3, color=白色3)], map={name=王乃醒, age=18, description=大帅哥！}, properties={password=root, url=jdbc:mysql://localhost:3306/spring, driverClassName=com.jdbc.mysql.Driver, username=root})
    }
}
```