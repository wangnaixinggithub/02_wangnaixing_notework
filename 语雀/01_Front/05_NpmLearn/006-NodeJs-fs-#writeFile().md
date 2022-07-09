# NodeJs-fs-#writeFile()

> 将信息写入文本文件中

- 参数1：必选参数，需要指定一个文件路径的字符串，表示文件的存放路径。
- 参数2：必选参数，表示要写入的内容。
- 参数3：写入格式，默认值是 `UTF-8`。 
- 参数4：必选参数，文件写入完成后的回调函数。

![image-20220612150647164](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220612150647164.png)

```javascript
const fs = require('fs')

let inputData = "My Life is Movie ,I'am So Happy!"

fs.writeFile('info.txt',inputData,'UTF-8',err => {
    //成功写入 error 为 null
    Object.is(null,err)
})
```

