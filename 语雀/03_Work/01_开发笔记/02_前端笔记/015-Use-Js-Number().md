# Use-Js-Number()

# 1、Number()

- Number()可以把布尔类型，日期类型，字符串类型转为数字类型。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_number函数的使用</title>
</head>
<body>
<script>
    let boolNumber_true = Boolean(true)
    let boolNumber_false = Boolean(false)
    let dateNumber = new Date()
    let strNumber = "999"

    document.write("true布尔类型使用=>" + Number(boolNumber_true) + "<br/>")
    document.write("false布尔类型使用=>" + Number(boolNumber_false) + "<br/>")
    document.write("日期类型使用 =>" + Number(dateNumber) + "<br/>")
    document.write("字符串类型使用 =>" + Number(strNumber) + "<br/>")

    //true 布尔类型使用 =>1
   
    //false 布尔类型使用 => 0
    
    // 日期类型使用 => 1651830875747
    
    //字符串类型使用 => 999

    let notParseNumber = "王乃醒"

    document.write("不能解析 =>" + Number(notParseNumber) + "<br/>")
    
    //不能解析 =>NaN
</script>
</body>
</html>
```

# 2、Number() do,Enhance!

解析失败的时候，解析结果为`NaN`，我们可以定义函数进行又一步处理。加入解析得到`NaN`，则取0。

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_number函数的使用</title>
</head>
<body>
<script>
    function number(value) {
        return Number(value) || 0
    }
    let boolNumber_true = Boolean(true)
    let boolNumber_false = Boolean(false)
    let dateNumber = new Date()
    let strNumber = "999"

    document.write("true布尔类型使用=>" + number(boolNumber_true) + "<br/>")
    document.write("false布尔类型使用=>" + number(boolNumber_false) + "<br/>")
    document.write("日期类型使用 =>" + number(dateNumber) + "<br/>")
    document.write("字符串类型使用 =>" + number(strNumber) + "<br/>")

    //true 布尔类型使用=> 1
    
    //false 布尔类型使用=> 0
    
    // 日期类型使用 => 1651830875747
    
    //字符串类型使用 => 999

    let notParseNumber = "王乃醒"

    document.write("不能解析 =>" + number(notParseNumber) + "<br/>")
    // 不能解析 =>0
    
</script>
</body>
</html>
```

