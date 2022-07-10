# Vue-Slot-Use-Slot-Set-Component-Context

# 1、Children Component Use slot Label

```vue
<template>
<div>
  <h3>MyComponent....Context....Start</h3>
<!--1、预留空间，这部分期待父组件填充-->
  <slot></slot>

  <h3>MyComponent....Context....End</h3>
</div>

</template>

<script>
export default {
  name: "MyComponent"
}
</script>

<style scoped>

</style>
```

# 2、Parent Component  Fill Context

```vue
  <template>
    <div id="app">
      <MyComponent>
  <!--2、父组件往里头填充内容       -->
        <p>HelloWorld ...Slot</p>
      </MyComponent>

    </div>
  </template>

  <script>


  import MyComponent from "@/components/MyComponent";
  export default {
    name: 'App',
    components:{
      MyComponent
    },
    data(){
      return {

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

# 3、Effect Result

![image-20220710143012197](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710143012197.png)

