# Vue-computed

> 好处在于:数据模型的值变化，计算属性执行方法逻辑，重新计算。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-计算属性的基本使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h1 v-text="myInfo"></h1>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            myName:'王乃醒',
            myDesc:'是个大帅哥！！！'
        },
        computed: {
            myInfo(){
                return this.myName + " " + this.myDesc
            },
        },
    })
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-计算属性的复杂使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h2>总价格：{{totalPrice}}</h2>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            books:[
                {id:110,name:"JavaScript从入门到入土",price:100},
                {id:111,name:"Java从入门到放弃",price:200},
                {id:112,name:"编码艺术",price:300},
                {id:113,name:"代码大全",price:400},
            ]
        },
        computed: {
            totalPrice(){
              return this.books
                    .map(item=>item.price)
                    .reduce((prePrice,nextPrice)=> prePrice + nextPrice,0)

            },
        },
    })
</script>
</body>
</html>
```

