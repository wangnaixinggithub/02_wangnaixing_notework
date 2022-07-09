# JS-DOM-Use-contextmenu-No-Access-Copy

- 通过监听`contextmenu`事件，然后禁用显示上下文菜单的默认行为，可以达到文字让用户不能复制的目的。

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
<h1>这一段文字不能被用户复制！！！</h1>

<script>
    let h1Element = document.querySelector('h1')
    h1Element.addEventListener('contextmenu',(e)=>{
        //禁用显示上下文菜单
        e.preventDefault()
    })
</script>
</body>

</html>
```

