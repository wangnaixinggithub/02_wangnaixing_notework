# JS-DOM-Update-Attr

# 1、Can Update Href Id Title By AElement

- 默认a标签的链接地址为11,id为22 title为33，执行JS代码后变为111，222，333.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_update_Attr</title>
</head>
<body>
<a href="11" id="22"  title="33">我是一个A链接</a> <br/>
</body>
<script>

let aElement = document.querySelector('a')
aElement.href = "111"
aElement.id = "222"
aElement.title = "333"

</script>
</html>
```

# 2、Can Update Src Title By AElement

- 默认img的图片路径为111，当图片无法正常显示时，显示文字为222，当执行JS代码后，变为src变为111，title变为222.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_update_Attr</title>
</head>
<body>
<img src="11" title="22"/>
</body>
    
<script>

let imgElement = document.querySelector('img')
imgElement.src = "111"
imgElement.title = "222"

</script>
</html>
```

# 3、Can Update Input Type Value Disable

- 默认元素是一个按钮，元素值为111并且禁用，执行JS代码后，类型更改为文本输入框，值为222 且禁用属性关闭。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_update_Attr</title>
</head>
<body>
<form>
    <input type="button" value="111"  disabled />
</form>
</body>
<script>
    let inputElement = document.querySelector('input')
    inputElement.type = 'text'
    inputElement.value="222"
    inputElement.disabled = false
</script>
</html>
```

# 4、Can Update Option Select Value

- 默认下拉框选中是香蕉，执行JS代码后，选择的是西红柿。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_update_Attr</title>
</head>
<body>
<form>
   <select>
       <option>苹果</option>
       <option selected>香蕉</option>
       <option>西红柿</option>
   </select>
</form>
</body>
<script>
    let options = document.querySelectorAll('option')
    options[2].selected =  true
</script>
</html>
```

