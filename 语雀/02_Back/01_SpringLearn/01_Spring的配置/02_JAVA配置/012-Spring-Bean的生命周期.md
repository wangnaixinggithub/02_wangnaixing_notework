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

```java
@Configuration
public class SpringConfig {
    @Bean(initMethod = "initBean",destroyMethod = "destroyBean")
    public Orders orders(){
        Orders orders = new Orders();
        orders.setOname("王乃醒的订单");
        return orders;

    }
}
```

- 获取Bean且关闭IOC

```java
   @Test
    public void test08() {
        AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext("com.wnx.spring");
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

```java
@Configuration
public class SpringConfig {
	//  注册Bean的后置处理器 
    @Bean
    public MyBeanProcessor myBeanProcessor(){
        return new MyBeanProcessor();
    }
    @Bean(initMethod = "initBean",destroyMethod = "destroyBean")
    public Orders orders(){
        Orders orders = new Orders();
        orders.setOname("王乃醒的订单");
        return orders;

    }
}
```

## 4、什么时候执行

> 可以看出他干预的是，Bean初始化方法前后，执行。

```c#
1、通过无参构造创建Bean实例
2、为Bean属性设置值和对其他Bean引用（底层调用set方法）
---在Bean初始化之前执行的方法---
3、调用Bean的初始化方法（需要进行配置初始化方法）
---在Bean初始化之后执行的方法---
Bean可以使用了（对象可以通过ID获取到了）===>com.wnx.spring.model.Orders@5f7b97da
4、当容器关闭时，调用Bean的销毁方法（需要进行配置销毁的方法）
```