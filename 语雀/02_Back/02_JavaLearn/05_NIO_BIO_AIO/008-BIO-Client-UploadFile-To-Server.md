# BIO-Client-UploadFile-To-Server

> 基于BIO进行客户端文件上传到服务端保存。

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.DataOutputStream;
import java.io.FileInputStream;
import java.io.OutputStream;
import java.net.Socket;

public class BIOClient {
    @SneakyThrows
    public static void main(String[] args) {
        Socket socket = new Socket("127.0.0.1",8888);
       
       //1.Socket的输出流
        OutputStream outputStream = socket.getOutputStream();
        DataOutputStream dataOutputStream = new DataOutputStream(outputStream);
        dataOutputStream.writeUTF(".pdf");

        //2.复制本地PDF文件给  DataOutputStream
        FileInputStream fileInputStream = new FileInputStream("D:/my_workspace/java-learn/src/main/java/com/wnx/model/wnx.pdf");
        byte[] buffer = new byte[1024];
        int len;
        while ((len = fileInputStream.read(buffer)) > 0){
             dataOutputStream.write(buffer,0,len);
        }
        dataOutputStream.flush();

        //3.告诉服务端，这边传输完毕
        socket.shutdownOutput();
        


    }
}

```

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.DataInputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.UUID;

public class ServerReadThread extends Thread {
    private Socket socket;

    public ServerReadThread(Socket socket) {
        this.socket = socket;
    }

    @SneakyThrows
    @Override
    public void run() {
        InputStream inputStream = socket.getInputStream();
        DataInputStream dataInputStream = new DataInputStream(inputStream);
        String suffix = dataInputStream.readUTF();
        System.out.println("File Type Is :" + suffix);

        FileOutputStream fileOutputStream = new FileOutputStream("D:/my_workspace/java-learn/src/main/java/com/wnx/model/"+ UUID.randomUUID().toString()+suffix);
        int len;
        byte[] buffer = new byte[1024];

        while ((len = dataInputStream.read(buffer)) > 0){
           fileOutputStream.write(buffer,0,len);
        }
        fileOutputStream.close();
        System.out.println("SUCCESS SAVE File!");


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
          ServerSocket serverSocket = new ServerSocket(8888);
          while (true){
              Socket socket = serverSocket.accept();
              ServerReadThread serverReadThread = new ServerReadThread(socket);
              serverReadThread.start();
          }
    }
}

```

