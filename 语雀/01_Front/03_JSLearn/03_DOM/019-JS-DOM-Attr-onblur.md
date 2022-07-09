# JS-DOM-Attr-onblur

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-DOM-Attr-onblur</title>
</head>
<style>

</style>
<body>
<input type="text">
<script>
    let inputElement = document.querySelector('input');
    inputElement.onblur = ()=>{
        console.log('输入框失去焦点时触发....')
    }
</script>
</body>
</html>
```

