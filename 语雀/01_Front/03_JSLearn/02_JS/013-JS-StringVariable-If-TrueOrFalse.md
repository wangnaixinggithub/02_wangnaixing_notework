# JS-StringVariable-If-TrueOrFalse

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-StrVariable-If-TrueOrFalse</title>
</head>
<body>
<script>
    let str1 = ''
    let str2 = null
    let str3 = '王乃醒'

    if (str1){
        console.log('真')
    }else {
        console.log('假')  //1.Result is 假
    }


    if (str2){
        console.log('真')
    }else {
        console.log('假')  //2.Result is 假
    }


    if (str3){
        console.log('真')
    }else {
        console.log('假')  //3.Result is 真
    }
    
    //总结：string变量，只有不为空就是真。
</script>
</body>
</html>
```

