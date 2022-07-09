# JS-string-split

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-string-split</title>
</head>
<body>
<script>
    //1.将一个字符串，拆分成字符串数组，可以通过[下标]的方法，访问元素。
    let str = "1.03"
    let strArr_01 = str.split('.')[0];
    let strArr_02 = str.split(".")[1];
    
    //1.第一个元素
    console.log(Object.is('1',strArr_01))
    //2.第二个元素
    console.log(Object.is('03',strArr_02))
    //3.字符串数组长度
    console.log(Object.is(2,str.split(".").length))
</script>
</body>
</html>
```

