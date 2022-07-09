# Vue-$event

$event 是 VUE 提供的特殊变量，用来表示原生的事件参数对象 event。$event 可以解决事件参数对象 event  被覆盖的问题。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <input @keyup="fun1($event)" type="text" />
    <input @keydown="fun2($event)" type="text"/>
    <input @input="fun3($event)" type="text"/>
    <button @click="fun4($event)">按钮</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            message:"你好啊 张三丰",
            myName:"王乃醒",
            myDesc:"是大帅哥！！",
            count:100

        },
        methods:{
            fun1(e){
                console.log(e)
            },
            fun2(e){
                console.log(e)
            },
            fun3(e){
                console.log(e)
            },
            fun4(e){
                console.log(e)
            }
        }
    })
</script>
</body>
</html>
```



![image-20220707002506042](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220707002506042.png)