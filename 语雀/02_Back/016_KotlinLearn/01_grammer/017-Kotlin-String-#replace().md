# Kotlin-String-#replace()

- String的字符串替换方法

```kotlin
package com.wnx

fun main() {
       val str:String = "WW王乃醒WwWWwW"

       val replace = str.replace('W', ' ', true)

        println(replace)
}

```

```shell
  王乃醒      
```

- 可以使用lambda表达式进行指定替换操作规则。

```kotlin
package com.wnx

fun main() {
       val str:String = "WW王乃醒WwWWwW"

       val replace = str.replace(Regex("[Ww]")){
            when(it.value){
                "W"->"9"
                "w"->"8"
                else->it.value
            }
       }

        println(replace)
}
```

```shell
99王乃醒989989
```

