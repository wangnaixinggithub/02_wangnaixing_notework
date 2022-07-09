# Spring-@Conditional

- Spring提供的条件注解，一般使用在Bean组件创建时的，当配置类被解析时，默认情况下，这个配置类的所以Bean的会被注册进Spring的容器中。由容器统一管理。

- 但如果加了@Confidition注解，在注解写入一个看条件类（实现Spring的条件接口）他就会判断了，看条件类的matches方法的返回值，如果时true就装配组件进容器，如果为false就不装配组件进容器。

# 1、Condition

```java
public interface Food {
    public void showFoodName();
}

public class Rice implements Food{
    @Override
    public void showFoodName() {
        System.out.println("这是米饭");
    }
}
public class Noodle implements Food{
    @Override
    public void showFoodName() {
        System.out.println("这是面条");
    }
}
```

```java
public class RiceCondition implements Condition {
    @Override //当环境属性的people为南方人时就注册进去。
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return Objects.equals(context.getEnvironment().getProperty("people"), "南方人");
    }
}
```

```java
public class NoodleCondition implements Condition {
    @Override  //当环境属性的people为北方人时就注册进去。
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return Objects.equals(context.getEnvironment().getProperty("people"),"北方人");
    }
}

```

# 2、@Conditional

```java
@Configuration
public class ConditionSpringConfig {
    @Bean
    @Conditional(NoodleCondition.class)
    public Food noodle(){
        return new Noodle();
    }
    @Bean
    @Conditional(RiceCondition.class)
    public Food rice(){
        return new Rice();
    }
}
```

```java
@Log4j2
public class SpringTest {
@Test
public void test() {
    AnnotationConfigApplicationContext ioc = new AnnotationConfigApplicationContext();
    ioc.getEnvironment().getSystemProperties().put("people","北方人");
    ioc.register(ConditionSpringConfig.class);
    ioc.refresh();
    Food bean = ioc.getBean(Food.class);
    bean.showFoodName();
}
}
```

