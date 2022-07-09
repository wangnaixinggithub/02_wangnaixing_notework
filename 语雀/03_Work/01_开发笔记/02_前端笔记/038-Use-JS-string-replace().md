# Use-JS-string-replace()

- replace(oldPartten,newPartten),可以用newPartten 替换在字符串中的oldPartten。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09_字符串对象的replace()函数</title>
</head>
<body>
<script>
    let str1 = 'http://127.0.0.1:8765/yxydgl.html#/jbxx?recordId=1&clsID=2012c1c1854111eab6f30a580af40df7&status=A'
    let str2 = str1.split('?')[1]
    let str3 = str2.replace('recordId=1','recordId=295e5b4bd0ff11eca9f0166d0925f6ce');

    
    //str3=> recordId=295e5b4bd0ff11eca9f0166d0925f6ce&clsID=2012c1c1854111eab6f30a580af40df7&status=A
    console.log('str3=>',str3)  
    
</script>
</body>
</html>
```

