# JS-Array-#some()

`#some()` 可以解决循环退出问题，因为`#forEach()` 对数组进行遍历这个过程无法终结，所以为了提高性能，JS提供了`#some()`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试Array#some</title>
</head>
<body>
<script>
    
    let arr = ['王乃醒','黄洁莹','张三','李四']

    let printCount = 0
    arr.some((item,index) =>{
        printCount++
        if (Object.is('王乃醒',item)){
            console.log('index',index)
            return true
        }

    })

    console.log(printCount) //结果：1 遍历因为条件而终结了


    printCount = 0
    arr.forEach((item,index)=>{
        printCount++
        if (Object.is('王乃醒',item)){
            console.log('index',index)
            return true
        }

    })

    console.log(printCount) //结果：4 遍历没有因为条件而终结，而是继续往下遍历。

</script>
</body>
</html>
```

