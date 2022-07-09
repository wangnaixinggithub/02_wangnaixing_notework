

# NIO-Selector-#serverSocketChannel.register()

- 使用Selector，通常需要注册到Channel中，监听接收事件。

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.net.InetSocketAddress;
import java.nio.Buffer;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;

/**
 * 从目标通道中去复制原通道数据
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //1.打开一个ServerSocketChannel
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();

        //2.切换到非阻塞形式
        serverSocketChannel.configureBlocking(false);


        //3.通道绑定Socket地址
        InetSocketAddress inetSocketAddress = new InetSocketAddress(9000);
        serverSocketChannel.bind(inetSocketAddress);


        //将Selector 注册到Channel中，监听接收事件。
        Selector selector = Selector.open();
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
    }
}
```

Selector可以实现：一个I/O线程可以并发处理N个客户端连接和读写操作，这从根本上解决了传统同步 阻塞I/O一连接一线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升。

![image-20220702080607614](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702080607614.png)