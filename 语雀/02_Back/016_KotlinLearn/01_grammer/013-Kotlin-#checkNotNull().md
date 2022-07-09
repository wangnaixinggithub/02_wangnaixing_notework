# Kotlin-#checkNotNull()

- 校验参数不为空。如果为空了就会抛出异常

```c++
package com.wnx

fun main() {

    var str1:String ? = "wangnaixing"
   
    //校验不为空
    checkNotNull(str1);
   
    val length = str1!!.length

}

```

