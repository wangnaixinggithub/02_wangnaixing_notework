# Create-MutableMap-Use-#mutableMapOf()

- 创建一个可以增减的集合

```kotlin
package com.wnx
fun main() {

    var map:MutableMap<String,String> = mutableMapOf("1" to "王乃醒","2" to "黄洁莹" )
    //add element
    map += "3" to "张三"
    map.put("1","王乃醒")
    map["1"] = "王乃醒"

    //remove element
    map -= "1"
    map.remove("1")


}
```

