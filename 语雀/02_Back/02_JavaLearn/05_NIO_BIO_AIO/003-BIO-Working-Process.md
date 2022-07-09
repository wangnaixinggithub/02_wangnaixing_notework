

# BIO-Working-Process

- 网络编程的基本模型是Client/Server模型，也就是两个进程之间进行相互通信，其中服务端提供位置（绑 定IP地址和端口)

- 客户端通过连接操作向服务端监听的端口地址发起连接请求，基于TCP协议下 进行三次握手连接，连接成功后，双方通过网络套接字（Socket）进行通信。

- 传统的同步阻塞模型开发中，服务端`ServerSocket`负责绑定IP地址，启动监听端口

- 客户端Socket负责 发起 连接操作。连接成功后，双方通过输入和输出流进行同步阻塞式通信。 

- 基于BIO模式下的通信，客户端-服务端是完全同步，完全藕合的。

![image-20220629235225648](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220629235225648.png)

# 1、Client Send Message To Server

> 客户端向服务端发送消息

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * BIO通信模式下的服务端
 */
public class BIOServer {
    @SneakyThrows
    public static void main(String[] args) {
        
        //1.通过ServerSocket注册端口
        ServerSocket  serverSocket = new ServerSocket(9000);

        //2.  监听客户端的Socket
        Socket socket = serverSocket.accept();

        //3.从Socket中获取字节输入流  进行读的操作
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));

        //4.读取字符并打印
        String msg;

        if ((msg = bufferedReader.readLine())!= null){
            System.out.println("BIOServer:Messages are received" + msg);
        }


    }
}
```

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.*;
import java.net.Socket;

/**
 * BIO通信模式下的客户端
 */
public class BIOClient {
    @SneakyThrows
    public static void main(String[] args) {
        //1.通过Socket对象请求与服务端的连接
        Socket socket = new Socket("127.0.0.1",9000);
         //2.从Socket获取字节输出流，写入数据。
        OutputStream outputStream = socket.getOutputStream();
        
        PrintWriter printWriter = new PrintWriter(outputStream);
        printWriter.println("WangNaiXing is Cool Boy ");
        printWriter.flush();

    }
}

```

![image-20220629235749001](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220629235749001.png)

# 2、Server Send Message To Client

> 服务端向客户端发送消息

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * BIO通信模式下的服务器
 */
public class BIOServer {
    @SneakyThrows
    public static void main(String[] args) {
        ServerSocket serverSocket = new ServerSocket(9000);
        Socket socket = serverSocket.accept();

        OutputStream outputStream = socket.getOutputStream();
        PrintWriter printWriter = new PrintWriter(outputStream);
		//服务端写入消息
        printWriter.println("WangNai is Brilliant Programmer");

        printWriter.flush();

    }
}

```

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.*;
import java.net.Socket;

/**
 * BIO通信模式下的客户端
 */
public class BIOClient {
    @SneakyThrows
    public static void main(String[] args) {
        Socket socket = new Socket("127.0.0.1",9000);
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
		//客户端读取。
        String msg;
        if ((msg = bufferedReader.readLine()) != null ){
            System.out.println(msg);
        }


    }
}

```

![image-20220630000151021](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220630000151021.png)