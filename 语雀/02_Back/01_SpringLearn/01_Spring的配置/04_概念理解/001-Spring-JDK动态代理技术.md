# JDK动态代理技术

# 1、帮谁做事？

> 代理对象是：MyCalculatorImpl,代理方法为：`# public int add(int a, int b)`

```java
 //被代理对象必须有接口
public interface MyCalculator {
    int add(int a,int b); 
}
public class MyCalculatorImpl implements MyCalculator{
    @Override
    public int add(int a, int b) {
        return a + b;
    }
}
```

# 2、增强

```java
package com.wnx.spring;

import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
public class CalculatorProxy {
    public static  Object getInstance(final MyCalculatorImpl  myCalculator){
        return Proxy.newProxyInstance(
                CalculatorProxy.class.getClassLoader(),
                myCalculator.getClass().getInterfaces(),
                (Object proxy, Method method, Object[] args) -> {
                    
                    System.out.println(method.getName() + "开始执行");
                    
                    //对被代理对象的方法进行增强，而不影响这个对象方法执行。
                    Object obj = method.invoke(myCalculator, args);
                    
                    System.out.println(method.getName() + "结束执行");

                     return obj;

        });
    }
}

```

# 3、代理做事

```java
package com.wnx.spring;
public class Test {
    public static void main(String[] args) {
        MyCalculator instance = (MyCalculator) CalculatorProxy.getInstance(new MyCalculatorImpl());
        //instance 是代理实例。
        System.out.println(instance);
        int add = instance.add(1, 2);

    }
}

```

# 4、InvocationHandler 

```java
public interface InvocationHandler {
    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable;
}
```

![](001_Spring-JDK%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E6%8A%80%E6%9C%AF.assets/image-20220510001632110.png)
