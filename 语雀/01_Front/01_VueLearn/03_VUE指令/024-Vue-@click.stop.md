# Vue-@click.stop 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-v-on的修饰符</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--
    1. .stop的使用，btn的click事件不会传播，不会冒泡到上层，底层调用event.stopPropagation()

    click事件不会传播：点击按钮1触发btnClick()，不会触发到divClick()方法执行

    -->
    <div @click="divClick">
        <button @click.stop="btnClick">按钮1</button>
    </div>


</div>
<script>
    const app = new Vue({
        el:"#app",
        methods: {
            btnClick(){
                console.log("点击button");
            },
            divClick(){
                console.log("点击div");
            },
         

        }

    })
</script>
</body>
</html>
```

