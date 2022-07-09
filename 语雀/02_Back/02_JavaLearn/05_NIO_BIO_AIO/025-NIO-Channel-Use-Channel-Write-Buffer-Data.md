# NIO-Channel-Use-Channel-Write-Buffer-Data

- 使用通道，将缓冲区的数据写入文件中。

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileOutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * 将缓冲区的数据通过 通道写入
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //1.缓存区分配空间，写入数据并将位置归0
        ByteBuffer buffer = ByteBuffer.allocate(100);
        buffer.put("王乃醒是一个大帅哥哦！".getBytes());
        buffer.flip();
        
        //2.开启一个文件通道
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\1.txt");
        FileChannel fileChannel = fileOutputStream.getChannel();
        
        //3.将缓存区的数据写入通道 通道传输给此文件
        fileChannel.write(buffer);
       
        //4.关闭通道
        fileChannel.close();
    }
}
```

