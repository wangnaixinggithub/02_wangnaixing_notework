# JS-Handle-Number-Calculate-Precision

# 1、Precision Question

- 演示精度问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Handle-Number-Calculate-Precision</title>
</head>
<body>
<script>
    
//1.number 小数运算在JS 中，会出现精度丢失。     
let num1 = 0.1
let num2 = 0.2

let sum = num1 + num2
console.log(Object.is(0.30000000000000004,sum)) //true!


</script>
</body>
</html>
```

# 2、Float  => Integer

- 化浮为整。变成整数运算，就不会有这个问题了。		

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Handle-Number-Calculate-Precision</title>
</head>
<body>
<script>


let num1 = 0.1
let num2 = 0.2


num1 = num1 * 10
num2 = num2 * 10

let sum = (num1 + num2) /10

console.log(Object.is(0.3,sum))


</script>
</body>
</html>
```

- 动态获取精度

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Handle-Number-Calculate-Precision</title>
</head>
<body>
<script>


let num1 = 0.000001
let num2 = 0.02

//1.取最小的数的位数
let positionNumber = num1.toString().split('.')[1].length
console.log(Object.is(positionNumber,6))

//计算出精度
let numberPrecision = Math.pow(10,positionNumber)
console.log(Object.is(1000000,numberPrecision))


//3.数乘以精度得到整数
num1 = num1 * numberPrecision
num2 = num2 * numberPrecision

//4.整数求和 再除以精度，可以确保精度不丢失。
let sum = (num1 + num2) / numberPrecision
console.log(Object.is(0.020001,sum))


</script>
</body>
</html>
```

# 3、Use Function

- 可以抽离成函数。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Handle-Number-Calculate-Precision</title>
</head>
<body>
<script>


let num1 = 0.000001
let num2 = 0.02

console.log(Object.is(0.020001,add(num1,num2)))

function add(num1,num2){

    let positionNumber1 = num1.toString().split('.')[1].length
    let positionNumber2 = num2.toString().split('.')[1].length

    //1. Find Max PositionNumber
    let  maxPositionNumber = positionNumber1
    if (positionNumber1 < positionNumber2){
        maxPositionNumber = positionNumber2
    }

    //2.计算出精度
    let numberPrecision = Math.pow(10,maxPositionNumber)


    //3.数乘以精度得到整数
    num1 = num1 * numberPrecision
    num2 = num2 * numberPrecision

   return (num1 + num2) / numberPrecision
}
</script>
</body>
</html>
```

- 妈的，忘记考虑,传入参数可能是整数了。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Handle-Number-Calculate-Precision</title>
</head>
<body>
<script>


let num1 = 1
let num2 = 2.00000200010311111

console.log(Object.is(3.00000200010311111,add(num1,num2)))

function add(num1,num2){
    num1 = num1.toString()
    num2 = num2.toString()
    let positionNumber1 =  0
    let positionNumber2 = 0


    //1. Handler num1 num2 是整数的情况
    if (!Object.is(-1,num1.indexOf('.') )){positionNumber1 = num1.split('.')[1].length}
    if (!Object.is(-1,num2.indexOf('.'))){positionNumber2 = num2.split('.')[1].length}





    //1. Find Max PositionNumber
    let  maxPositionNumber = positionNumber1
    if (positionNumber1 < positionNumber2){
        maxPositionNumber = positionNumber2
    }

    //2.计算出精度
    let numberPrecision = Math.pow(10,maxPositionNumber)


    //3.数乘以精度得到整数
    num1 = num1 * numberPrecision
    num2 = num2 * numberPrecision

   return (num1 + num2) / numberPrecision
}
</script>
</body>
</html>
```

