# Use-object-Define-Use-One-Class



> 使用object关键字，定义使用一次的类。有点和Java里面的匿名接口实现类有点像，用一次就丢。

```kotlin
package com.wnx
open class Student(name:String){
  open fun showMsg(){
      println("showMsg....Student...")
   }
}


fun main() {
   val stu1 = object:Student("王乃醒"){
      override fun showMsg() {
         println("哈哈哈哈")
      }
   }

   stu1.showMsg()
   
}

```

