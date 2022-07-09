# NIO-Channel-Use-Channel-read-Data-To-Buffer-Data

- 将IO资源数据读到缓存区中

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileInputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * 将缓冲区的数据通过 通道写入
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //1.定义一个缓冲区
       ByteBuffer buffer = ByteBuffer.allocate(1024);

       //2.利用文件输入流 开启一个文件通道。
       FileInputStream fileInputStream = new FileInputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\1.txt");
        FileChannel channel = fileInputStream.getChannel();

        //3.将数据写入缓存区
        channel.read(buffer);

        //4.把缓存区的结果拿出来
        String result = new String(buffer.array(),0,buffer.remaining());
        System.out.println(result);

        //5.关闭通道
        channel.close();
    }
}
```

