# Vue-@input

当用户在输入框输入数据的时候，触发`#handleCustomerWriterDataInput()`

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
    <input @input="handleCustomerWriterDataInput" >
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
            handleCustomerWriterDataInput(){
                console.log('处理用户写入数据进入Input框！')
            }
        }
    })
</script>
</body>
</html>
```

