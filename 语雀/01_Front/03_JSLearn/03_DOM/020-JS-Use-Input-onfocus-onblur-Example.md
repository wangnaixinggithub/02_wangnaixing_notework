# Use-Input-onfocus-onblur-Example

- 这是使用输入框，焦点事件和离焦事件的一个例子。
- 主要体现在，当用户有焦点落到输入框的时候，就把value值清空.
- 当用户离开输入框的时候，如果用户没有输入内容，就显示会手机。
- 当用户输入了内容时，就给他输入款内容的颜色为淡灰色。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<style>

</style>
<body>
<label>
    <input type="text" value="手机"/>
</label>
</body>
<script>
    let inputElement = document.querySelector('input');

    inputElement.onfocus = () => {
        if (Object.is('手机',inputElement.value)){
            inputElement.value = null
        }
        inputElement.style.color = '#333';
    }

    inputElement.onblur = ()=>{
        if (!inputElement.value || Object.is(0,inputElement.value.length)){
            inputElement.value = '手机'
        }
        inputElement.style.color = '#999'
    }
</script>
</body>
</html>
```

