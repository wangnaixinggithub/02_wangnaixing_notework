# JS-DOM-eventObj#preventDefault()

> 默认情况下，超链接和表单提交按钮都会进行请求的。我们可以使用手段，阻止事件提交。

# 1、#preventDefault()

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

    </style>
</head>

<body>
<a href="http://www.baidu.com">百度</a>
<form action="http://www.baidu.com">
    <input type="submit" value="提交" name="sub">
</form>
<script>
    let aElement = document.querySelector('a')
    let inputElement = document.querySelector('input')

    aElement.addEventListener('click',eventObj=>{
        //阻止事件提交
        eventObj.preventDefault()
    })
    
    inputElement.addEventListener('click',eventObj=>{
         //阻止事件提交
        eventObj.preventDefault()
    })


</script>
</body>

</html>
```

# 2、return false

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

    </style>
</head>

<body>
<a href="http://www.baidu.com">百度</a>
<form action="http://www.baidu.com">
    <input type="submit" value="提交" name="sub">
</form>
<script>
    let aElement = document.querySelector('a')
    let inputElement = document.querySelector('input')

    aElement.onclick = () => {
        return false
    }
    inputElement.onclick = () => {
        return false
    }


</script>
</body>

</html>
```

