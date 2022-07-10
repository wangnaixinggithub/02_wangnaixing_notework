# JS-Array-#shift()

删除第一个元素 	

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
                //shift()删除第一个元素
                this.letters.shift();

            },

        }
    })
</script>
</body>
</html>
```

