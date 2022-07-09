# Bean的生命周期

## 1、默认生命周期

> Spring中Bean生命周期：指的Spring的一个Bean从创建到销毁的过程。,Spring中Bean的生命周期可分为：

1、通过无参构造创建Bean实例

2、为Bean属性设置值和对其他Bean引用（底层调用set方法）

3、调用Bean的初始化方法（需要进行配置初始化方法）

4、Bean可以使用了（对象可以通过ID获取到了）

5、当容器关闭时，调用Bean的销毁方法（需要进行配置销毁的方法）

## 2、在XML配置中演示

```java
public class Orders implements Serializable {
    private String oname;

    public Orders() {
        System.out.println("1、通过无参构造创建Bean实例");
    }

    public void setOname(String oname) {
        System.out.println("2、为Bean属性设置值和对其他Bean引用（底层调用set方法）");
        this.oname = oname;
    }

    public void initBean(){
        System.out.println("3、调用Bean的初始化方法（需要进行配置初始化方法）");
    }

    public void destroyBean(){
        System.out.println("4、当容器关闭时，调用Bean的销毁方法（需要进行配置销毁的方法）");
    }
}
```

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
    <!--
      需要自己配置Bean的初始化方法，以及销毁方法。
    -->
    <bean id="orders" class="com.wnx.spring.bean.Orders" init-method="initBean" destroy-method="destroyBean">
      <property name="oname" value="王乃醒的订单"/>
    </bean>

</beans>
```

- 获取Bean且关闭IOC

```java
	 @Test
    public void test08(){
        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring.xml");
        Object orders = ioc.getBean("orders", Orders.class);
        System.out.println("Bean可以使用了（对象可以通过ID获取到了）===>"+orders);
        ioc.close();
    }
```

- 控制台打印，和设想保持一致！

```c#
1、通过无参构造创建Bean实例
2、为Bean属性设置值和对其他Bean引用（底层调用set方法）
3、调用Bean的初始化方法（需要进行配置初始化方法）
Bean可以使用了（对象可以通过ID获取到了）===>com.wnx.spring.bean.Orders@75881071
4、当容器关闭时，调用Bean的销毁方法（需要进行配置销毁的方法）
```

## 3、配置有Bean后置处理器的Bean的生命周期

```java
public class MyBeanProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("---在Bean初始化之前执行的方法---");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("---在Bean初始化之后执行的方法---");
        return bean;
    }
```

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
    <!--
            注册Bean的后置处理器 
    -->
    <bean id="myBeanProcessor" class="com.wnx.spring.bean.MyBeanProcessor"/>
    <bean id="orders" class="com.wnx.spring.bean.Orders" init-method="initBean" destroy-method="destroyBean">
      <property name="oname" value="王乃醒的订单"/>
    </bean>

</beans>
```

## 4、什么时候执行

> 可以看出他干预的是，Bean初始化方法前后，执行。

```c#
1、通过无参构造创建Bean实例
2、为Bean属性设置值和对其他Bean引用（底层调用set方法）
---在Bean初始化之前执行的方法---
3、调用Bean的初始化方法（需要进行配置初始化方法）
---在Bean初始化之后执行的方法---
Bean可以使用了（对象可以通过ID获取到了）===>com.wnx.spring.bean.Orders@22eeefeb
4、当容器关闭时，调用Bean的销毁方法（需要进行配置销毁的方法）
```

