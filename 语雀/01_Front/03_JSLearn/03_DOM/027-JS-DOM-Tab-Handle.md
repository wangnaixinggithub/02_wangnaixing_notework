# JS-DOM-Tab-Handle

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        li {
            list-style-type: none;
        }

        .tab {
            width: 978px;
            margin: 100px auto;
        }

        .tab_list {
            height: 39px;
            border: 1px solid #ccc;
            background-color: #f1f1f1;
        }

        .tab_list li {
            float: left;
            height: 39px;
            line-height: 39px;
            padding: 0 20px;
            text-align: center;
            cursor: pointer;
        }

        .tab_list .current {
            background-color: #c81623;
            color: #fff;
        }

        .item_info {
            padding: 20px 0 0 20px;
        }

        .item {
            display: none;
        }
    </style>
</head>
<body>
<div class="tab">
    <div class="tab_list">
        <ul>
            <li class="current">商品介绍</li>
            <li>规格与包装</li>
            <li>售后保障</li>
            <li>商品评价（50000）</li>
            <li>手机社区</li>
        </ul>
    </div>
    <div class="tab_con">
        <div class="item" style="display: block;">
            商品介绍模块内容
        </div>
        <div class="item">
            规格与包装模块内容
        </div>
        <div class="item">
            售后保障模块内容
        </div>
        <div class="item">
            商品评价（50000）模块内容
        </div>
        <div class="item">
            手机社区模块内容
        </div>

    </div>
</div>
<script>
 let liElements = document.querySelector('.tab_list').querySelectorAll('li');
 let divElements = document.querySelectorAll('.item');

 for (let i = 0; i < liElements.length; i++) {
        liElements[i].setAttribute('index',i)

        liElements[i].onclick = () =>{
            let index =  liElements[i].getAttribute('index')

            console.log(index)

            //1.选项框处理
            for (let j = 0; j < liElements.length; j++) {
                liElements[j].className = ''
            }
           liElements[i].className = 'current'
            
            //2.内容展示处理
            for (let j = 0; j < divElements.length; j++) {
                divElements[j].style.display = 'none'
            }
            divElements[index].style.display = 'block'
       
        }

 }
</script>
</body>

</html>
```

