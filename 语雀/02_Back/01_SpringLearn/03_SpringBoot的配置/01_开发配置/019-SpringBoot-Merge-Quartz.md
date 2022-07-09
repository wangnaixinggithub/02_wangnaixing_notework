# SpringBoot-Merge-Quartz

```xml
  <!--pom.xml-->   
  <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-quartz</artifactId>
    </dependency>
```

```java
@EnableScheduling
@Configuration
public class ScheduledConfig {
    
    @Bean
    public MethodInvokingJobDetailFactoryBean methodInvokingJobDetailFactoryBean(){
        //1.装配 => 调用作业详细信息工厂
        MethodInvokingJobDetailFactoryBean methodInvokingJobDetailFactoryBean = new MethodInvokingJobDetailFactoryBean();
        //2. 你配置的定时任务组件名 
        methodInvokingJobDetailFactoryBean.setTargetBeanName("myJob");
        //3. 你配置的定时任务目标方法名
        methodInvokingJobDetailFactoryBean.setTargetMethod("sayHello");
        
        return methodInvokingJobDetailFactoryBean;
    }
    
    @Bean
    public SimpleTriggerFactoryBean simpleTriggerFactoryBean(MethodInvokingJobDetailFactoryBean methodInvokingJobDetailFactoryBean ){
        //1.装配 => 简单的触发器工厂Bean
        SimpleTriggerFactoryBean simpleTriggerFactoryBean = new SimpleTriggerFactoryBean();
        //2.设置触发器启动时间
        simpleTriggerFactoryBean.setStartTime(new Date());
        //3.触发次数5次
        simpleTriggerFactoryBean.setRepeatCount(5);
        //4.触发周期间隔 3秒
        simpleTriggerFactoryBean.setRepeatInterval(3000);
        //6 => 注入 methodInvokingJobDetailFactoryBean
        simpleTriggerFactoryBean.setJobDetail(methodInvokingJobDetailFactoryBean.getObject());
        
        return simpleTriggerFactoryBean;
    }
    
    @Bean
    public SchedulerFactoryBean schedulerFactoryBean(SimpleTriggerFactoryBean simpleTriggerFactoryBean){
        //1.装配 => 调用作业详细信息工厂
        SchedulerFactoryBean schedulerFactoryBean = new SchedulerFactoryBean();
        
        //2.注入 => simpleTriggerFactoryBean
        schedulerFactoryBean.setTriggers(simpleTriggerFactoryBean.getObject());
       
        return schedulerFactoryBean;
    }
    
}
```

```java
@Component
public class MyJob {
    public void sayHello(){
        System.out.println("王乃醒是一个大帅哥！！！");
    }
}
```







