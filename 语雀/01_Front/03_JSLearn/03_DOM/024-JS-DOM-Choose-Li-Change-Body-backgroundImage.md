# JS-DOM-Choose-Li-Change-Body-backgroundImage

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
    * {
        margin: 0;
        padding: 0;
    }

    body {
        background: url("images/1.jpg") no-repeat center top;
    }

    div {
        margin: 200px auto;
        padding: 10px;
    }

    ul {
        width: 1200px;
    }

    li {
        border: 3px solid #FFFFFF;
        list-style: none;
        float: left;
        width: 100px;
    }

    img {
        width: 100%;
        height: 100%;
        overflow: hidden;
    }

</style>
<body>
<div>
    <ul>
        <li><img src="images/1.jpg"></li>
        <li><img src="images/2.jpg"></li>
        <li><img src="images/3.jpg"></li>
        <li><img src="images/4.jpg"></li>
    </ul>
</div>
<script>
    let imgElements = document.getElementsByTagName('img')

    for (let i = 0; i < imgElements.length; i++) {
        imgElements[i].onclick = () =>{
            document.body.style.backgroundImage = `url(${imgElements[i].src})`
        }
    }

</script>

</body>
</html>`
```

![image-20220619142227644](image-20220619142227644.png)