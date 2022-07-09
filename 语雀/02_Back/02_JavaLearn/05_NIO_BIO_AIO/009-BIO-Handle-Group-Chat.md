# BIO-Handle-Group-Chat

> 使用BIO实现群聊功能

# 1、Send Msg And Received Msg From Another Socket

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

        Socket socket = new Socket("127.0.0.1",8888);
        OutputStream outputStream = socket.getOutputStream();
        Scanner scanner = new Scanner(System.in);

        //1.接受其他Socket发来的消息
        ClientReadThread clientReadThread = new ClientReadThread(socket);
        clientReadThread.start();

        while (true){
            //2.发送消息
            System.out.println("You Can Input Msg:");
            String sendMsg = scanner.nextLine();
            PrintWriter printWriter = new PrintWriter(new OutputStreamWriter(outputStream));
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
import java.net.Socket;

public class ClientReadThread extends Thread{
    private Socket socket;

    public ClientReadThread(Socket socket) {
        this.socket = socket;
    }

    @SneakyThrows
    @Override
    public void run() {
        while (true){
            InputStream inputStream = socket.getInputStream();
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));

            String msg;
            if ((msg = bufferedReader.readLine()) != null){
                System.out.println("Msg Received:" + msg);

            }
        }

    }
}

```

# 2、Server Handle Msg , Send To Other Socket

```java
package com.wnx.model;

import java.io.*;
import java.net.Socket;
import java.util.List;

public class ServerReadThread extends Thread{

    private Socket socket;

    public ServerReadThread(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        //1.获取接受到的消息，同步发给其他Socket
        try {
            InputStream inputStream = socket.getInputStream();
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
            String msg;

            while (( msg = bufferedReader.readLine()) != null){
                System.out.println("Server Received : " + msg);
                //中转消息，给其他客户端。
                this.sendMsgToAllClient(socket,msg);
            }

        } catch (Exception e) {
            //2.如果出现异常。说明某个客户端Socket和服务端断开了连接，则我们不再维护该Socket
            BIOServer.onLineSocketList.remove(socket);
            throw new RuntimeException(e);

        }


    }

    /**
     * 消息同步发送给其他Socket
     *
     * @param socket
     * @param msg    消息
     */

    private void sendMsgToAllClient(Socket socket, String msg) throws Exception {
        List<Socket> onLineSocketList = BIOServer.onLineSocketList;
        for (Socket s : onLineSocketList) {
            if (socket != s){
                OutputStream outputStream = s.getOutputStream();
                PrintWriter printWriter = new PrintWriter(new OutputStreamWriter(outputStream));
                printWriter.println(msg);
                printWriter.flush();
            }

        }

    }
}

```

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.ArrayList;
import java.util.List;

public class BIOServer {
    public static List<Socket> onLineSocketList = new ArrayList<>();

    @SneakyThrows
    public static void main(String[] args) {

        ServerSocket serverSocket = new ServerSocket(8888);
        while (true){
            Socket socket = serverSocket.accept();

            //1.维护统计在线的Socket
            onLineSocketList.add(socket);

            //2.启动一个线程，处理当前BIO连接。
            ServerReadThread serverReadThread = new ServerReadThread(socket);
            serverReadThread.start();
        }


    }
}

```

![image-20220701092426009](009-BIO-Handle-Group-Chat.assets/image-20220701092426009.png)