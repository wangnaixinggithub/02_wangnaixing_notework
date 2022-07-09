# Create-Class

- 满参构造函数，并提供Getter Setter

```kotlin
package com.wnx



class Student(var id:Int, var name: String, var age:Int){


   fun showMsg(){
      println("${this.id} ${this.name} ${this.age} ")

   }

}

fun main() {
 var student01 = Student(1,"王乃醒",26)

    println(student01.id)
    println(student01.name)
    println(student01.age)


    student01.id = 2
    student01.name = "黄洁莹"
    student01.age = 24

    student01.showMsg()

}
```

- 多个构造函数

```kotlin
package com.wnx


class Student(var id:Int, var name: String, var age:Int){
    //构造函数重载
    constructor(id: Int):this(id,"王乃醒",25)
    constructor(id: Int,name: String):this(id,name,39)
    fun showMsg(){
        println("${this.id} ${this.name} ${this.age} ")
        
    }
    
}

fun main() {
    var stu1 = Student(1)
    var sut2 = Student(2,"王乃醒")
    
}
```

- 初始化代码块

> 类似Java的static代码块，在类加载成字节码的时候就会执行。

```kotlin
package com.wnx


class Student(var id:Int, var name: String, var age:Int){
   init {
       print("初始化代码块，在类构造后马上执行！")
   }


   constructor(id: Int):this(id,"王乃醒",25)
   constructor(id: Int,name: String):this(id,name,39)
   fun showMsg(){
      println("${this.id} ${this.name} ${this.age} ")

   }

}

fun main() {
  var stu1 = Student(1)
}
```

