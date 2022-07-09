# JS-string-indexof

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-string-indexof</title>
</head>
<body>
<script>

    let str = '1234567890'
    
    //1.如果存在 则返回对应的字符位置
    console.log(Object.is(str.indexOf('1'),0))
    console.log(Object.is(str.indexOf('0'),9))

    //2.不存在 返回-1
    console.log(Object.is(str.indexOf('10'),-1))

</script>
</body>
</html>
```

