# JS-DOM-Attr-innerText

- 获取文档对象内部的Text内容。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_#attr_innerText</title>
</head>
<body>
<div id="app"><h1>我今天不想上班！</h1></div>
<script>
    let element = document.getElementById('app')
    let innerText = element.innerText; //我今天不想上班！
    console.log(innerText)
</script>
</body>
</html>
```

