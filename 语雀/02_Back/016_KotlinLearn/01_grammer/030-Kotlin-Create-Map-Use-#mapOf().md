# Kotlin-Create-Map-Use-#mapOf()

```kotlin
package com.wnx
fun main() {
    //1.创建方式1
    var map:Map<String,String> = mapOf("1" to "王乃醒","2" to "黄洁莹" )

    println(map) //{1=王乃醒, 2=黄洁莹}

    var map2:Map<String,String> = mapOf(Pair("1","黄洁莹"), Pair("2","黄洁莹"))

    print(map2) //{1=黄洁莹, 2=黄洁莹}

}
```

