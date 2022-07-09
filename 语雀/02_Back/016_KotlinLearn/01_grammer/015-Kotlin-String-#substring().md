# Kotlin-String-#substring()

- 截取字符串

```kotlin
package com.wnx

fun main() {

     var str:String = "王乃醒是一个大帅哥！！！"

    val index = str.indexOf("醒")

    val str2 = str.substring(0, index+1)

    println(str2) //王乃醒
}

```

