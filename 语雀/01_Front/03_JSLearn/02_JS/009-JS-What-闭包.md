# JS-What-闭包

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-What-闭包</title>
</head>
<body>
<script>
    //方法的返回值是一个函数
    function test(){
      return   function print(){
            console.log('王乃醒真帅！')
        }
    }

    
    //调用
    test()()


</script>
</body>
</html>
```

