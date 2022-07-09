# JS-DOM-#setAttribute()

> 1、JS的元素属性，有些是内置的，本身就存在。比如id title这些。有一些是我们添加的比如class index 
>
> 2、内置属性用.就可以操作。
>
> 3、如果不是内置属性，就必须运用的setAttribute()方法。

# 1、Update No is No Built-in properties

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_DOM_#setAttribute()</title>
</head>
<body>
<div id="div" title="hahaha" index="1" class="divClass" >
    <span>hello</span>this is the world
</div>
</body>
<script>
    let divElement = document.querySelector('div');

    //内置属性可以直接通过.修改
    divElement.id = "div2";
    divElement.title ="hahaha";
    divElement.index = "222";


    //非内置属性必须使用setAttribute方法进行修改。
    divElement.setAttribute('class','divClass2');
    divElement.setAttribute('index','2');
</script>
</html>
```

# 2、Can Also SET New Attribute

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_DOM_#setAttribute()</title>
</head>
<body>
<div id="div" title="hahaha" >
    <span>hello</span>this is the world
</div>
</body>
<script>
    let divElement = document.querySelector('div');
    //如果原来元素没有，就当成新增了。
    divElement.setAttribute('index','1');
    divElement.setAttribute('class','divClass')
</script>
</html>
```

