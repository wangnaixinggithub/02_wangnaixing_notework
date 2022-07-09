```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>reduce函数</title>
</head>
<body>
<script>
    //要求将num2的数组所有数累加
    const nums = [150,160,163,170,177,155,160,190]
    /**
     * preValue:上一个值，第一次被我指定为0
     * currentValue：当前值
     * 1、 preValue = 0
     * 2、 preValue = 0+150
     * 3、 preValue = 0+150+160
     * .。。
     */
    const newNum2 =  nums.reduce((preValue,currentValue)=> preValue + currentValue,0)
    console.log(newNum2)

</script>
</body>
</html>
```

