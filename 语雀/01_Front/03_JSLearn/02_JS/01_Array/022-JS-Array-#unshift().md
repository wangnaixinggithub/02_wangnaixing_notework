# JS-Array-#unshift()

在数组最前面添加若干元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-数组的响应式方法</title>
</head>
<body>
<div id="app">

    <ul>
        <li v-for="item in letters">{{item}}</li>
    </ul>
    
    <button @click="btn1">pop</button><br>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            letters:['a','b','c','d','e']
        },
        methods: {
            btn1(){
                //4.unshift()添加在最前面,可以添加多个
                this.letters.unshift('1','2','3');

            },

        }
    })
</script>
</body>
</html>
```

