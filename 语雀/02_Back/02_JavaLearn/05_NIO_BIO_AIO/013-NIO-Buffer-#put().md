# NIO-Buffer-#put()

- 使用`#put()` 方法可以往缓存区里头发送数据。可以看到，内部会把数据存起来。

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(20);
        String sendData = "WangNaiXing";
        buffer.put(sendData.getBytes());
        System.out.println(buffer);   //java.nio.HeapByteBuffer[pos=11 lim=20 cap=20]
    }
}
```

