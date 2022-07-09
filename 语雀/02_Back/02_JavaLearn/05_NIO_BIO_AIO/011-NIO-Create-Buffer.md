# NIO-Create-Buffer

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        //创建缓冲区，缓存区存存储数据的类型可以是 byte int short long float  double
        ByteBuffer byteBuffer = ByteBuffer.allocate(10);
        IntBuffer intBuffer = IntBuffer.allocate(10);
        ShortBuffer shortBuffer = ShortBuffer.allocate(10);
        LongBuffer longBuffer = LongBuffer.allocate(10);
        FloatBuffer floatBuffer = FloatBuffer.allocate(10);
        DoubleBuffer doubleBuffer = DoubleBuffer.allocate(10);
    }
}

```

