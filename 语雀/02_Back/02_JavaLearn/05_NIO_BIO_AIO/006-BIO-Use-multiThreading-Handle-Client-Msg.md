# BIO-Use-multiThreading-Handle-Client-Msg

# 1、One Socket Connection Create => One Thread Start

- 在服务端，使用开启多个线程的方式，处理多个客户端发来的请求。

![image-20220701000932389](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220701000932389.png)

# 2、Code Impl

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

public class BIOServer {

    @SneakyThrows
    public static void main(String[] args) {
        ServerSocket serverSocket = new ServerSocket(9000);

        //客户端每发起一个连接，就创建一个线程来处理此数据交互。
        while(true){
            Socket socket = serverSocket.accept();
            ServerThreadReader serverThreadReader = new ServerThreadReader(socket);
            serverThreadReader.start();
        }


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

public class ServerThreadReader extends  Thread{
    private Socket socket;

    public ServerThreadReader(Socket socket) {
        super();
        this.socket = socket;
    }



    @SneakyThrows
    @Override
    public void run() {
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String msg;
        while ((msg = bufferedReader.readLine()) != null){
            System.out.println("Received Content:" + msg);

        }


    }
}
```

- 打开多个进程的客户端，分别给服务端发送消息。

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.OutputStream;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;

public class BIOClient {
    
    @SneakyThrows
    public static void main(String[] args) {
        Socket socket = new Socket("127.0.0.1",9000);
        OutputStream outputStream = socket.getOutputStream();
        PrintWriter printWriter = new PrintWriter(outputStream);
        Scanner scanner = new Scanner(System.in);

        while(true){
            String sendMsg = scanner.nextLine();
            printWriter.println(sendMsg);
            printWriter.flush();
        }

    }
}

```

# 3、Cause problems

 每个Socket接收到，都会创建一个线程，线程的竞争、切换上下文影响性能；



 每个线程都会占用栈空间和CPU资源； 并不是每个socket都进行lO操作，无意义的线程处理； 



客户端的并发访问增加时。服务端将呈现1:1的线程开销，访问量越大，系统将发生线程栈溢出， 线程创建失败，最终导致进程宕机或者僵死，从而不能对外提供服务。  