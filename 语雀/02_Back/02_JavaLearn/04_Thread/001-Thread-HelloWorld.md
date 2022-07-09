# Thread-HelloWorld

# 1、extends Thread

- 继承线程类，并重写`#run()`方法逻辑

```java
package com.wnx.model;

public class CustomerThread extends Thread{

    @Override
    public void run() {
        for (int i = 0; i < 3000; i++) {
            System.out.println("此线程正在执行中...." + i);
        }

        super.run();

    }

}

```

# 2、start()

- 创建线程类对象，调用`#start()` 启动线程。

```java
    public static void main(String[] args) {

        CustomerThread customerThread = new CustomerThread();
        customerThread.start();
        for (int i = 0; i < 2000; i++) {
            System.out.println("Main主线程执行了..."+i);
        }


    }
```

# 3、Validation Test

- 在Main主线程中，可以看到，自定义线程和Main主线程是交替执行的。

![image-20220627095023692](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220627095023692.png)