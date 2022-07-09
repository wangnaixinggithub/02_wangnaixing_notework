# Use-JS-toFixed()

- toFixed()函数，可以对number类型的数据，进行精度保留。最后得到的返回值类型为string

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS_toFixed函数的使用</title>
</head>
<body>
<script>
    let BL_ZAO =  0.09389671361502347
    let toFixedNumber1 = BL_ZAO.toFixed(1)
    let toFixedNumber2 = BL_ZAO.toFixed(2)
    let toFixedNumber3 = BL_ZAO.toFixed(3)
    let toFixedNumber4 = BL_ZAO.toFixed(4)
    let toFixedNumber5 = BL_ZAO.toFixed(5)


    console.log('toFixedNumber1 =>',toFixedNumber1) // toFixedNumber1 => 0.1
    console.log('toFixedNumber2 =>',toFixedNumber2) // toFixedNumber2 => 0.09
    console.log('toFixedNumber3 =>',toFixedNumber3) // toFixedNumber3 => 0.094
    console.log('toFixedNumber4 =>',toFixedNumber4) // toFixedNumber4 => 0.0939
    console.log('toFixedNumber5 =>',toFixedNumber5) // toFixedNumber5 => 0.09390



    // toFixed() ReturnValueType is string
    console.log(typeof (toFixedNumber1))
    console.log(typeof (toFixedNumber2))
    console.log(typeof (toFixedNumber3))
    console.log(typeof (toFixedNumber4))
    console.log(typeof (toFixedNumber5))
</script>
</body>
</html>
```

- 可以做小数类型百分比展示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>使用toFixed()方法将小数转为百分比</title>
</head>
<body>

<script>
    console.log("输入比例值为'',则函数输出为=>",getBl(''))      //  输入比例值为'',则函数输出为=> 0.00%
    console.log('输入比例值为undefined,则函数输出为',getBl(undefined)) //输入比例值为undefined,则函数输出为 0.00%
    console.log("输入比例值为null,则函数输出为=>",getBl(null))   //输入比例值为null,则函数输出为=> 0.00%
    console.log("输入正常小数，则函数输出为=>",getBl(0.6845))    //输入正常小数，则函数输出为=> 68.45%
    console.log("输入数字字符串，则函数输出为 =>",getBl('0.6845')) //输入数字字符串，则函数输出为 => 68.45%


    //1.计算比例的方法
   function getBl(bl){
        //handle :数字字符串
        if (Object.is(typeof(bl),'string')){
            return '0%'
        }
        

        let _percent = ((bl || 0) * 100).toFixed(2)
        _percent = Object.is(Number(_percent),'NaN') ? 0 : _percent + '%'
        return _percent
    }

</script>
</body>
</html>
```

