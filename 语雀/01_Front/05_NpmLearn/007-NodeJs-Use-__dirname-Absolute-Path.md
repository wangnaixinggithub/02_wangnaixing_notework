# NodeJs-Use-`__dirname`-Absolute-Path

# 1、What Mean?

> `__dirname`,代表 代码在运行的时候，会以执行 node 命令时所处的目录，动态拼接出被操作文件的完整路径。

```javascript
const fs = require('fs')
console.log(__dirname)
```

![image-20220612151339585](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220612151339585.png)

# 2、So Use It,As Absolute Path

```javascript
const fs = require('fs')

fs.readFile(__dirname + '/input.txt','UTF-8',(err,dataStr)=>{
    console.log(dataStr)
})
```

```javascript
const fs = require('fs')

let inputData = '王乃醒是一个大帅哥哈哈哈'
fs.writeFile(__dirname+'/output.txt',inputData,err=>{
    console.log(err)
})
```

