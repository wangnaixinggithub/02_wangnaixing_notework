# Create-Date-Object-Use-String

- 通过字符串，创建日期对象。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>日期字符串转为日期对象</title>
</head>
<body>
<script>
    let dateStr1 = '2022-06-06 14:07:31'
    let dateStr2  = '2022-06-06 00:00:00'

   let dateObj = new Date(dateStr1);
   let dateObj2 = new Date(dateStr2);

   console.log(dateObj) //Mon Jun 06 2022 14:07:31 GMT+0800 (中国标准时间)

   console.log(dateObj2) //Mon Jun 06 2022 00:00:00 GMT+0800 (中国标准时间)

    
</script>

</body>
</html>
```

