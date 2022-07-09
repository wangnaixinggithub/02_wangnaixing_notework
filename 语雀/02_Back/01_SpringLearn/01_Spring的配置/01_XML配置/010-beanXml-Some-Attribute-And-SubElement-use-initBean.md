# beanXml-Some-Attribute-And-SubElement-use-initBean

> 例如Book组件

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Book implements Serializable {
    private String bname;
    private String bauthor;
}
```

## 1、子标签 property

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">

  <!--
    1.set方法注入
  -->
  <bean id="book" class="com.wnx.spring.bean.Book">
    <property name="bname" value="西游记"/>
    <property name="bauthor" value="吴承恩"/>
  </bean>


</beans>
```

## 2、子标签 constructor-arg

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">

  <!--
    2.构造函数注入
  -->
  <bean id="orders" class="com.wnx.spring.bean.Orders">
    <constructor-arg name="oname" value="王乃醒订单"/>
    <constructor-arg name="address" value="广东广州"/>
  </bean>

</beans>
```

## 3、namespace `xmlns:p`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
  <!--
    3.P名称空间注入
  -->
<bean id="book" class="com.wnx.spring.bean.Book" p:bname="三国演义" p:bauthor="罗贯中"/>
</beans>
```

## 4、注入 NULL

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
  <!--
     1.属性注入空值
  -->
  <bean id="book" class="com.wnx.spring.bean.Book">
    <property name="bname">
      <null/>
    </property>
    <property name="bauthor">
      <null/>
    </property>
  </bean>
</beans>
```

## 5、含特殊符号

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
<!--
  1.xml特殊字符的使用
-->
   <bean id="orders" class="com.wnx.spring.bean.Orders">
        <property name="oname" value="我的订单"/>
        <property name="address">
          <value><![CDATA[<<南京>>]]></value>
        </property>
   </bean>
</beans>
```

## 6、外部 bean

```java
public class UserDao {
    public void addUser() {
        System.out.println("UserDao...添加用户。。。。");
    }
}

public class UserService {
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void addUser(){
        userDao.addUser();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
  <bean id="userDao" class="com.wnx.spring.dao.UserDao"/>
  <bean id="userService" class="com.wnx.spring.serivce.UserService">
    <property name="userDao" ref="userDao"/>
  </bean>
</beans>
```

## 7、内部 bean

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
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
  <!--
      使用property属性对dept对象赋值的时候，可以使用bean标签写法
  -->
    <bean id="emp" class="com.wnx.spring.bean.Emp">
      <property name="eid" value="1"/>
      <property name="ename" value="王乃醒"/>
      <property name="dept">
        <bean class="com.wnx.spring.bean.Dept">
          <property name="did" value="1"/>
          <property name="dname" value="开发一组"/>
        </bean>
      </property>
    </bean>
</beans>
```

## 8、级联属性赋值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!--
       级联属性赋值
    -->
      <bean id="emp" class="com.wnx.spring.bean.Emp">
        <property name="eid" value="1"/>
        <property name="ename" value="王乃醒"/>
        <property name="dept" ref="dept"/>
        <property name="dept.did" value="1"/>
        <property name="dept.dname" value="开发一组"/>
      </bean>
    <bean id="dept" class="com.wnx.spring.bean.Dept">
      <property name="did" value="2"/>
      <property name="dname" value="开发二组"/>
  </bean>
</beans>
```

