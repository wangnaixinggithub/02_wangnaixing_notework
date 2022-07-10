# v-bind

# 1、绑定src 绑定href

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-v-bind的基本使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--1.绑定src-->
    <img v-bind:src="imgURL" alt="这是一张图片">
    <!--2.绑定href-->
    <a v-bind:href="aHref">百度一下</a>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            imgURL:"https://cn.bing.com/th?id=OIP.NaSKiHPRcquisK2EehUI3gHaE8&pid=Api&rs=1",
            aHref:"https://www.baidu.com"
        }
    })
</script>
</body>
</html>
```

# 2、绑定class

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-v-bind动态绑定class(对象语法)</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<style>
    /*1.定义 .active 样式 */
    .active{
        color:red;
    }
</style>

<body>
<div id="app">
    <!--
       3.对象写法
        :class="{key1:value1,key2:value2}
        key1: 类名
        value1: 是否显示 true 显示  false 不显示
        用,逗号分割，可以添加多个！！！
        
        结论：
        h2标签：拥有肯定有一个css类选择器：.title .class1，
        通过按钮能动态控制css类选择器 .active
        class2类选择器不显示
     -->
    <h2 class="title" v-bind:class="{'active':isActive,'class1':true,'class2':false}">6666</h2>
    <button @click="change">点击变色</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            //2.定义 .active 的数据模型是否显示数据模型
            isActive:true
        },
        methods:{
            change(){
                this.isActive = !this.isActive
            }
       }
    })
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-v-bind动态绑定class(数组语法)</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<style>
    /*1.定义 .active 样式 */
    .active{
        color:red;
    }
</style>

<body>
<div id="app">
    <!--
    对象数组的写法
    -->
    <h2 class="title" v-bind:class="[{'active':isActive},{'class1':true},{'class2':false}]">6666</h2>
    <button @click="change">点击变色</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            isActive:true
        },
        methods:{
            change(){
                this.isActive = !this.isActive
            }
        }
    })
</script>
</body>
</html>
```

# 3、绑定style

```javascript
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>05-v-bind动态绑定style(对象语法)</title>
        <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    </head>
    <body>
    <div id="app">
        <!--2.
          内部样式对象写法 {样式名：'样式值'}
        -->
        <h2 :style="{fontSize:'50px',backgroundColor:'red'}">666</h2>
    </div>
    <script>
        const app = new Vue({
            el:"#app",
        })
    </script>
    </body>
    </html>
```

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-v-bind动态绑定style(数组语法).html</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--2.
      内部样式对象写法 [{样式名：'样式值'}]
    -->
    <h2 :style="[{fontSize:'50px'},{backgroundColor:'red'}]">666</h2>
</div>
<script>
    const app = new Vue({
        el:"#app",
    })
</script>
</body>
</html>
```



# 4、语法糖

```properties
v-bind:src= :src
v-bind:href= :href
v-bind:class= :class
v-bind:style= :style
```

