# JS-DOM-#innerHTML()	

- 文档对象的属性，能够得到操作的文档元素的内部的HTML。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_#attr</title>
</head>
<body>
<div id="app"><h1>我今天不想上班！</h1></div>
<script>
    let element = document.getElementById('app')
    let innerHTML = element.innerHTML; //<h1>我今天不想上班！</h1>
    console.dir(innerHTML)
</script>
</body>
</html>
```

