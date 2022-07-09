# Js-URLSearchParams-Do-GetQueryString



- 通过`URLSearchParams`对象，我们可以把查询字符串转为JS对象。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Js-URLSearchParams-Do-GetQueryString</title>
</head>
<body>
<script>
    let urlSearchParams = new URLSearchParams('?username=wangnaixing&sex=男&age=26');
    let queryObjectParms = Object.fromEntries(urlSearchParams);

    console.log(queryObjectParms);

</script>
</body>
</html>
```

![image-20220606214949556](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220606214949556.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
   <title>Js-URLSearchParams-Do-GetQueryString</title>
</head>
<body>
<script>
    let urlSearchParams = new URLSearchParams(location.search);
    let queryObjectParms = Object.fromEntries(urlSearchParams);

    console.log(queryObjectParms);

</script>
</body>
</html>
```

