# Kotlin-String-#split()

- 将字符串按照某个字符进行拆分，放到一个集合中。使用集合的解构，拿到str1,str2,str3并打印。

```kotlin
package com.wnx

fun main() {
        val str:String = "王,乃,醒"
        val strList = str.split(",")

        val  (str1,str2,str3) = strList

        println(str1)
        println(str2)
        println(str3)
}
//王
//乃
//醒
```