## 9、注入Collection<包装类>

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Stu implements Serializable {
    private String[] courses;
    private List<String> list;
    private Map<String,String> maps;
    private Set<String> sets;
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd">
    <bean id="stu" class="com.wnx.spring.bean.Stu">
      <property name="courses">
        <array>
          <value>Java</value>
          <value>Spring</value>
          <value>SpringMVC</value>
          <value>Mybatis</value>
        </array>
      </property>
      <property name="list">
        <list>
          <value>王乃醒1</value>
          <value>王乃醒2</value>
          <value>王乃醒3</value>
        </list>
      </property>
      <property name="maps">
        <map>
          <entry key="java" value="JAVA"/>
          <entry key="spring" value="SPRING"/>
          <entry key="springmvc" value="SPRINGMVC"/>
        </map>
      </property>
      <property name="sets">
        <set>
          <value>MYSQL</value>
          <value>ORACLE</value>
        </set>
      </property>
    </bean>
</beans>
```

## 10、注入Collection<类>

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Course implements Serializable {
    private Integer cid;
    private String cname;
}

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Stu implements Serializable {
    private Course[] courses;
    private List<Course> list;
    private Map<String,Course> maps;
    private Set<Course> sets;
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd">
  
    <bean id="course1" class="com.wnx.spring.bean.Course">
        <property name="cid" value="1"/>
        <property name="cname" value="Java"/>
    </bean>
    <bean id="course2" class="com.wnx.spring.bean.Course">
        <property name="cid" value="2"/>
        <property name="cname" value="Spring"/>
    </bean>
    <bean id="course3" class="com.wnx.spring.bean.Course">
        <property name="cid" value="3"/>
        <property name="cname" value="SpringMVC"/>
    </bean>
    <bean id="course4" class="com.wnx.spring.bean.Course">
        <property name="cid" value="4"/>
        <property name="cname" value="Mybatis"/>
    </bean>


    <bean id="stu" class="com.wnx.spring.bean.Stu">
        <property name="courses">
            <array>
                <ref bean="course1"/>
                <ref bean="course2"/>
                <ref bean="course3"/>
                <ref bean="course4"/>
            </array>
        </property>
        <property name="list">
            <list>
                <ref bean="course1"/>
                <ref bean="course2"/>
                <ref bean="course3"/>
                <ref bean="course4"/>
            </list>
        </property>
        <property name="maps">
            <map>
                <entry key="java" value-ref="course1"/>
                <entry key="spring" value-ref="course2"/>
                <entry key="springmvc" value-ref="course3"/>
            </map>
        </property>
        <property name="sets">
            <set>
                <ref bean="course1"/>
                <ref bean="course2"/>
                <ref bean="course3"/>
                <ref bean="course4"/>
            </set>
        </property>
    </bean>
</beans>
```

> util:list  util:set  util:map 可以抽取集合作为Bean组件被引用。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">

    <util:list id="courseList">
        <ref bean="course1"/>
        <ref bean="course2"/>
        <ref bean="course3"/>
        <ref bean="course4"/>
    </util:list>
    <util:set id="courseSet">
        <ref bean="course1"/>
        <ref bean="course2"/>
        <ref bean="course3"/>
        <ref bean="course4"/>
    </util:set>
    <util:map id="courseMap">
        <entry key="java" value-ref="course1"/>
        <entry key="spring" value-ref="course2"/>
        <entry key="springmvc" value-ref="course3"/>
    </util:map>
    <bean id="course1" class="com.wnx.spring.bean.Course">
        <property name="cid" value="1"/>
        <property name="cname" value="Java"/>
    </bean>
    <bean id="course2" class="com.wnx.spring.bean.Course">
        <property name="cid" value="2"/>
        <property name="cname" value="Spring"/>
    </bean>
    <bean id="course3" class="com.wnx.spring.bean.Course">
        <property name="cid" value="3"/>
        <property name="cname" value="SpringMVC"/>
    </bean>
    <bean id="course4" class="com.wnx.spring.bean.Course">
        <property name="cid" value="4"/>
        <property name="cname" value="Mybatis"/>
    </bean>


    <bean id="stu" class="com.wnx.spring.bean.Stu">
        <property name="courses" ref="courseList"/>
        <property name="list" ref="courseList"/>
        <property name="maps" ref="courseMap"/>
        <property name="sets" ref="courseSet"/>
    </bean>
</beans>
```

