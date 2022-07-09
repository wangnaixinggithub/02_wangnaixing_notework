# NodeJs-path-#extname()

> 获取到 文件扩展名。

```javascript
const path = require('path')

let joinPath = path.join(__dirname,'file','1.txt');

let extname = path.extname(joinPath);

Object.is('.txt',extname)
```

