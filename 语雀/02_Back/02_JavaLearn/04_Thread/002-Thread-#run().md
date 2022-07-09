# Thread-#run()

# 1、Extend Thread

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

# 2、#run()

```java
package com.wnx.model;

public class CustomerThread extends Thread{

    public static void main(String[] args) {

        CustomerThread customerThread = new CustomerThread();
        //调用执行run()
        customerThread.run();
        for (int i = 0; i < 2000; i++) {
            System.out.println("Main主线程执行了..."+i);
        }


    }
}
```

# 3、Validation Test

- 和`#start()`不同，`#run()`是使用主Main线程去执行`CustomerThread`的`#run()`
- 线程执行完毕了，才执行Main线程下面的代码

![image-20220627095831250](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220627095831250.png)