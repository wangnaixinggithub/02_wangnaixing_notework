# NIO-Channel-#getChannel()

> 如果获取到通道呢？

# 1、FileInputStream

```java
package com.wnx.model;

import lombok.SneakyThrows;
import java.io.FileInputStream;
import java.nio.channels.FileChannel;

public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //1。通过文件输入流获取文件通道 channel
      FileInputStream fileInputStream = new FileInputStream("D:\\迅雷下载\\bio、nio、aio.pdf");
      FileChannel channel = fileInputStream.getChannel();

    }
}
```

# 2、FileOutputStream

```java
package com.wnx.model;

import lombok.SneakyThrows;
import java.io.FileOutputStream;
import java.nio.channels.FileChannel;

public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //1。通过文件输出流获取文件通道 channel
      FileOutputStream fileOutputStream = new FileOutputStream("D:\\迅雷下载\\bio、nio、aio.pdf");
      FileChannel channel = fileOutputStream.getChannel();
    }
}
```

# 3、Socket

```java
package com.wnx.model;

import lombok.SneakyThrows;
import java.net.Socket;
import java.nio.channels.SocketChannel;

public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //从Socket中获取到SocketChannel通道 channel
     Socket socket = new Socket("127.0.0.1",9000);
     SocketChannel channel = socket.getChannel();
    }
}
```

# 4、ServerSocket

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.net.ServerSocket;
import java.net.Socket;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;

public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //从ServerSocket中获取到ServerSocketChannel 通道
        ServerSocket serverSocket = new ServerSocket(9000);
        ServerSocketChannel serverSocketChannel = serverSocket.getChannel();
    }
}
```

# 5、DatagramSocket

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.net.DatagramSocket;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.channels.DatagramChannel;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;

public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //从DataGramSocket中获取到DataGramChannel
        DatagramSocket datagramSocket = new DatagramSocket(8080);
        DatagramChannel datagramSocketChannel = datagramSocket.getChannel();
    }
}
```

# 6、ServerSocketChannel

```java
       //1.打开一个ServerSocketChannel
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
```

