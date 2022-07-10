# Vue-@keyup

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
    <input @keyup="handleCustomerKeyboardInput" >
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
            handleCustomerKeyboardInput(){
                console.log('处理用户在数据框，按下键盘的事件,一些按钮会失效比如Alt')
            }
        }
    })
</script>
</body>
</html>
```

