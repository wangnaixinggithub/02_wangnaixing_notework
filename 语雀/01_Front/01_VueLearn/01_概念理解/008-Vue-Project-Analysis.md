# Vue-Project-Analysis

# 1、index.html bind Vue Instance

进入`index.html`，查看源代码可以看到`body` 中有一块`div#id=app`的区域。此H5页面。

![image-20220708094502231](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708094502231.png)

 底层会把`main.js`中创建的Vue 实例和该`index.html` 进行绑定。由Vue全局管控。

```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')

```

# 2、Project  Analysis

一个完整的VUE工程化项目有：

`dist`:代表由WebPack进行项目打包后生成的工程文件

`node_moudles` : VUE工程基于`Node.js` 构建，需要到`Node.js ` 的底层依赖支撑。

`public`: 存放图标以及唯一的HTML页面：`index.html`

`VUE工程的所有配置文件`：绿色框区域

![image-20220708095315841](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708095315841.png)

# 3、Src Analysis

`src`  ：源码文件夹，是我们程序员最关心的区域了。

`assets` ：存放VUE工程的静态资源，比如图片资源，CSS资源，规定统一放到`assets`目录下

`components`:存放程序员编写的VUE组件

`main.js`: VUE程序的入口文件，当VUE工程执行，首要先执行`main.js`

`App.vue`: 根VUE组件，打开页面最先展示的内容就是`App.vue`中的内容

![image-20220708095930375](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708095930375.png)