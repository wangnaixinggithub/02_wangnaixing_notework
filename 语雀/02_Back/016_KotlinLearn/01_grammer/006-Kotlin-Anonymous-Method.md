# Anonymous-Method

- 匿名函数1

```kotlin
package com.wnx

private fun showMsg(str:String) {
    print(str)
}

fun main() {
   val  str1 = "WangNaiXing"
    //匿名函数，重写原来方法的逻辑。
   val length =  str1.count { it == 'W' }
    print(length)
}
```

```
1
```

- 匿名函数2

```kotlin
package com.wnx


fun main() {

    val getTwoNumberSum : (number1:Int,number2:Int) -> Int = {
          number1: Int, number2: Int ->
            println("业务处理1")
            println("业务处理2")
            println("业务处理3")

           number1+number2
    }

    val sum = getTwoNumberSum(1, 2)
    println(sum)


    val showMsg:(str:String) -> Unit = {
        println("业务处理3")
        println("业务处理2")
        println("业务处理1")

        print(it);
    }

    showMsg("王乃醒是帅哥...")



}
```

```shell
业务处理1
业务处理2
业务处理3
3
业务处理3
业务处理2
业务处理1
王乃醒是帅哥...
```

- 匿名函数3，函数方法声明可接受一个函数作为参数。

```kotlin
package com.wnx



fun main() {

   val sum = getTwoNumberAndPrint(num1 = 20, num2 = 30){
        println("计算两个数之和的结果为${it}")
    }

    println(sum)



}

   fun getTwoNumberAndPrint(num1:Int,num2:Int,showMsg:(msg:String)->Unit):Int{
       showMsg((num1+num2).toString());
     return  num1+num2
   }

```

```
计算两个数之和的结果为50
50
```

