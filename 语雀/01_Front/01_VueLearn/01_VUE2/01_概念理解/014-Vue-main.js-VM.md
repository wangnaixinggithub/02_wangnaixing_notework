# Vue-main.js-VM

# 1、Use Vue Constructor Create VM

Vue工程一启动就会加载main.js，然后渲染`App` 组件的内容，最后挂载管控index.html。

```javascript
//	src/main.js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

let vm = new Vue({
  render: h => h(App),
})
//打印VM
console.log(vm)

vm.$mount('#app')

```

# 2、Print VM

VM非常的笨重，有许多的方法和属性。在VUE3的时候，就不是使用VM来做挂载了，而是使用`App` 对象。

```javascript
Vue
$attrs: (...)
$children: [VueComponent]
$createElement: ƒ (a, b, c, d)
$el: div#app
$listeners: (...)
$options: {components: {…}, directives: {…}, filters: {…}, _base: ƒ, render: ƒ}
$parent: undefined
$refs: {}
$root: Vue {_uid: 0, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
$scopedSlots: {}
$slots: {}
$vnode: undefined
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
_renderProxy: Proxy {_uid: 0, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
_scope: EffectScope {active: true, effects: Array(1), cleanups: Array(0)}
_self: Vue {_uid: 0, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
_staticTrees: null
_uid: 0
_vnode: VNode {tag: 'vue-component-1-App', data: {…}, children: undefined, text: undefined, elm: div#app, …}
_watcher: Watcher {vm: Vue, deep: false, user: false, lazy: false, sync: false, …}
$data: (...)
$isServer: (...)
$props: (...)
$ssrContext: (...)
get $attrs: ƒ reactiveGetter()
set $attrs: ƒ reactiveSetter(newVal)
get $listeners: ƒ reactiveGetter()
set $listeners: ƒ reactiveSetter(newVal)
[[Prototype]]: Object
```

