# BIO-Use-ThreadPool-Handle-Client-Msg

# 1、Use Thread Pool And Queue Manger Socket Connection

- 通过线程池，和任务队列来有效管理客户端线程的创建问题。

- JDK的线程池维护一个消息队列和N个活跃的线程，对消息队列中Socket任务进行处理

![image-20220701003953971](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220701003953971.png)

# 2、Code Impl

```java
package com.wnx.model;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

/**
 * 处理Socket服务的线程池
 */
public class HandlerSocketServerPool {
    private ExecutorService executorService;

    public HandlerSocketServerPool(int corePoolSize,int capacity) {
        //线程数最大数量
        int maximnumberedPool = 20;
        //线程存活
        long keepAliveTime = 2;

        executorService = new ThreadPoolExecutor(
                corePoolSize,
                maximnumberedPool,
                keepAliveTime,
                TimeUnit.MINUTES,
                new ArrayBlockingQueue<Runnable>(capacity));
    }


    /**
     * 线程池需要执行的命令
      * @param command 指令
     */
    public void execute(Runnable command){
        executorService.execute(command);
    }

    
}

```

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.Socket;

/**
 * 线程池包装的可执行对象
 */
public class ServerRunnableTarget  implements  Runnable{
    private Socket socket;

    public ServerRunnableTarget(Socket socket) {
        this.socket = socket;
    }

    @SneakyThrows
    @Override
    public void run() {
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String msg;
        while ((msg =bufferedReader.readLine()) != null){
            System.out.println("Msg:" + msg);
        }
    }
}

```

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.net.ServerSocket;
import java.net.Socket;

public class BIOServer {
    @SneakyThrows
    public static void main(String[] args) {
          ServerSocket serverSocket = new ServerSocket(9000);
          while (true){
              Socket socket = serverSocket.accept();
              ServerRunnableTarget serverRunnableTarget = new ServerRunnableTarget(socket);
              HandlerSocketServerPool handlerSocketServerPool = new HandlerSocketServerPool(5,20);

              handlerSocketServerPool.execute(serverRunnableTarget);
          }
    }
}

```

- 同样也是开启多个线程的客户端发送消息。

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

public class BIOClient {
    @SneakyThrows
    public static void main(String[] args) {
        Socket socket = new Socket("127.0.0.1",9000);
        OutputStream outputStream = socket.getOutputStream();
        PrintWriter printWriter = new PrintWriter(new OutputStreamWriter(outputStream));
        Scanner scanner = new Scanner(System.in);

        while (true){
            String sendMsg = scanner.nextLine();
            printWriter.println(sendMsg);
            printWriter.flush();
        }

    }
}

```

![image-20220701004112160](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220701004112160.png)

# 3、Cause problems

- 伪异步旧采用了线程池实现，因此避免了为每个请求创建一个独立线程造成线程资源耗尽的问题， 但由于底层 依然是采用的同步阻塞模型，因此无法从根采上解决问题。
-  如果单个消息处理的缓慢，或者服务器线程池中的全部线程都被阻塞，那么后续socket的I/O消息 都将在队列 中排队。新的Socket请求将被拒绝，客户端会发生大量连接超时。