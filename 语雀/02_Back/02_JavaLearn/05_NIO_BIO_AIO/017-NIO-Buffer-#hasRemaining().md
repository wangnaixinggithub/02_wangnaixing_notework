# NIO-Buffer-#hasRemaining()

- 判断缓存区是否还有剩余的存储空间

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(11);
        //WangNaiXing 占用11个字节  数据全部塞满了缓存区
        buffer.put("WangNaiXing".getBytes());

        boolean hasRemaining = buffer.hasRemaining();

        System.out.println(hasRemaining);    //则没有空间了，结果:false


    }
}

```

