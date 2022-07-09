# NIO-Selector-Client-Send-Msg-To-Server

# 1、NIOClient

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SocketChannel;
import java.text.SimpleDateFormat;
import java.util.Scanner;

public class NIOClient {
    @SneakyThrows
    public static void main(String[] args) {
        //1.搭建SocketChannel通道(非阻塞的)
        SocketChannel socketChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 2222));
        socketChannel.configureBlocking(false);

        //2.向缓存区写入内容 并输入进通道。
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        Scanner scanner = new Scanner(System.in);

        while (scanner.hasNext()) {
            String str = scanner.nextLine();
            String dateTime = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(System.currentTimeMillis());
            buffer.put((dateTime + str).getBytes());

            buffer.flip();
            socketChannel.write(buffer);
            buffer.clear();
        }

        socketChannel.close();

    }
}

```

# 2、NIOServer

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;

public class NIOServer {
    @SneakyThrows
    public static void main(String[] args) {
        //1.开启服务端SocketChannel(非阻塞)
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.configureBlocking(false);

        //2.channel 绑定 IPAddress
        serverSocketChannel.bind(new InetSocketAddress(2222));

        //3.Channel 注册 Selector
        Selector selector = Selector.open();
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        //4.处理Selector 事件监听。
        while (selector.select() > 0 ){
            
            Iterator<SelectionKey> selectionKeyIterator = selector.selectedKeys().iterator();

            while (selectionKeyIterator.hasNext()){
                SelectionKey selectionKey = selectionKeyIterator.next();

                if (selectionKey.isReadable()){  //1.读就绪态
                    //输出客户端发送的数据
                    SocketChannel selectableChannel = (SocketChannel) selectionKey.channel();
                    ByteBuffer buf = ByteBuffer.allocate(1024);

                    int len = 0;
                    while((len = selectableChannel.read(buf)) > 0){
                        buf.flip();
                        System.out.println(new String(buf.array(),0,len));
                        buf.clear();
                    }



                } else if (selectionKey.isAcceptable()) {        // 2.接受就绪态

                    SocketChannel socketChannel = serverSocketChannel.accept();
                    socketChannel.configureBlocking(false);
                    socketChannel.register(selector,SelectionKey.OP_READ);

                }


                selectionKeyIterator.remove();
            }
            



        }


    }
}

```

