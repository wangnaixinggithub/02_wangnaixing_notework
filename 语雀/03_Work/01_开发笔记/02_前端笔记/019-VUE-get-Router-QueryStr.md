# VUE-get-Router-QueryStr

## 1、$router.replace()

- 在AddJbxx组件中，通过 `this.$router.replace()` 方法，输入`jbxx`路由path，则VUE就把自动路由到jbxx组件。
- 从parameters中获取查询字符串中key=recordId的值。

```vue
<!--AddJbxx.vue-->
<template>
<div>
  AddJbxx.vue.....
</div>
</template>

<script>
export default {
  name: "AddJbxx",
  methods: {
    createRecord() {
      console.log('正在初始化...')
      //路由跳转
      this.$router.replace('/jbxx?recordId=295e5b4bd0ff11eca9f0166d0925f6ce')

    },
  },
    
  mounted () {
    this.createRecord()
  }
}
</script>

<style scoped>

</style>


<!--jbxx.vue-->
<template>
  <div>
    jbxx.vue.....
    <el-button @click="getRecordId">获取请求参数recordId</el-button>
  </div>
</template>

<script>
export default {
  name: "jbxx",
  props: ['parameters'],
  methods:{
    formId () {
      return this.parameters.recordId
    },
    getRecordId(){
       //recordId=295e5b4bd0ff11eca9f0166d0925f6ce
      console.log(this.formId())
    }
  },


}
</script>

<style scoped>

</style>
```

## 2、props: (route) => ({ parameters: route.query })

- 通过prop属性，配置`parameters: route.query`,即路由查询字符串按照=分离的数组，赋值给parameters参数。

```js
import Vue from 'vue'
import Router from 'vue-router'
import addJbxx from '../pages/yxydgl/views/jbxx/AddJbxx'
import jbxx from '../pages/yxydgl/views/jbxx/jbxx'

Vue.use(Router)

export default new Router({
   mode:"history",
    routes: [
        {
            path: '/addJbxx',
            name: 'addJbxx',
            component: addJbxx,
            props: (route) => ({ parameters: route.query })
        },
        {
            path: '/jbxx',
            name: 'jbxx',
            component: jbxx,
            props: (route) => ({ parameters: route.query })
        },

    ]
})
```

