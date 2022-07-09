# Kotlin-Class-Extends

```kotlin
package com.wnx
//1.父类要使用open关键字，才能被子类继承。 并且方法使用了open关键字，该方法才能被子类重写。
open class Person(name:String){

  open fun showMsg(){
      println("Person ...Msg...")
   }
}

//继承Person，提供满参构造方法，并对外暴露Getter/Setter方法（Use var）
class Student(var name: String,var stuNo:String):Person(name){
   override fun showMsg() {
      super.showMsg()
      println("Student...Msg...")
   }
}


fun main() {
   var student = Student("王乃醒","201501818134")

   student.showMsg()
  println(student.name)
  println(student.stuNo)
}
```

