# Vue-Project-main.js

分析：`main.js` 代码逻辑 ，目的就是把`App`根组件的内容展示到`public/index.html`中，并创建的`Vue` 对象，接管了`index.html` 中元素`ID=#app` 的区域。

```javascript
// 1、导入vue
import Vue from 'vue'

//2、导入项目中App.vue 组件
import App from './App.vue'

//在生产环境时 不显示任何VUE警告信息
Vue.config.productionTip = false

//3、构造Vue实例
new Vue({

  //4、使用render函数，将App.vue 渲染到 HTML页面上
  render: h => h(App),

 // 5、让VUE管控HTML页面中 ID = app 的元素
}).$mount('#app')

```

