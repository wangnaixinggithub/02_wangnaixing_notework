# JS-click-Div-display-None

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<style>
.box1{
    position: relative;
    margin: 100px auto;
    width: 90px;
    height: 90px;
    border: 1px solid #ccc;
    color: red;
    font-size: 12px;
    text-align: center;
}
img{
    margin-top: 5px;
    width: 60px;
    height: 60px;
}
i {
    position: absolute;
    left: -16px;
    top: -1px;
    border: 1px solid #cccccc;
    font-family: Arial, Helvetica, sans-serif;
    color: red;
    width: 14px;
    height: 14px;
    cursor: pointer;
}
</style>
<body>
<div class="box1">
淘宝二维码
<img src="images/tao.png">
<i class="close-btn">×</i>
</div>
<script>
let divElement = document.querySelector('div');
let iElement = document.querySelector('i');
iElement.onclick = () => {
    divElement.style.display = 'none';
}
</script>
</body>
</html>
```

