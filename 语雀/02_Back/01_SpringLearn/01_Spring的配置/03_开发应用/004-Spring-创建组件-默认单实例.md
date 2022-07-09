# Spring-创建组件-默认均为单实例

# 1、单实例组件

```xml
    <bean id="course" class="com.wnx.spring.model.Course">
        <property name="courseId" value="1"/>
        <property name="courseName" value="JAVA"/>
    </bean>
```

```java
@Setter
public class Course implements Serializable {
    public Course() {
        System.out.println("Course无参构造器被执行，Course对象被创建！");
    }
    private Integer courseId;
    private String courseName;
}
```

- 注册一个Course组件，启动IOC容器，并连续两次获取他。比较变量course course2 进行==比较，返回true。
- 表明，他们都指向了堆区的同一块内存空间。从而说明IOC只会把组件创建赋值一次！

```java
@Test
public void test(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    System.out.println("IOC容器创建完毕");
    Course course = applicationContext.getBean("course", Course.class);
    Course course2 = applicationContext.getBean("course", Course.class);
    System.out.println(course == course2); 
}
```

```
Course无参构造器被执行，Course对象被创建！
IOC容器创建完毕
true
```

# 2、改为多实例

```xml
   <bean id="course" class="com.wnx.spring.model.Course" scope="prototype">
        <property name="courseId" value="1"/>
        <property name="courseName" value="JAVA"/>
    </bean>
```

```java
@Test
public void test(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    System.out.println("IOC容器创建完毕");
    Course course = applicationContext.getBean("course", Course.class);
    Course course2 = applicationContext.getBean("course", Course.class);
    System.out.println(course == course2);
}
```

- 调用依次，创建一次，每次都是新的。

```c#
IOC容器创建完毕
Course无参构造器被执行，Course对象被创建！
Course无参构造器被执行，Course对象被创建！
false
```

