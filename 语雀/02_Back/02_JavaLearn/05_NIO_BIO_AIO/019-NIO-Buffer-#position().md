# NIO-Buffer-#position()

- 返回缓冲区的当前位置position

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(13);
        //WangNaiXing 占用11个字节  还有2字节的空间。
        buffer.put("WangNaiXing".getBytes());

        int position = buffer.position();
        System.out.println(position);   //11


    }
}

```

- 设置当前的opsition

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(13);

        System.out.println(buffer);        //java.nio.HeapByteBuffer[pos=0 lim=13 cap=13]

        //设置position
        buffer.position(5);

        System.out.println(buffer);       //java.nio.HeapByteBuffer[pos=5 lim=13 cap=13]



    }
}
```

