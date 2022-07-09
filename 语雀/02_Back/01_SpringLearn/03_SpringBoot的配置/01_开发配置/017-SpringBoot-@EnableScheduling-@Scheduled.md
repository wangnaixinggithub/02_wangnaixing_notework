# SpringBoot-@EnableScheduling-@Scheduled

- SpringBoot默认提供有定时任务，作为开发者可以通过@EnableScheduling,启动它。

```java
@EnableScheduling
@Configuration
public class ScheduledConfig {

}
```

- 创建一个组件，在组件的方法上使用@Scheduled注解，使用`cron表达式`就可以在指定的时间内执行啦！

```java
@Component
public class MyJob {
    @Scheduled(cron = "1-2 0/1 * * * ? ")
    public void corn(){
        System.out.println("呵呵呵");
    }
}
```

