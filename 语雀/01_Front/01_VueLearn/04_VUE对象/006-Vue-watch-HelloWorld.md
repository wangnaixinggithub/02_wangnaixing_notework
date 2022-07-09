# Vue-watch-HelloWorld

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <style>
        body {
            padding: 15px;
            user-select: none;
        }
    </style>
</head>
<body>
<div id="app">
    <input type="text" v-model="username"  />
    <div>{{username}}</div>

</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            username: 'wang'

        },
        watch:{
            //监听到data数据变化 ： 变化后的值 变化前的值
            username(newValue,oldValue){
                console.log('新值'+newValue)
                console.log('旧值'+oldValue)
            }

        }

    })
</script>
</body>
</html>
```

