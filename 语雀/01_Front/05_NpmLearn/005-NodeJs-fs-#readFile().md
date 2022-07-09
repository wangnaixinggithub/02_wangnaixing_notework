# NodeJs-fs-#readFile()

# 1、API 

> 运行读入项目中的文件。

- 参数path,需要指定一个文件路径的字符串，表示文件的存放路径。
- 可选参数，表示以什么编码格式来读取文件.
- 必选参数，文件读取完成后，通过回调函数拿到读取的结果。

![image-20220612150050599](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220612150050599.png)



# 2、Use

```javascript
const fs = require('fs')

fs.readFile('./info.txt','UTF-8',(err,dataString)  =>{
    
    //读取成功，err值为null.
    Object.is(null,err) 
     
    //dataString 存储读取得到的内容
    console.log(dataString)
})
```

