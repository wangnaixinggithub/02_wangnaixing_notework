# JS-DOM-#getElementsByTagName()

- 根据元素标签获取到所有的元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_#getElementsByTagName()</title>
</head>
<body>
<h3>我的爱好</h3>
<ul>
    <li>代码</li>
    <li>睡觉</li>
    <li>吃饭</li>
</ul>
<h3>我的书籍</h3>
<ol>
    <li>Java</li>
    <li>JavaScript</li>
    <li>MySQL</li>
</ol>
</body>
<script>
    //1. 默认 获取到所有的 li
    let elements = document.getElementsByTagName('li')
    
    //2.得到的是一个伪数组 HtmlCollection 只能获取到长度，没有forEach()遍历
    console.log(elements.length)
    
</script>
</html>
```

- 获取到指定的li,元素其实也是一个子DOM,可以继续使用API#getElementsByTagName().

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_#getElementsByTagName()</title>
</head>
<body>
<h3>我的爱好</h3>
<ul id="id1">
    <li>代码</li>
    <li>睡觉</li>
    <li>吃饭</li>
</ul>
<h3>我的书籍</h3>
<ol id="id2">
    <li>Java</li>
    <li>JavaScript</li>
    <li>MySQL</li>
</ol>
</body>
<script>
    let elementById = document.getElementById('id1');
    let elements = elementById.getElementsByTagName('li')
    console.log(elements)
</script>
</html>
```

![image-20220616150754786](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220616150754786.png)