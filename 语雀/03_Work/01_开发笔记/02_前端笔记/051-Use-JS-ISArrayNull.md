

# Use-JS-ISArrayNull?

# 1、Array.prototype.isPrototypeOf()

- 判断出一个变量的类型为数组

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>判断一个数组是一个空数组</title>
</head>
<body>
<script>
    //1.数组
    let emtryArr = []
    Object.is(true,Array.prototype.isPrototypeOf(emtryArr)) 

    //2.对象
    let emtryObj = {}
     Object.is(false,Array.prototype.isPrototypeOf(emtryObj))

    //3.number
    let num1 = 0
    Object.is(false,Array.prototype.isPrototypeOf(num1))

    //4. string
    let str1 = ''
    Object.is(false, Array.prototype.isPrototypeOf(str1))
    
    //Note: Type is Array Use Method Can Return True


</script>
</body>
</html>
```

# 2、Arr.length

- 空数组的数组长度为0

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>判断一个数组是一个空数组</title>
</head>
<body>
<script>
    //1.空数组的长度为0
    let emtryArr = []
    Object.is(0,emtryArr.length)


</script>
</body>
</html>
```

# 3、Can Do It

- 综合两个逻辑，就可以校验当前数组是不是空数组。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>判断一个数组是一个空数组</title>
</head>
<body>
<script>
    //1.空数组的长度为0
    let emtryArr = []
    let noEmtryArr = [1,2,3]

    Object.is(true,isNullArr(emtryArr)).lo
    Object.is(false,isNullArr(noEmtryArr))
    
    
    function isNullArr(arr){
      return   Array.prototype.isPrototypeOf(arr) && Object.is(0,arr.length)
    }

</script>
</body>
</html>
```

