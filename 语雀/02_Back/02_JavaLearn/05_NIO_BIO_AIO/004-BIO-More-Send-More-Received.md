# BIO-More-Send-More-Received

# 1、Use While 

```java
public class BIOClient {
    
    @SneakyThrows
    public static void main(String[] args) {
        Socket socket = new Socket("127.0.0.1",9000);
        OutputStream outputStream = socket.getOutputStream();
        PrintWriter printWriter = new PrintWriter(outputStream);
        Scanner scanner = new Scanner(System.in);
		
        //使用while循环，让客户端始终获取用户在控制台的输入，得到输入内容后，执行发送。
        while(true){
            String sendMsg = scanner.nextLine();
            printWriter.println(sendMsg);
            printWriter.flush();
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
import java.net.ServerSocket;
import java.net.Socket;

public class BIOServer {

    @SneakyThrows
    public static void main(String[] args) {
        ServerSocket serverSocket = new ServerSocket(9000);
        Socket socket = serverSocket.accept();
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));

        String msg;
        //使用while循环，多次输出客户端nei'rong
        while ((msg =bufferedReader.readLine()) != null){
                   // 输出 客户端 发送内容。
            System.out.println(msg);
        }
    }
}

```

![image-20220630205841474](004-BIO-More-Send-More-Received.assets/image-20220630205841474.png)