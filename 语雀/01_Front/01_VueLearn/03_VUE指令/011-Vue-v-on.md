# Vue-v-on

# 1、v-on

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-v-on的基本使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h2>{{count}}</h2>
    <button v-on:click="increment">加</button>
    <button v-on:click="decrement()">减</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            count:0
        },
        methods: {
            increment(){
                this.count++
            },
            decrement(){
                this.count--
            }
        }

    })
</script>
</body>
</html>
```

# 2、语法糖

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-v-on的基本使用</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <h2>{{count}}</h2>
    <!--方法如果没有参数，可以不写()
        @click 等价于 v-on:click 
        用于视图绑定函数
    -->
    <button @click="increment">加</button>
    <button @click="decrement()">减</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            count:0
        },
        methods: {
            increment(){
                this.count++
            },
            decrement(){
                this.count--
            }
        }

    })
</script>
</body>
</html>
```

# 3、参数传递

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-v-on的参数传递</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
</head>
<body>
<div id="app">

    <!-- 1.事件没传参 可以不写括号-->
    <button @click="btnClick">按钮1</button>
    <button @click="btnClick()">按钮2</button>

    <!-- 2.事件调用方法需要传参，
   case1:正常传参  传参值value= 123
   case2:没有传参   value = undefined
   case3:没有括号 value 为 PointerEvent event 对象
   case4:正常传参，并且想得到 PointerEvent event 对象  value=123 event = $event
    -->
    <button @click="btnClick2(123)">按钮3</button>
    <button @click="btnClick2()">按钮4</button>
    <button @click="btnClick2">按钮5</button>
    <button @click="btnClick3(123,$event)">按钮6</button>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            count:0
        },
        methods: {
            btnClick(){
                console.log("点击XXX");
            },
            btnClick2(value){
                console.log(value+"----------");
            },
            btnClick3(value,event){
                console.log(event+"----------"+value);
            }
        }

    })
</script>
</body>
</html>
```

