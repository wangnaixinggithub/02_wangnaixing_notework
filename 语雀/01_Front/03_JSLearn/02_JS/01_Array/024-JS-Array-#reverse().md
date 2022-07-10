# JS-Array-#reverse()

反转数组。最后一个元素反转后变成第一个元素，倒数第二个元素变成第二个元素，依次类推。

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

    <button @click="btn1">reverse</button><br>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            letters:['e','d','c','b','a'],
            number:[10,9,8,7,6,5,4,3,2,1]
        },
        methods: {

            btn1(){
                //整体数组反转最后一个元素变成第一个元素 
                this.letters.reverse()
                this.number.reverse()
            },

        }
    })
</script>
</body>
</html>
```

