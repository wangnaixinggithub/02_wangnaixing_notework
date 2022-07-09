# Use-JS-Array-Object-API-DomIsChange



# 1、push()

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

# 2、pop()

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
                //pop()删除最后一个元素
                this.letters.pop();

            },

        }
    })
</script>
</body>
</html>
```

# 3、shift()

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

# 4、unshift()

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
                //4.unshift()添加在最前面,可以添加多个
                this.letters.unshift('1','2','3');

            },

        }
    })
</script>
</body>
</html>
```

# 5、splice()

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
                //5.splice():删除元素/新增元素/修改元素

                //=================1、删除========================
                //1.1、 删除单个元素
                // splice(1,1)在索引为1的地方删除一个元素,
                //this.letters.splice(1,1);

                //1.2 删除指定索引后面全部元素
                // 第二个元素不传，直接删除索引为1后面d的所有元素
                //this.letters.splice(1);

                //=================2、新增========================
                // 在指定索引之后，新增元素。
                // splice(index,0,'aaa')在索引index后面删除0个元素，加上'aaa',
                //this.letters.splice(1,0,'aaa');


                //=================2、修改========================
                // 修改指定索引的元素
                // splice(1,1,'aaa')替换索引为1的后一个元素为'aaa'
                this.letters.splice(1,1,'aaa');




            },

        }
    })
</script>
</body>
</html>
```

# 5、sort()

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

# 6、reverse()

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
            //反转，最后一个元素变成第一个元素
            this.letters.reverse()
            this.number.reverse()
            },

        }
    })
</script>
</body>
</html>
```



我们改变DOM绑定的数据时，DOM会动态的改变值。数组也是一样的。但是对于动态变化数据，有要求，不是任何情况改变数据都会变化。`btn2按钮`是通过索引值修改数组的值，这种情况，数组letters变化.

`DOM不会变化`。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>数组的响应式方法 </title>
</head>
<body>
<div id="app">
    <!-- 不会改变 -->
    <ul>
        <li v-for="item in letters">{{item}}</li>
    </ul>
    <button @click="btn2">通过索引值修改数组</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            letters:['a','b','c','d','e']
        },
        methods: {
            btn2(){
                this.letters[0]='f'
            }
        }
    })
</script>
</body>
</html>
```

而数组的方法，例如`push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`等方法修改数组的数据，DOM元素会随之修改。





