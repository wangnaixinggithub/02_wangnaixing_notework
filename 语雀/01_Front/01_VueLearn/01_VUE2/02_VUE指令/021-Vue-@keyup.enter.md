# Vue-@keyup.enter

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

    
    <!--3. 监听鼠标点击输入框的键盘事件，并且这个按钮是回车键Enter才会触发回调 -->
    <input type="text" @keyup.enter="keyupAndKeyBoardIsEnter">
    

</div>
<script>
    const app = new Vue({
        el:"#app",
        methods: {
            keyupAndKeyBoardIsEnter(){
                console.log("输入框被点击")
            },
        }
    })
</script>
</body>
</html>
```

