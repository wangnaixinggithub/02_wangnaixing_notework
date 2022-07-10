# Vue-main.ts-APP

# 1、Use Vue #createApp() Create app

Vue工程一启动就会加载main.ts，然后渲染`App` 组件的内容，最后挂载管控index.html。

```typescript
//	src/main.ts
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

console.log(app)

app.mount('#app')

```

# 2、Print APP

VM非常的笨重，有许多的方法和属性。VUE3，我们使用`App` 轻量级对象来记性优化性挂载。

```js
Object
component: ƒ component(name, component)
config: (...)
directive: ƒ directive(name, directive)
mixin: ƒ mixin(mixin)
mount: containerOrSelector => {…}
provide: ƒ provide(key, value)
unmount: ƒ unmount()
use: ƒ use(plugin, ...options)
version: "3.2.37"
_component: {components: {…}, extends: {…}, methods: {…}, computed: {…}, setup: ƒ, …}
_container: div#app
_context: {app: {…}, config: {…}, mixins: Array(0), components: {…}, directives: {…}, …}
_instance: {uid: 0, vnode: {…}, type: {…}, parent: null, appContext: {…}, …}
_props: null
_uid: 0
get config: ƒ config()
set config: ƒ config(v)
[[Prototype]]: Object
```

