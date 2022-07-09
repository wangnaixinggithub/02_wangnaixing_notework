# NodeJs-path-#join()-joinStrPath

> 专门用来拼接路径的！

# 1、How Use ?

```javascript
const fs = require('fs')
const path = require('path')

let inputData = '王乃醒是一个大帅哥哈哈哈'
fs.writeFile(path.join(__dirname,'/output.txt'),inputData,err=>{
    console.log(err)
})
```

