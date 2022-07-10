# JS-Array-#push()

在数组末尾添加一个元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array#push()</title>
</head>
<body>
<div id="app">

    <ul>
        <li v-for="item in letters">{{item}}</li>
    </ul>

    <button @click="btn1">push</button><br>

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
                //push 在末尾新增f
                this.letters.push('f')

            },
        }
    })
</script>
</body>
</html>
```

