# 	NIO-Buffer-#remaining()

- 输出剩余空间大小，返回position和limit之间的元素个数

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(13);
        //WangNaiXing 占用11个字节  还有2字节的空间。
        buffer.put("WangNaiXing".getBytes());

        //通过remaining()可以输出剩余空间
        int remaining = buffer.remaining();
        System.out.println(remaining);      //2


    }
}

```

