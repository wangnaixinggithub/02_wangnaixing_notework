# 从容器中获取Bean

## 1、Spring配置

```java
@Configuration
public class SpringConfig {

    /**
     * beanName 就是方法名
     */
    @Bean
    public Book book() {
        return new Book();
    }
}

```



## 2、getBean三种方式

```java
public class getBeanTest {
   
    @Test
    public void getBeanWay01(){
        //方式1：根据ID获取
        ApplicationContext applicationContext =  new AnnotationConfigApplicationContext("com.wnx.spring.config");
        Book book = (Book) applicationContext.getBean("book");
    }

    @Test
    public void getBeanWay02(){
        //方式2：根据类型获取
        ApplicationContext applicationContext = new  AnnotationConfigApplicationContext("com.wnx.spring.config");
        Book book = applicationContext.getBean(Book.class);
    }

    @Test
    public void getBeanWay02_easy_Belong_Exception(){
        //根据类型获取，如果容器中有两个类型一样的Bean，容易出错。而根据ID就不会，因为ID在IOC中唯一！
        //: No qualifying bean of type 'com.wnx.spring.model.Book' available: expected single matching bean but found 2: book,book2
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext("com.wnx.spring.config");
        Book bean = applicationContext.getBean(Book.class);
    }

    
 	@Test
    public void getBeanWay3() throws IOException {


        //1.准备Bean定义注册器。
        BeanDefinitionRegistry beanDefinitionRegistry = new DefaultListableBeanFactory();

        //2.基于此Bean定义注册器构建Bean定义阅读器
        AnnotatedBeanDefinitionReader annotatedBeanDefinitionReader = new AnnotatedBeanDefinitionReader(beanDefinitionRegistry);
        
        //3.原来xml是把配置文件变成Resource资源解析。但是注解Bean定义的话。好像只需要类型。  不知道配置类和注解Bean定义阅读器的联系在哪？
        annotatedBeanDefinitionReader.register(Book.class);

        //4.此时Bean定义就加载进了Bean定义注册器中
        BeanDefinition beanDefinition = beanDefinitionRegistry.getBeanDefinition("book");
        

        //5.一个Bean定义注册器也是Bean工厂
        BeanFactory beanFactory = (BeanFactory) beanDefinitionRegistry;

        //6.从Bean工厂中获取Bean
        Book book = beanFactory.getBean("book", Book.class);
        System.out.println(book);
    }
    
}
```

