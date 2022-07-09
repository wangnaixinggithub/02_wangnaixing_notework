# Use-Math-floor

- 向下后，最接近的整数。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Math_floor取JS向下最大整数</title>
</head>
<body>
<script>
    let num1 = 0.1
    let num2 = 1.1
    let num3 = 1.6
    let num4 = -1.1
    let num5 = -1.6
    let num6 = 1

    console.log(Math.floor(num1)) //0
    console.log(Math.floor(num2)) //1
    console.log(Math.floor(num3)) //1
    
    //注意了，这是-2
    console.log(Math.floor(num4)) //-2
    console.log(Math.floor(num5)) //-2
    
    console.log(Math.floor(num6)) //1

</script>
</body>
</html>
```

