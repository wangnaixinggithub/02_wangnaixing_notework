# Use-JS-type()

- type()函数，能帮助我们判断变量的数据类型
- 因为JS中，对象和数组对视object类型，所以我们可以使用`Array.isArray()` 做进一步区分。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13_JS的type of _判断变量类型</title>
</head>
<body>
<script>
    let num1 =  1
    let num2 = 1.1
    let num3 = -1.1
    let num4 = Number(1)

    console.log(Object.is(typeof(num1),'number'))
    console.log(Object.is(typeof(num2),'number'))
    console.log(Object.is(typeof(num3),'number'))
    console.log(Object.is(typeof(num4),'number')) //均为true

    let str1 = '王乃醒'
    let str2 = ''
    let str3 = '3'
    let str4 = String(1)

    console.log(Object.is(typeof (str1),'string'))
    console.log(Object.is(typeof (str2),'string'))
    console.log(Object.is(typeof (str3),'string'))
    console.log(Object.is(typeof (str4),'string'))//均为true

    let bool1 = true
    let bool2 = false
    let bool3 = Boolean(0)
    let bool4 = Boolean(1)

    console.log(Object.is(typeof (bool1),'boolean'))
    console.log(Object.is(typeof (bool2),'boolean'))
    console.log(Object.is(typeof (bool3),'boolean'))
    console.log(Object.is(typeof (bool4),'boolean'))//均为true



    console.log(Object.is(typeof undefined,'undefined')) //true


    function User(name,age) {
        this.name = name
        this.age = age
    }

    let obj = new User('王乃醒',25)
    let arr = [1,2,3,4]

    let defaultObj = new Date()

    console.log(Object.is(typeof obj,'object'))
    console.log(Object.is(typeof arr,'object'))
    console.log(Object.is(typeof defaultObj,'object'))//均为true

    //todo:区分对象和数组

    console.log(Array.isArray(obj)) //false
    console.log(Array.isArray(defaultObj)) //false
    
    console.log(Array.isArray(arr)) //true


    
</script>
</body>
</html>
```

