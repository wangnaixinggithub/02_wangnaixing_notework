# NIO-Buffer-#clear()

- 清空缓存区原来有的数据。

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(20);
        String sendData = "WangNaiXing";
        buffer.put(sendData.getBytes());

        buffer.clear();

        System.out.println(buffer);   //java.nio.HeapByteBuffer[pos=0 lim=20 cap=20]


    }
}

```

