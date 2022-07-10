# Vue-watch-Monitor-Object-Use-deep

> 如果没有显示配置，默认监听不可以监听对象的属性变化的。使用deep就可以处理这个问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body>
<div id="app">
   学生姓名： <input v-model="formData.name"/>
   学生学号:  <input v-model="formData.stuNo"/>
   学生年龄： <input v-model="formData.age"/>
</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
          formData:{
              name:'',
              stuNo:'',
              age:''
          }

        },
        watch:{
            formData:{
                handler(newObj,oldObj){
                    console.log('----------newObj---------')
                    console.log(newObj.name)
                    console.log(newObj.stuNo)
                    console.log(newObj.age)

                    console.log('----------oldObj---------')
                    console.log(oldObj.name)
                    console.log(oldObj.stuNo)
                    console.log(oldObj.age)
                },
                //开启deep:true 才可以监听对象数据变化
                deep:true
            },
        }

    })
</script>
</body>
</html>
```

