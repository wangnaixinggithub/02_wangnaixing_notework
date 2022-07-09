# JS-DOM-Event-Bubbling

# 1、Use onclick

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .father {
            overflow: hidden;
            width: 300px;
            height: 300px;
            margin: 100px auto;
            background-color: pink;
            text-align: center;
        }

        .son {
            width: 200px;
            height: 200px;
            margin: 50px;
            background-color: purple;
            line-height: 200px;
            color: #fff;
        }
    </style>
</head>

<body>
<div class="father">
    <div class="son">son盒子</div>
</div>
<script>
    //1.JS代码只能执行捕获和冒泡中的一个阶段，而onclick 只能是 冒泡阶段
    let fatherElement = document.getElementsByClassName('father')
    let sonElement = document.getElementsByClassName('son')


    sonElement.onclick = ()=>{
        console.log('Son....')
    }

    fatherElement.onclick = () =>{
        console.log('Father....')
    }

    //2.只有此方法执行  冒泡阶段  document -> html -> body -> father -> son
    document.onclick = () =>{
        console.log('Document...')
    }



</script>
</body>

</html>
```

# 2、Use addEventListener

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .father {
            overflow: hidden;
            width: 300px;
            height: 300px;
            margin: 100px auto;
            background-color: pink;
            text-align: center;
        }

        .son {
            width: 200px;
            height: 200px;
            margin: 50px;
            background-color: purple;
            line-height: 200px;
            color: #fff;
        }
    </style>
</head>

<body>
<div class="father">
    <div class="son">son盒子</div>
</div>
<script>
	//效果同上
    let fatherElement = document.querySelector('.father')
    let sonElement = document.querySelector('.son')


    fatherElement.addEventListener('click',()=>{
        console.log('Father....')
    })

    sonElement.addEventListener('click',()=>{
        console.log('Son....')
    })

    document.addEventListener('click',()=>{
        console.log('Document...')
    })


</script>
</body>

</html>
```

- 但是，我们可以通过#addEventListener()，设置第三的参数的属性。来改变事件的属性为捕获阶段

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .father {
            overflow: hidden;
            width: 300px;
            height: 300px;
            margin: 100px auto;
            background-color: pink;
            text-align: center;
        }

        .son {
            width: 200px;
            height: 200px;
            margin: 50px;
            background-color: purple;
            line-height: 200px;
            color: #fff;
        }
    </style>
</head>

<body>
<div class="father">
    <div class="son">son盒子</div>
</div>
<script>
    //1.JS代码只能执行捕获和冒泡中的一个阶段，而onclick 只能是 捕获阶段
    let fatherElement = document.querySelector('.father')
    let sonElement = document.querySelector('.son')


    fatherElement.addEventListener('click',()=>{
        console.log('Father....')
    })

    sonElement.addEventListener('click',()=>{
        console.log('Son....')
    })

    document.addEventListener('click',()=>{
        console.log('Document...')
    })
    
    // 捕获阶段 son -> father ->body -> html -> document


</script>
</body>

</html>
```

