# Kotlin-Process-Contrl

# 1、if else if.. else

- `..`叫做range表达式，相当于一个数在某个区间内。

```kotlin
package com.wnx


fun main() {
    var score = 34

    if (score in 0..60){
        println("不及格")
    }else if (score in 60..80){
        println("良好")
    }else if(score in 80..100){
        println("优秀")
    }
    
}
```

# 2、when

> 类似Java中的switch，分支选择。

- 能够直接接受到返回值，返回值为箭头指向的值。

```kotlin
package com.wnx


fun main() {
    var weekDay:Int = -1

    val info = when (weekDay) {
        1 -> "Monday"
        2 -> "Tuesday"
        3 -> "Wednesday"
        4 -> "Thursday"
        5 -> "Friday"
        6 -> "Saturday"
        7 -> "Sunday"
        else -> {
            TODO("weekDay Value is illegal！ ")
        }
    }

    print(info)

}
```

- 重点是，不再向之前switch那种，需要break关键字了。

```kotlin
package com.wnx


fun main() {
    var weekDay:Int = 3

    when (weekDay) {
        1 -> {
            print("Monday")
        }
        2 -> {
            print("Tuesday")
        }
        3 -> {
            print("Wednesday")
        }
        4 ->{
            print( "Thursday")
        }
        5 -> {
            print("Friday")
        }
        6 -> {
            print("Saturday")
        }
        7 -> {
            print("Sunday")
        }
        else -> {
            TODO("weekDay Value is illegal！ ")
        }
    }
    
}
```

# 3、for in

- 输出每一个元素

```kotlin
package com.wnx
fun main() {
    val list:List<Int> = listOf(10,20,30,40,50,60)
    for (item in list) {
        println(item)
    }
}
```

```
10
20
30
40
50
60
```

- 输出每一个Entry

```kotlin
package com.wnx
fun main() {

    var map:Map<String,String> = mapOf("1" to "王乃醒","2" to "黄洁莹" )
    for (entry in map) {
        println(entry)
    }
}
```

```
1=王乃醒
2=黄洁莹
```



# 4、forEachIndexed()

- 输出索引和元素

```kotlin
package com.wnx
fun main() {
    val list:List<Int> = listOf(10,20,30,40,50,60)
    list.forEachIndexed(){index,item->
        println("$index $item")
    }
}
```

```
0 10
1 20
2 30
3 40
4 50
5 60
```

# 5、forEach()

- 输出每一个元素

```kotlin
package com.wnx
fun main() {
    val list:List<Int> = listOf(10,20,30,40,50,60)
    list.forEach(){ println(it) }
}
```

```
10
20
30
40
50
60
```

- 输出每一个key value

```kotlin
package com.wnx
fun main() {

    var map:Map<String,String> = mapOf("1" to "王乃醒","2" to "黄洁莹" )
    map.forEach(){(k,v)->
            println("$k $v")
    }
}
```

```kotlin
1 王乃醒
2 黄洁莹
```

- 输出每一个Entry

```kotlin
package com.wnx
fun main() {

    var map:Map<String,String> = mapOf("1" to "王乃醒","2" to "黄洁莹" )
    map.forEach(){ print(it) }
}
```

```
1=王乃醒
2=黄洁莹
```

