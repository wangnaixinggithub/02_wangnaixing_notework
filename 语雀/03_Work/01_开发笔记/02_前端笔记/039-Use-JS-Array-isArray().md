# Use-JS-Array-isArray()

- 判断是不是数组的方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10_Array类的isArray函数,判断当前变量是不是一个数组？</title>
</head>
<body>
<script>
    let numArr = [1,2,3,4]
    let strArr = ['1','2','3','4']
    function User(name,age) {
        this.name = name
        this.age = age
    }
    let user1 = new User('王乃醒',25)
    let user2 = new User('张三',27)
    let objArr = [user1,user2]
    
    //JS世界里,数组就是对象 所以有了这个isArray方法，便于开发这区分是对象还是数组
    
    
    //Array.isArray()=>判断数组类型数组. true
    console.log('Array.isArray()=>判断数组类型数组.',Array.isArray(numArr))
   
    //Array.isArray()=>判断字符串类型数组. true
    console.log('Array.isArray()=>判断字符串类型数组.',Array.isArray(strArr))
  
    //Array.isArray()=>判断对象类型数组. true
    console.log('Array.isArray()=>判断对象类型数组.',Array.isArray(objArr))
</script>
</body>
</html>
```

