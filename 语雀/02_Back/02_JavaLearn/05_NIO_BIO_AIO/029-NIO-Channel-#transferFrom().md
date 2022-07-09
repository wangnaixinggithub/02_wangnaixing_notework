# NIO-Channel-#transferFrom()

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.Buffer;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * 从目标通道中去复制原通道数据
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        
       //1.字节输入管道
        FileInputStream is = new FileInputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\data1.txt");
        FileChannel channel1 = is.getChannel();//原通道

        //2.字节输出管道
        FileOutputStream os = new FileOutputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\data2.txt");
        FileChannel channel2 = os.getChannel();//目标通道

        //3.复制数据  channel1的数据 复制给channel2
        channel2.transferFrom(channel1,channel1.position(),channel1.size());

        channel1.close();
        channel2.close();

        System.out.println("复制完成！");

        

    }
}
```

