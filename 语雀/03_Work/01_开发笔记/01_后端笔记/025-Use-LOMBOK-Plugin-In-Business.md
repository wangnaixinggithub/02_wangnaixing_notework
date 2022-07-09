# Use-LOMBOK-Plugin-In-Business

# 1、注解

## 1.1、@Builder

> 方便创建对象

```java
package com.wnx.mall.tiny.example;

import lombok.Builder;
import lombok.ToString;

@Builder
@ToString
public class BuilderExample {
    private Long id;
    private String name;
    private Integer age;


    public static void main(String[] args) {
        BuilderExample example = BuilderExample.builder()
                .id(1L)
                .name("test")
                .age(20)
                .build();
        System.out.println(example);
    }
}

```

## 1.2、@Cleanup

> 流，不用trycatch和close

```java
package com.wnx.mall.tiny.example;

import lombok.Cleanup;

import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;

public class CleanupExample {
    public static void main(String[] args) throws IOException {
        String inStr = "Hello World";
        //使用输入输出六自动关闭，无需编写try catch和调用close()方法
        @Cleanup ByteArrayInputStream in = new ByteArrayInputStream(inStr.getBytes("UTF-8"));
        @Cleanup ByteArrayOutputStream out = new ByteArrayOutputStream();
        byte[] b = new byte[1024];
        while (true){
            int r = in.read(b);
            if (r == -1) break;
            out.write(b,0,r);
        }

        String outStr = out.toString();
        System.out.println(outStr);



    }
}

```

## 1.3、@XXXArgsConstructor

>  无参，有参，指定参数构造器。

```java
package com.wnx.mall.tiny.example;

import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;
import lombok.NonNull;
import lombok.RequiredArgsConstructor;

@NoArgsConstructor
@RequiredArgsConstructor(staticName = "of")
@AllArgsConstructor
public class ConstructorExample {
    @NonNull
    private Long id;
    private String name;
    private Integer age;

    public static void main(String[] args) {
        //无参构造器
        ConstructorExample example1 = new ConstructorExample();
        //全部参数构造器
        ConstructorExample example2 = new ConstructorExample(1L,"test",20);
        //@NonNull注解的必须参数构造器
        ConstructorExample example3 = ConstructorExample.of(1L);
    }
}

```

## 1.4、@Data

> @Getter @Setter @ToString @EqualsAndHashCode 注解之和！

```java
package com.wnx.mall.tiny.example;

import lombok.Data;
import lombok.EqualsAndHashCode;
import lombok.NonNull;
@Data
public class DataExample {
    @NonNull
    private Long id;
    @EqualsAndHashCode.Exclude
    private String name;
    @EqualsAndHashCode.Exclude
    private Integer age;


    public static void main(String[] args) {
        //@RequiredArgsConstructor已生效
        DataExample example1 = new DataExample(1L);
        //@Getter @Setter已生效
        example1.setName("test");
        example1.setAge(20);
        //@ToString已生效
        System.out.println(example1);
        DataExample example2 = new DataExample(1L);
        //@EqualsAndHashCode已生效
        System.out.println(example1.equals(example2));
    }

}

```

## 1.5、@EqualsAndHashCode

```java
package com.wnx.mall.tiny.example;


import lombok.EqualsAndHashCode;
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@EqualsAndHashCode
public class EqualsAndHashCodeExample {
    private Long id;
    @EqualsAndHashCode.Exclude
    private String name;
    @EqualsAndHashCode.Exclude
    private Integer age;

    public static void main(String[] args) {
        EqualsAndHashCodeExample example1 = new EqualsAndHashCodeExample();
        example1.setId(1L);
        example1.setName("test");
        example1.setAge(20);
        EqualsAndHashCodeExample example2 = new EqualsAndHashCodeExample();
        example2.setId(1L);
        //equals方法只对比id，返回true
        System.out.println(example1.equals(example2));
    }
}

```

## 1.6、@GET(lazy=true)

> 懒加载

```
package com.wnx.mall.tiny.example;

import lombok.Getter;

public class GetterLazyExample {
    @Getter(lazy = true)
    private final double[] cached = expensive();

    private double[] expensive() {
        double[] result = new double[1000000];
        for (int i = 0; i < result.length; i++) {
            result[i] = Math.asin(i);
        }
        return result;
    }

    public static void main(String[] args) {
        //使用Double Check Lock 样板代码对属性进行懒加载
        GetterLazyExample example = new GetterLazyExample();
        System.out.println(example.getCached().length);
    }
}

```

## 1.7、@Getter@Setter

```java
package com.wnx.mall.tiny.example;

import lombok.Getter;
import lombok.Setter;

public class GetterSetterExample {
    @Getter
    @Setter
    private String name;
    @Getter
    @Setter
    private Integer age;


    public static void main(String[] args) {
        GetterSetterExample example = new GetterSetterExample();
        example.setName("test");
        example.setAge(20);
        System.out.printf("name:%s age:%d",example.getName(),example.getAge());
    }
}

```

