# JS-Array-#sort()

对字母进行排序

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
    
    <ul>
        <li v-for="item in number">{{item}}</li>
    </ul>

    <button @click="btn1">sort</button><br>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            letters:['e','d','c','b','a','f'],
            number:[10,9,8,7,6,5,4,3,2,1]
        },
        methods: {
            btn1(){
            //排序 从小字母到大字母
            this.letters.sort()
            //不能对数字进行排序
            this.number.sort()

            },

        }
    })
</script>
</body>
</html>
```

