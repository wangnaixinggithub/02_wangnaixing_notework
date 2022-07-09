# JS-string-replace

- 字符串替换函数，可以替换掉指定字符。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-string-replace</title>
</head>
<body>
<script>
    

    let str = '1234567890'

    let replace = str.replace('1','0');
    
    console.log(Object.is(replace,'0234567890'))
</script>
</body>
</html>
```

