# Spring-JavaConfig-Aop

# 1、配置 `<aop:aspectj-autoproxy/>`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

     <context:component-scan base-package="com.wnx.springmvc" use-default-filters="true">
       <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
     <!--事务的注解启动-->
    <tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>


</beans>
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

# 4、在很早之前的AOP的Xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx
      http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <context:component-scan base-package="com.wnx" use-default-filters="true">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    <!--
        告诉Spring哪个类是切面类，切入点在哪
    -->
    <aop:config>
        <aop:pointcut id="pointCut1" expression="execution(* com.wnx.spring.dao.*.*(..))"/>
        <aop:aspect ref="userDaoProxy">
            <aop:before method="before" pointcut-ref="pointCut1"/>
            <aop:after method="after" pointcut-ref="pointCut1"/>
            <aop:after-throwing method="afterThrowing" pointcut-ref="pointCut1" throwing="e"/>
            <aop:after-returning method="afterReturning" pointcut-ref="pointCut1"/>
            <aop:around method="around" pointcut-ref="pointCut1"/>
        </aop:aspect>
    </aop:config>
</beans>
```





