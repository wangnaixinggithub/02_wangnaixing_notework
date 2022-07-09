# Use-BackgroudPostion-Change-Icon

- 每一个`li`,只是`background-position`在Y轴中的坐标在变化。所以我们可以遍历，修改指定像素的`backgroud-position`

# 1、Use Before Very Long

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<style>
 .box1{
     width: 180px;
 }

 ul{
     list-style-type: none;
 }
li{
    width: 23px;
    height: 23px;
    text-align: center;
    margin: 5px;
    float: left;
    background: url('images/sprite.png') no-repeat;
}
.li1{
    background-position: 0 0;
}

 .li2{
     background-position: 0 -44px;
 }
 .li3{
     background-position: 0 -88px;
 }
 .li4{
     background-position: 0 -132px;
 }
 .li5{
     background-position: 0 -176px;
 }
 .li6{
     background-position: 0 -220px;
 }
 .li7{
     background-position: 0 -264px;
 }
 .li8{
     background-position: 0 -308px;
 }
 .li9{
     background-position: 0 -352px;
 }
 .li9{
     background-position: 0 -396px;
 }
 .li9{
     background-position: 0 -440px;
 }
 .li10{
     background-position: 0 -484px;
 }

 .li11{
     background-position: 0 -524px;
 }

 .li12{
     background-position: 0 -568px;
 }

 .li13{
     background-position: 0 -612px;
 }
 .li14{
     background-position: 0 -656px;
 }
 .li15{
     background-position: 0 -700px;
 }

</style>
<body>
<ul>
    <div class="box1">
        <li class="li1"></li>
        <li class="li2"></li>
        <li class="li3"></li>
        <li class="li4"></li>
        <li class="li5"></li>

        <li class="li6"></li>
        <li class="li7"></li>
        <li class="li8"></li>
        <li class="li9"></li>
        <li class="li10"></li>


        <li class="li11"></li>
        <li class="li12"></li>
        <li class="li13"></li>
        <li class="li14"></li>
        <li class="li15"></li>
    </div>

</ul>
</body>
<script>

</script>
</body>
</html>
```

# 2、Use After 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<style>
 .box1{
     width: 180px;
 }

 ul{
     list-style-type: none;
 }
li{
    width: 23px;
    height: 23px;
    text-align: center;
    margin: 5px;
    float: left;
    background: url('images/sprite.png') no-repeat;
}


</style>
<body>
<ul>
    <div class="box1">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>

        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>


        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </div>

</ul>
</body>
<script>
    let lis = document.getElementsByTagName('li')
    for (let index in lis) {
        const y = index * 44
        lis[index].style.backgroundPosition = `0-${y}px`;
    }
</script>
</body>
</html>
```

# 3、Validation Test

	- 使用的背景图片，每一个在Y轴上间隔44px.

![image-20220619125100370](image-20220619125100370.png)

- 达成效果

![image-20220619125150468](image-20220619125150468.png)
