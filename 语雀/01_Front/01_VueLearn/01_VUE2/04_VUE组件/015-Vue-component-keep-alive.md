# Vue-component-keep-alive

# 1、No Use keep alive Bring Problems

默认情况下，切换动态组件时无法保持组件的状态。此时可以使用 vue 内置的 组件保持动态组件的状态。

```vue
<template>
<div style="border: 1px solid red;height: 300px;width: 300px;">
  <h2>子组件1</h2>
    <!--1、子组件定义了count++的事件和模型，这时候我在父组件中展示，触发事件。把count➕到10-->
  <button @click="count++">count+1</button>
  <h3>{{count}}</h3>
</div>
</template>

<script>

export default {
  name: "Left",
  data(){
    return {
      count:1
    }
  },
  methods:{

  },

}
</script>

<style scoped>

</style>
```

```vue
  <template>
    <div id="app">

      <button @click="componentName='Left'">渲染组件左组件</button>
      <button @click="componentName='Right'">渲染组件右组件</button>
      <!--2、这边切换到右组件，再切换回左组件。发现count又变成了初始状态，count=1-->
      <div style="text-align: center;padding-left: 480px;">
        <component :is="componentName"></component>
      </div>

    </div>
  </template>

  <script>



  import Left from "@/components/Left";
  import Right from "@/components/Right";
  export default {
    name: 'App',
    components:{
      Right,
      Left

    },
    data(){
      return {
         componentName:''
      }
    },

    methods: {



    }
  }
  </script>

  <style lang="less">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  </style>

```

# 2、Use keep-alive

```vue
  <template>
    <div id="app">

      <button @click="componentName='left'">渲染组件左组件</button>
      <button @click="componentName='right'">渲染组件右组件</button>

      <div style="text-align: center;padding-left: 480px;">
        <!--3、使用了keep-alive就保证组件切换前后Model状态一致的问题-->
        <keep-alive>
          <component :is="componentName"></component>
        </keep-alive>

      </div>

    </div>
  </template>

  <script>



  import Left from "@/components/Left";
  import Right from "@/components/Right";
  export default {
    name: 'App',
    components:{
      Right,
      Left

    },
    data(){
      return {
         componentName:''
      }
    },

    methods: {



    }
  }
  </script>

  <style lang="less">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  </style>

```

keep-alive 对应的生命周期函数

当组件被缓存时，会自动触发组件的 deactivated 生命周期函数

 当组件被激活时，会自动触发组件的 activated 生命周期函数

![image-20220710141413270](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710141413270.png)
