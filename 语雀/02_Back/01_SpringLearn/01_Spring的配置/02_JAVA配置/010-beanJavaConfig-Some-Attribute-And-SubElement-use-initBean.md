# beanJavaConfig-Some-Attribute-And-SubElement-use-initBean

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

## 1、set方法

```java
@Configuration
public class SpringConfig {
    @Bean
    public Book book(){
        Book book = new Book();
        book.setBname("王乃醒是JAVA大神");
        book.setBauthor("王乃醒");
        return book;
    }
}
```

## 2、构造方法

```java
@Configuration
public class SpringConfig {
    @Bean
    public Book book(){
        return  new Book("王乃醒是JAVA大神","王乃醒");
    }
}
```



## 4、注入 NULL

```java
@Configuration
public class SpringConfig {
    @Bean
    public Book book(){
        Book book = new Book();
        //下面两行可以省略，因为对象创建后，String是类，默认初始值也是Null
        book.setBname(null);
        book.setBauthor(null);
        return book;
    }
}
```

## 5、含特殊符号

```java
@Configuration
public class SpringConfig {
    @Bean
    public Orders orders(){
        Orders orders = new Orders();
        orders.setOname("我的订单");
        orders.setAddress("<![CDATA[<<南京>>]]>");
        return orders;
    }
}
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

```java
@Configuration
public class SpringConfig {
    @Bean
    public UserDao userDao(){
        return new UserDao();
    }

    @Bean
    public UserService userService(UserDao userDao){
        UserService userService = new UserService();
        userService.setUserDao(userDao);
        return userService;
    }
}
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

```java
@Configuration
public class SpringConfig {
    @Bean
    public Emp emp(){
        Emp emp = new Emp();
        emp.setEid(1L);
        emp.setEname("王乃醒");
        Dept dept = new Dept();
        dept.setDid(1L);
        dept.setDname("开发一组");
        emp.setDept(dept);
        return emp;
    }
}
```

## 8、级联属性赋值

```java
@Configuration
public class SpringConfig {
    @Bean
    public Dept dept(){
        Dept dept = new Dept();
        dept.setDid(1L);
        dept.setDname("开发一组");
        return dept;
    }
    @Bean
    public Emp emp(Dept dept){
        Emp emp = new Emp();
        emp.setEid(1L);
        emp.setEname("王乃醒");
        emp.setDept(dept);
        emp.getDept().setDid(2L);
        emp.getDept().setDname("开发2组");
        return emp;
    }
}
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

```java
@Configuration
public class SpringConfig {
    @Bean
    public Stu stu(){
        Stu stu = new Stu();
        stu.setCourses(StringUtils.split("Java,Spring,SpringMVC,Mybatis"));
        stu.setList(Arrays.asList("王乃醒1","王乃醒2","王乃醒3"));
        stu.setMaps(ImmutableMap.of("java","java","spring","SPRING","springmvc","SPRINGMVC"));
        stu.setSets(ImmutableSet.of("MYSQL","ORACLE"));
        return stu;
    }
}
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

```java
@Configuration
public class SpringConfig {

    @Bean
    public Course course1(){
        Course course = new Course();
        course.setCid(1);
        course.setCname("Java");
        return course;
    }

    @Bean
    public Course course2(){
        Course course = new Course();
        course.setCid(2);
        course.setCname("Spring");
        return course;
    }

    @Bean
    public Course course3(){
        Course course = new Course();
        course.setCid(3);
        course.setCname("SpringMVC");
        return course;
    }

    @Bean
    public Course course4(){
        Course course = new Course();
        course.setCid(4);
        course.setCname("Mybatis");
        return course;
    }


    @Bean
    public Stu stu(Course course1,Course course2,Course course3,Course course4){
        Stu stu = new Stu();
        stu.setCourses(new Course[]{course1,course2,course3,course4});
        stu.setList(Lists.newArrayList(course1,course2,course3,course4));
        stu.setMaps(ImmutableMap.of("java",course1,"spring",course2,"springmvc",course3));
        stu.setSets(ImmutableSet.of(course1,course2,course3,course4));
        return stu;
    }
}

```

> util:list  util:set  util:map 可以抽取集合作为Bean组件被引用。

```java
package com.wnx.spring.config;

import com.google.common.collect.ImmutableMap;
import com.google.common.collect.ImmutableSet;
import com.google.common.collect.Lists;
import com.wnx.spring.model.Course;
import com.wnx.spring.model.Stu;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.List;
import java.util.Map;
import java.util.Set;


@Configuration
public class SpringConfig {

    @Bean
    public Course course1(){
        Course course = new Course();
        course.setCid(1);
        course.setCname("Java");
        return course;
    }

    @Bean
    public Course course2(){
        Course course = new Course();
        course.setCid(2);
        course.setCname("Spring");
        return course;
    }

    @Bean
    public Course course3(){
        Course course = new Course();
        course.setCid(3);
        course.setCname("SpringMVC");
        return course;
    }

    @Bean
    public Course course4(){
        Course course = new Course();
        course.setCid(4);
        course.setCname("Mybatis");
        return course;
    }

    @Bean
    public Course[] commonArray(Course course1,Course course2,Course course3,Course course4){
       return new Course[]{course1,course2,course3,course4};
    }

    @Bean
    public List<Course> commonList(Course course1,Course course2,Course course3,Course course4){
       return Lists.newArrayList(course1,course2,course3,course4);
    }

    @Bean
    public Map<String,Course> commonMap(Course course1,Course course2,Course course3,Course course4){
       return ImmutableMap.of("java",course1,"spring",course2,"springmvc",course3);

    }

    @Bean
    public Set<Course> commonSet(Course course1,Course course2,Course course3,Course course4){
       return  ImmutableSet.of(course1,course2,course3,course4);

    }



    @Bean
    public Stu stu(Course[] commonArray,List<Course> commonList,Map<String,Course> commonMap,Set<Course> commonSet){
        Stu stu = new Stu();
        stu.setCourses(commonArray);
        stu.setList(commonList);
        stu.setMaps(commonMap);
        stu.setSets(commonSet);
        return stu;
    }
}

```