## 1.8、@LOG @LogSlf4j

> 日志

```java
package com.wnx.mall.tiny.example;

import lombok.extern.java.Log;

@Log
public class LogExample {
    public static void main(String[] args) {
        log.info("level info");
        log.warning("level warning");
        log.severe("level severe");
    }
}

```

```java
package com.wnx.mall.tiny.example;

import lombok.extern.slf4j.Slf4j;

@Slf4j
public class LogSlf4jExample {
    public static void main(String[] args) {
        log.info("level:{}","info");
        log.warn("level:{}","warn");
        log.error("level:{}", "error");
    }
}
```

## 1.9、@NonNull

> 字段不为空，空了就抛出空指针异常。

```java
package com.wnx.mall.tiny.example;



import lombok.NonNull;

public class NonNullExample {
    private String name;
    private NonNullExample(@NonNull String name){
        this.name = name;
    }

    public static void main(String[] args) {
        new NonNullExample("test");
        //会抛出NullPointerException
        new NonNullExample(null);
    }
}

```

## 1.11、@SneakyThrows

> 自动抛出异常，无需处理

```java
package com.wnx.mall.tiny.example;

import lombok.SneakyThrows;

import java.io.UnsupportedEncodingException;

public class SneakyThrowsExample {
    //自动抛出异常，无需处理

    @SneakyThrows
    public static byte[] str2byte(String str){
        return str.getBytes("UTF-8");
    }

    public static void main(String[] args) {
        String str = "Hello World!";
        System.out.println(str2byte(str).length);
    }
}

```

## 1.12、@Synchronized

> 让方法瞬间变成同步方法

```java
package com.wnx.mall.tiny.example;

import lombok.*;

@Data
public class SynchronizedExample {

    @NonNull
    private Integer count;

    @Synchronized
    @SneakyThrows
    public void reduceCount(Integer id) {
        if (count > 0) {
            Thread.sleep(500);
            count--;
            System.out.println(String.format("thread-%d count:%d", id, count));
        }
    }

    public static void main(String[] args) {
        //添加@Synchronized三个线程可以同步调用reduceCount方法
        SynchronizedExample example = new SynchronizedExample(20);
        new ReduceThread(1, example).start();
        new ReduceThread(2, example).start();
        new ReduceThread(3, example).start();
    }


    @RequiredArgsConstructor
    static class ReduceThread extends Thread {
        @NonNull
        private Integer id;
        @NonNull
        private SynchronizedExample example;

        @Override
        public void run() {
            while (example.getCount() > 0) {
                example.reduceCount(id);
            }
        }
    }
}

```

## 1.13、@ToString

> 自动实现toString方法

```java
package com.wnx.mall.tiny.example;

import lombok.ToString;

@ToString
public class ToStringExample {
    @ToString.Exclude
    private Long id;
    private String name;
    private Integer age;
    public ToStringExample(Long id,String name,Integer age){
        this.id =id;
        this.name = name;
        this.age = age;
    }
    public static void main(String[] args) {
        ToStringExample example = new ToStringExample(1L,"test",20);
        //自动实现toString方法，输出ToStringExample(name=test, age=20)
        System.out.println(example);
    }
}

```

## 1.14、@Val

> JDK 的var一样，局部变量的类型推断。

```java
package com.wnx.mall.tiny.example;
import lombok.val;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;

public class ValueExample {

    private static void example(){
        //val代替ArrayList<String> 和String类型
        val example = new ArrayList<String>();
        example.add("Hello world!");
        val foo = example.get(0);
        System.out.println(foo.toLowerCase());
    }

    private static void example2(){
        //val代替Map.Entry<Integer,String>类型
        val map = new HashMap<Integer,String>();
        map.put(0,"zero");
        map.put(5,"five");
        for (val entry : map.entrySet()) {
            System.out.printf("%d: %s\n",entry.getKey(),entry.getValue());
        }
    }


    public static void main(String[] args) {
        example();
        example2();
    }
}

```

## 1.15、@With

> 对象属性复制

```java
package com.wnx.mall.tiny.example;

import lombok.AllArgsConstructor;
import lombok.ToString;
import lombok.With;

@With
@AllArgsConstructor
@ToString
public class WithExample {
    private Long id;
    private String name;
    private Integer age;

    public static void main(String[] args) {
        WithExample example1 = new WithExample(1L, "test", 20);
        WithExample example2 = example1.withAge(22);
        //将原对象进行clone并设置age，返回false
        System.out.println(example2);
        System.out.println(example1.equals(example2));
    }
}

```



# 2、原理

> taget目录，会自动补上该有的代码。IDEA必须装LOMBOk插件！

![image-20220104163049259](https://s2.loli.net/2022/01/04/857w31Ec9QxSANd.png)