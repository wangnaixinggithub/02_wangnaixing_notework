# Date-Obj-API-Get-Info

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>日期对象的详细信息</title>
</head>
<body>
<script>
    let dateStr1 = '2022-06-06 14:07:31'

    let dateObj = new Date(dateStr1);

    let year = dateObj.getFullYear()

    let month = dateObj.getMonth() //从0开始。

    let day = dateObj.getDay() //周几 从0开始

    let hour = dateObj.getHours()

    let millisecond = dateObj.getTime()

    let minutes = dateObj.getMinutes()

    

    Object.is(2022,year)

    Object.is(5,month)

    Object.is(1,day)

    Object.is(14,hour)

    Object.is(7,minutes)

    Object.is(1654495651000,millisecond)

</script>

</body>
</html>
```

