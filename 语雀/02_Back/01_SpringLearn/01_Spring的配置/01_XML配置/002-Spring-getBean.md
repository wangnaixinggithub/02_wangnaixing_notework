# 从容器中获取Bean

## 1、Spring配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="book" class="com.wnx.spring.model.Book"/>
    <bean id="book2" class="com.wnx.spring.model.Book"/>

</beans>
```

## 2、getBean三种方式

```java
public class getBeanTest {
   private ApplicationContext applicationContext;

   //根据Spring.xml配置文件读取IOC容器
    @Before
    public void before(){
         applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    }

    
    @Test
    public void getBeanWay01(){
        //方式1：根据ID获取
       ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Book book = (Book) applicationContext.getBean("book");
    }
    
    @Test
    public void getBeanWay02(){
        //方式2：根据类型获取
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Book book = applicationContext.getBean(Book.class);
    }
    
    @Test
    public void getBeanWay02_easy_Belong_Exception(){
        //根据类型获取，如果容器中有两个类型一样的Bean，容易出错。而根据ID就不会，因为ID在IOC中唯一！
        //: No qualifying bean of type 'com.wnx.spring.model.Book' available: expected single matching bean but found 2: book,book2
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Book bean = applicationContext.getBean(Book.class);
    }
    
    @Test
    public void getBeanWay3(){
        //1.把classpath的bean.xml资源转换为Spring的Resource资源。
        Resource resource = new ClassPathResource("bean.xml");

        //2.准备Bean定义注册器。
        BeanDefinitionRegistry beanDefinitionRegistry = new DefaultListableBeanFactory();

        //3.基于此Bean定义注册器构建Bean定义阅读器
        BeanDefinitionReader  beanDefinitionReader = new XmlBeanDefinitionReader(beanDefinitionRegistry);

        //4.Bean定义阅读器读取此Resource资源，加载Ban定义对象。
        beanDefinitionReader.loadBeanDefinitions(resource);

        //5.此时Bean定义就加载进了Bean定义注册器中（底层完成bean标签转变为Bean定义对象）
        BeanDefinition beanDefinition = beanDefinitionRegistry.getBeanDefinition("book");

        //6.一个Bean定义注册器也是Bean工厂
        BeanFactory beanFactory = (BeanFactory) beanDefinitionRegistry;
        
        //7.从Bean工厂中获取Bean
        Book book = beanFactory.getBean("book", Book.class);
  
    }
    
}
```

