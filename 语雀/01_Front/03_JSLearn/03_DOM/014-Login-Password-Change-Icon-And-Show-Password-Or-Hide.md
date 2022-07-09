# Login-Password-Change-Icon-And-Show-Password-Or-Hide

- 应用于登陆时，明文显示密码或者密文显示密码
- 通常提供两张图片
- 操作图片元素的src属性，input框的type属性。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    .box{
        position: relative;
        width: 400px;
        margin: 100px auto;
        border-bottom: 1px solid #ccc;
    }
    .box>img{
        position: absolute;
        width: 30px;
        height: 30px;
        left: 370px;

    }
    .box>input{

        width: 220px;
        height: 30px;
        border: 0;
        outline: none;

    }
</style>
<body>
<div class="box">
    <img src="images/close.png">
    <input  type="password" >
</div>
<script>
    let inputElement = document.querySelector('input')
    let imageElement = document.querySelector('img')
    let clickFlag = 0
    imageElement.onclick = () => {
        if (Object.is(clickFlag,0)){
           inputElement.type = 'text'
           imageElement.src = 'images/open.png'
            clickFlag = 1
        }else {
           inputElement.type = 'password'
            imageElement.src = 'images/close.png'
            clickFlag = 0
        }

    }
</script>
</body>
</html>
```

