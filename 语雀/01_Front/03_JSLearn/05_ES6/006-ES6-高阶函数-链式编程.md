# 高阶函数的链式编程

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>高阶函数的链式编程</title>
</head>
<body>
<script>
   const nums = [150,160,163,170,177,155,160,190]
   let newNums = nums.filter( item => item > 170)
                     .map( item => item * 2)
                     .reduce((preValue,currentValue) => preValue + currentValue,0);
    console.log(newNums);
</script>
</body>
</html>
```

