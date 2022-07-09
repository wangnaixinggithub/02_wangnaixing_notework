# Vue-watch-Monitor-Object-Property

> 只监听对象中的某个属性

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
            //只监听对象中某个属性
            'formData.name':{
                handler(newObj,oldObj){
                    console.log('----------newObj---------')
                    console.log(newObj)

                    console.log('----------oldObj---------')
                    console.log(oldObj)
                },
                deep:true
            },
        }
    })
</script>
</body>
</html>
```

