# Use-For-of-get-Obj-Property

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>遍历对象数组的元素对象的属性</title>
</head>
<body>
<script>
 function User(id,name,isMarried){
     this.id = id
     this.name = name
     this.isMarried = isMarried
 }

 let objArr =  [
     new User(1,'小明','是'),
     new User(2,'小红','否'),
     new User(3,'小张','否'),
     new User(4,'小李','是'),

 ]

 //todo:     successTip:成功打印出来，每个User的 isMarried 属性。
 for (const {isMarried} of objArr) {
    console.log(isMarried)
 }

 //todo:     successTip:成功打印出来，每个User的 id 属性
 for (const {id} of objArr) {
     console.log(id)

 }
 
 //todo:     successTip:成功打印出来，每个User的 name 属性
 for (const {name} of objArr) {
     console.log(name)
 }



</script>
</body>
</html>
```

