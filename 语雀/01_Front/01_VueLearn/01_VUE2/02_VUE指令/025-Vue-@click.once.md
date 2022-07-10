# Vue-@click.once

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
    <!--加入.once属性，会让事件只会触发一次-->
    <button @click.once="handleClick">按钮</button>

</div>
<script>
    const app = new Vue({
        el:"#app",
        methods: {
            handleClick(){
                console.log("哈哈哈")
            },

        }
    })
</script>
</body>
</html>
```

