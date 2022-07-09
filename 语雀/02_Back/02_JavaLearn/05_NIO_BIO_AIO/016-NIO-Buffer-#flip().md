# NIO-Buffer-#flip()

- 可以重设limit position 的量

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(20);
        //WangNaiXing 占用11个字节
        buffer.put("WangNaiXing".getBytes());
        System.out.println(buffer);   //java.nio.HeapByteBuffer[pos=11 lim=20 cap=20]
         //java.nio.HeapByteBuffer[pos=0 lim=11 cap=20]
        buffer.flip();
        System.out.println(buffer);        //java.nio.HeapByteBuffer[pos=0 lim=11 cap=20]


    }
}
```

