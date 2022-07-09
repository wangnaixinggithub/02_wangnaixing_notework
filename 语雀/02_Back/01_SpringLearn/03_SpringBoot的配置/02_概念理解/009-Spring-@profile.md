# Spring-@Profile

```java
public interface Food {
    public void showFoodName();
}

public class Noodle implements Food{
    @Override
    public void showFoodName() {
        System.out.println("这是面条");
    }
}

public class Rice implements Food{
    @Override
    public void showFoodName() {
        System.out.println("这是米饭");
    }
}
```

# 1、 @Profile

```java
@Configuration
public class ConditionSpringConfig {
    @Bean
    @Profile("南方人")
    public Food noodle(){
        return new Noodle();
    }
    @Bean
    @Profile("北方人")
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
    ioc.getEnvironment().setActiveProfiles("北方人");
    ioc.register(ConditionSpringConfig.class);
    ioc.refresh();
    Food bean = ioc.getBean(Food.class);
    bean.showFoodName();
}
}
```

# 2、底层

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(ProfileCondition.class)
public @interface Profile {
	String[] value();

}
```

- ProfileCondition类，获取到@Profile注解的value属性，如果组件上的profile和系统使用的profile一样就注册组件进去。

```java
class ProfileCondition implements Condition {

	@Override
	public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
		MultiValueMap<String, Object> attrs = metadata.getAllAnnotationAttributes(Profile.class.getName());
		if (attrs != null) {
			for (Object value : attrs.get("value")) {
				if (context.getEnvironment().acceptsProfiles(Profiles.of((String[]) value))) {
					return true;
				}
			}
			return false;
		}
		return true;
	}

}
```

