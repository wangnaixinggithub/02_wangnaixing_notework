# NIO-Channel-Scatter-Gathering

分散读取（Scatter)：是指把Channel通道的数据读取入到多个缓存区中去。

聚集写入（Gathering)：是指将多个Buffer中的数据聚集到Channel。

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.Buffer;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * 使用通道+缓冲区 实现IO资源的复制。
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        
        //1.开启IO资源的通道
        FileInputStream fileInputStream = new FileInputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\data1.txt");
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\data2.txt");
        FileChannel channel1 = fileInputStream.getChannel();
        FileChannel channel2 = fileOutputStream.getChannel();

        //2.定义多个缓存区做数据分散
        ByteBuffer buffer1 = ByteBuffer.allocate(4);
        ByteBuffer buffer2 = ByteBuffer.allocate(1024);
        ByteBuffer[] buffers = new ByteBuffer[]{buffer1,buffer2};

        //3.通道的数据读入缓冲区
        long flag = channel1.read(buffers);

         //4.转换为写模式
        for (ByteBuffer buffer : buffers) {
            buffer.flip();
        }


        //5.缓冲区的数据写入通道
        channel2.write(buffers);


        //6.通道关闭
        channel1.close();
        channel2.close();

      

    }
}
```

