#  CommonJS的模块化实现

> CommonJS需要nodeJS的依赖支持。

# 1、module.exports

aaa.js

```javascript
let name = '小明'
let age = 22

const sum = (num1, num2) =>  num1 + num2

let flag = true


if (flag) {
  console.log(sum(10, 20))
}


//导出对象
module.exports = {
  flag,
  sum
}
```

#  2、require()

ccc.js

```javascript
//导入对象,nodejs语法,需要node支持,从aaa.js取出对象

let {flag,sum} =  require("./aaa")

console.log(sum(10,20));

if(flag){
  console.log("flag is true");
}
```

