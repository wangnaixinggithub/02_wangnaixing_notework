# IOCBean的创建顺序

- Bean的创建顺序决定于写配置文件顺序前后有关。 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="emp" class="com.wnx.spring.model.Emp">
        <property name="eid" value="1"/>
        <property name="ename" value="王乃醒"/>
        <property name="dept">
            <null/>
        </property>
    </bean>
    <bean id="dept" class="com.wnx.spring.model.Dept">
        <property name="did" value="1"/>
        <property name="dname" value="开发一部"/>
    </bean>
</beans>
```

```java
    package com.wnx.spring;

    import lombok.extern.log4j.Log4j2;
    import org.junit.jupiter.api.Test;
    import org.springframework.test.context.junit.jupiter.SpringJUnitConfig;

    /**
     * @author by wangnaixing
     * @Description
     * @Date 2022/4/23 14:41
     */
    @Log4j2
    @SpringJUnitConfig(locations = "classpath:/bean.xml")
    public class SpringXmlConfigTest {
        @Test
        public void test() {
           
        }

    }

```

```c#
Emp被创建出来了....
Dept被创建出来了...
```

- 切换顺序,再启动测试

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
    <bean id="emp" class="com.wnx.spring.model.Emp">
        <property name="eid" value="1"/>
        <property name="ename" value="王乃醒"/>
        <property name="dept">
            <null/>
        </property>
    </bean>
  
</beans>
```

```c#
Dept被创建出来了...
Emp被创建出来了....
```

