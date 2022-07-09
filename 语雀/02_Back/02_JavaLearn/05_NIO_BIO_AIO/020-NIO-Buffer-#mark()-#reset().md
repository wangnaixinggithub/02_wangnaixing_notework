# NIO-Buffer-#mark()-#reset()

- 可以使用`#mark()`进行位置标记，通过`#reset()` 进行定位到mark()标记的位置

```java
import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(30);
        buffer.put("WangNaiXing".getBytes());
        buffer.mark();
        buffer.put("Is ShuaiGe".getBytes());
        System.out.println(buffer);    //java.nio.HeapByteBuffer[pos=21 lim=30 cap=30]
        buffer.reset();
        System.out.println(buffer);   //java.nio.HeapByteBuffer[pos=11 lim=30 cap=30]
    }
}
```

