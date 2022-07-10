# Vue-Use-ref-Get-Component-Instance

- 在父组件中，我们使用子组件的方式来复用样式，通过ref，我们可以得到加载在父组件的子组件实例。

# 1、This is My Children Component

```vue
<template>
<div style="border: 1px solid red;height: 300px;width: 300px;">
  <h2>子组件1</h2>
  <button @click="method1">按钮1</button>
  <button @click="method2">按钮2</button>
</div>
</template>

<script>

export default {
  name: "Left",
  data(){
    return {
      msg:"Hello Vue ,Mr.WangNaiXing Say Hello..."
    }
  },
  methods:{
    method1(){
      console.log('method1...')
    },
    method2(){
      console.log('method1...')
    }
  },

}
</script>

<style scoped>

</style>
```

# 2、Use ref

```vue
<template>
  <div id="app">
    <left ref="left"></left>
    <button @click="getLeftComponents">Ref可以获取组件实例</button>
  </div>
</template>

<script>

import Left from "@/components/Left";
export default {
  name: 'App',
  data(){
    return {
      count:1,
    }
  },
  components: {
    Left

  },
  methods: {
    getLeftComponents(){
      let leftComponent = this.$refs['left']
      console.log(leftComponent)

    }

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

# 3、Observe The Console Print

可以看到，这个子组件实例的所有的属性和方法都打印出来了，比如我定义的`Model#msg` `Method#method1() #method2()`

```javascript
VueComponent
$attrs: (...)
$children: []
$createElement: ƒ (a, b, c, d)
$el: div
$listeners: (...)
$options: {parent: VueComponent, _parentVnode: VNode, propsData: undefined, _parentListeners: undefined, _renderChildren: undefined, …}
$parent: VueComponent {_uid: 3, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
$refs: {}
$root: Vue {_uid: 0, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
$scopedSlots: {$stable: true, $key: undefined, $hasNormal: false}
$slots: {}
$vnode: VNode {tag: 'vue-component-2-Left', data: {…}, children: undefined, text: undefined, elm: div, …}
method1: ƒ ()
method2: ƒ ()
msg: "Hello Vue ,Mr.WangNaiXing Say Hello..."
__v_skip: true
_c: ƒ (a, b, c, d)
_data: {__ob__: Observer}
_directInactive: false
_events: {}
_hasHookEvent: false
_inactive: null
_isBeingDestroyed: false
_isDestroyed: false
_isMounted: true
_isVue: true
_provided: {}
_renderProxy: Proxy {_uid: 4, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
_scope: EffectScope {active: true, effects: Array(1), cleanups: Array(0)}
_self: VueComponent {_uid: 4, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
_staticTrees: null
_uid: 4
_vnode: VNode {tag: 'div', data: {…}, children: Array(3), text: undefined, elm: div, …}
_watcher: Watcher {vm: VueComponent, deep: false, user: false, lazy: false, sync: false, …}
$data: (...)
$isServer: (...)
$props: (...)
$ssrContext: (...)
get $attrs: ƒ reactiveGetter()
set $attrs: ƒ reactiveSetter(newVal)
get $listeners: ƒ reactiveGetter()
set $listeners: ƒ reactiveSetter(newVal)
get msg: ƒ proxyGetter()
set msg: ƒ proxySetter(val)
[[Prototype]]: Vue
```

