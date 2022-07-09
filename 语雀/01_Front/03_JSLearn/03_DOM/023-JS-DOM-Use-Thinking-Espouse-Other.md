# JS-DOM-Use-Thinking-Espouse-Other

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
</head>
<style>


</style>
<body>
    <button>按钮1</button>
    <button>按钮2</button>
    <button>按钮3</button>
    <button>按钮4</button>
    <button>按钮5</button>

<script>
    //排他思想
    let buttonElements = document.getElementsByTagName('button');
    
	for (let i = 0; i < buttonElements.length; i++) {
        buttonElements[i].onclick = () => {
            
            //1.先清空全部的背景颜色
            for (let j = 0; j < buttonElements.length; j++) {
                buttonElements[j].style.backgroundColor = ''
            }

            //2.再设置进去需要设置的背景颜色
            buttonElements[i].style.backgroundColor = 'pink'
        }

    }
</script>

</body>
</html>
```

![image-20220619135804075](image-20220619135804075.png)
