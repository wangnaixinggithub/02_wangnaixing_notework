# JS-DOM-InputElement-#focus()

- 监听键盘按下事件，如果用户按下了`s`，让输入框获取到焦点光标。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
</head>

<body>
<input type="text">

<script>
let inputElement = document.querySelector('input')
document.addEventListener('keydown',(e) =>{
    if (Object.is(e.key,'s')){
        //指定输入框，获取焦点。
        inputElement.focus()
    }
})
</script>
</body>

</html>
```

