# NodeJs-module-exports

# 1、Default Case

- 默认情况下，NodeJs的每一个模块都有`module`对象。其中export属性为`{}`.
- 在自己模块里里面，定义的变量和方法，不会对外暴露，只在当前模块有效（模块作用域）。

```javascript
Module {
  id: '.',
  id: '.',
  path: 'E:\\vue-learn\\01-nodejsLearn',
  exports: {},
  parent: null,
  filename: 'E:\\vue-learn\\01-nodejsLearn\\a.js',
  loaded: false,
  children: [],
  paths: [
    'E:\\vue-learn\\01-nodejsLearn\\node_modules',
    'E:\\vue-learn\\node_modules',
    'E:\\node_modules'
  ]
}

```

# 2、module-exports

> 我们可以通过`module.exports`属性，把变量或者方法进行对外暴露。

```javascript
//a.js
const obj = {
    id:1,
    name:'王乃醒',
    age:26,
    hobby:'打游戏'
}


const myfun = () => {
    console.log('This is My Function..')
}

module.exports = {
    myfun,
    obj
}

console.log(module)
```

# 3、require()

> 外界用 require() 方法导入自定义模块时，得到的就是 `module.exports` 所指向的对象。

```javascript
//b.js
const a = require('./a')

a.myfun()

console.log(a.obj);
```

# 4、Validation Test

![image-20220612161638564](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220612161638564.png)