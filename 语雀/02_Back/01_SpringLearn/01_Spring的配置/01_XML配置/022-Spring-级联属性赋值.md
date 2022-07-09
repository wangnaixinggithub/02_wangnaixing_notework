# Spring-XmlConfig-级联属性-赋值

- Spring的级联属性赋值，前提条件是这个组件已经存在。通过访问成员变量的方式，修改确定引用指向的存储空间中值。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
	
	 <bean id="emp" class="com.wnx.spring.model.Emp">
        <property name="empId" value="1"/>
        <property name="empName" value="王乃醒"/>
        <property name="empSex" value="男"/>
        <property name="dept" ref="dept"/>
        <property name="dept.deptId" value="2"/>
        <property name="dept.deptName" value="开发二部"/>
    </bean>
    
    <bean id="dept" class="com.wnx.spring.model.Dept">
        <property name="deptId" value="1"/>
        <property name="deptName" value="开发一部"/>
    </bean>
</beans>
```

