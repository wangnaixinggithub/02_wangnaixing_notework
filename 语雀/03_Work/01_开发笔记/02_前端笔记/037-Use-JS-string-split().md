# Use-JS-string-split()

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08_字符串对象的split()函数</title>
</head>
<body>

<script>
    let locationHref = 'http://127.0.0.1:8765/yxydgl.html#/jbxx?recordId=295e5b4bd0ff11eca9f0166d0925f6ce&clsID=2012c1c1854111eab6f30a580af40df7&status=A'
    let arr = locationHref.split('?');

                     //JS 世界里,数组本质也是对象~

    //此时的locationHref,被拆分成了数组.=> true
    console.log('此时的locationHref,被拆分成了数组.=>',Object.is(typeof (arr),'object'))


    //arr[0] =>  http://127.0.0.1:8765/yxydgl.html#/jbxx
    console.log(' arr[0] => ',arr[0])

    // arr[1] =>  recordId=295e5b4bd0ff11eca9f0166d0925f6ce&clsID=2012c1c1854111eab6f30a580af40df7&status=A
    console.log(' arr[1] => ',arr[1])

</script>

</body>
</html>
```

