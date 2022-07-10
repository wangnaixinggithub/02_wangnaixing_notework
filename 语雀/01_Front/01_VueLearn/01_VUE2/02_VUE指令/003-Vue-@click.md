# @click

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-vue案例-计数器</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h2>当前计数：{{count}}</h2>
    <!--
      v-on:click 点击事件：绑定方法！！！
      @click
    -->
    <button v-on:click="sub()">-</button>
    <button @click="add()">+</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            //1.定义数据模型count
            count:0
        },
        //2.在方法们对象中，定义计数增加 计数减少。
        methods:{
            add(){
                this.count ++;
            },
            sub(){
                this.count --;
            }
        }

    })
</script>
</body>
</html>
```

