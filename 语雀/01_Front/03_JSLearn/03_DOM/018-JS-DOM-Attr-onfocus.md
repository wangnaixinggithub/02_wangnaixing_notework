# JS-DOM-Attr-onfocus

- onfocus属性，在输入框获得焦点的时候触发回调。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-DOM-Attr-onfocus</title>
</head>
<style>

</style>
<body>
<input type="text">
<script>
    let inputElement = document.querySelector('input');
    inputElement.onfocus = () => {
        console.log('输入框获取焦点')
    }
</script>
</body>
</html>
```

