# Spring-JavaConfig-Aop

# 1、配置 `@EnableAspectJAutoProxy`

```java
package com.wnx.springmvc.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration
@ComponentScan(
        basePackages = "com.wnx.springmvc",
        excludeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,classes = Controller.class)})
@PropertySource("classpath:/jdbc.properties")
@EnableAspectJAutoProxy      //事务的注解启动
@EnableTransactionManagement
public class SpringConfig {
 

}

```

# 2、切面

> 怎么切？

## 2.1、方式1：切入标注有@Action注解的方法

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
@Documented
public @interface Action {
     //自定义注解

}
```

```java
@Component
@Aspect
public class LogAspect {
    
    @Pointcut("@annotation(com.wnx.spring.anno.Action)")
    public void pointCut(){}

    @Before(value = "pointCut()")
    public void before(JoinPoint joinpoint){
        Signature signature = joinpoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()开始执行了！！！");

    }
    @After(value = "pointCut()")
    public void after(JoinPoint joinpoint){
        Signature signature = joinpoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()方法执行完毕");

    }
    @AfterReturning(value = "pointCut()",returning = "returnValue")
    public void returning(JoinPoint joinpoint,Integer returnValue){
        Signature signature = joinpoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()方法返回，返回结果"+returnValue);

    }
    @AfterThrowing(value = "pointCut()",throwing = "e")
    public void throwing(JoinPoint joinPoint,Exception e){
        Signature signature = joinPoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()方法出现异常"+e.getMessage());
    }
    @Around(value = "pointCut()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint){
        Object resultValue = null;
        try {
            // TODO: 2022/3/3  可加前置通知

            resultValue = proceedingJoinPoint.proceed();

            // TODO: 2022/3/3  可加后置通知
        } catch (Throwable e) {

            // TODO: 2022/3/3  可加异常通知
            e.printStackTrace();

        }
        // TODO: 2022/3/3  可加返回通知
        return resultValue;
    }
}

```

## 2.2、方式2：切入指定包下的方法

```java
@Component
@Aspect
public class LogAspect {
    @Pointcut("execution(* com.wnx.spring.bean.*.*(..))")
    public void pointCut(){}

    @Before(value = "pointCut()")
    public void before(JoinPoint joinpoint){
        Signature signature = joinpoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()开始执行了！！！");

    }
    @After(value = "pointCut()")
    public void after(JoinPoint joinpoint){
        Signature signature = joinpoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()方法执行完毕");

    }
    @AfterReturning(value = "pointCut()",returning = "returnValue")
    public void returning(JoinPoint joinpoint,Integer returnValue){
        Signature signature = joinpoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()方法返回，返回结果"+returnValue);

    }
    @AfterThrowing(value = "pointCut()",throwing = "e")
    public void throwing(JoinPoint joinPoint,Exception e){
        Signature signature = joinPoint.getSignature();
        String methodName = signature.getName();
        System.out.println(methodName+"()方法出现异常"+e.getMessage());
    }
    @Around(value = "pointCut()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint){
        Object resultValue = null;
        try {
            // TODO: 2022/3/3  可加前置通知

            resultValue = proceedingJoinPoint.proceed();

            // TODO: 2022/3/3  可加后置通知
        } catch (Throwable e) {

            // TODO: 2022/3/3  可加异常通知
            e.printStackTrace();

        }
        // TODO: 2022/3/3  可加返回通知
        return resultValue;
    }
}

```

# 3、切入点

```java
package com.wnx.spring.bean;

import com.wnx.spring.anno.Action;
import org.springframework.stereotype.Component;
@Component
public class MyCalculatorImpl {
    @Action
    public int add(int a, int b) {
        return a + b;
    }
    public int reduce(int a, int b) {
        return a - b;
    }
}
```





