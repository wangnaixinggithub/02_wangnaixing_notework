# NIO-Buffer-Copy-IO-Resource

- 进行文件复制传输

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

/**
 * 使用通道+缓冲区 实现IO资源的复制。
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        //1.定义IO输入流 输出流
        FileInputStream fileInputStream = new FileInputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\QQ图片20220627101123.jpg");
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\my_workspace\\java-learn\\src\\main\\java\\com\\wnx\\model\\copy.jpg");

        //2.开启流对应Channel
        FileChannel fileChannel1 = fileInputStream.getChannel();
        FileChannel fileChannel2 = fileOutputStream.getChannel();

        //3.定义一个缓冲区
        ByteBuffer buffer = ByteBuffer.allocate(1024);
     
        //4.执行传输工作。
        while (true){
            buffer.clear();
            int flag = fileChannel1.read(buffer);
            
            if(flag == -1){
                break;
            }
            //转换为写模式
            buffer.flip();
            fileChannel2.write(buffer);
        }

    }
}
```

