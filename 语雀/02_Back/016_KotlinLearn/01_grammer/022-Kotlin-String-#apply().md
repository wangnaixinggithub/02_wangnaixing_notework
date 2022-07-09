# Kotlin-String-#apply()

- 在apply方法内，this指代当前`str` 可以进行许多操作。返回值还是`str`

```kotlin
package com.wnx
fun main() {
  val str:String = "王乃醒是一个大帅哥哈哈哈，帅哥的一批";

  val str2 =   str.apply {
        println(this.length)
        println(this[3])
    }

    println(str2)  
}

```

```shell
18
是
王乃醒是一个大帅哥哈哈哈，帅哥的一批
```

