# Use -!!- Invoke-Always-Null-Or-NotNull

- 使用`!!`就断言了这个调用必须执行，断言了`str1`不为空了。

```kotlin
package com.wnx

fun main() {

    var str1:String ? = "wangnaixing"
    //不管st1s是不是空格，我这都会执行。
    val length = str1!!.length
    
}

```

