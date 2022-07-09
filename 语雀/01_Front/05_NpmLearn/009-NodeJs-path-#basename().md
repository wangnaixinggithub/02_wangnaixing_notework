# NodeJs-path-#basename()

> 获取到对应的文件名。

```javascript
const path = require('path')

let joinPath = path.join(__dirname,'file','1.txt');

let fileName = path.basename(joinPath);

Object.is('1.txt',fileName)
```

