# Use-JS-object-hasOwnProperty()

- 对象特有的`hasOwnproperty()` 可以判断属性是不是这个对象中的属性？

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12_JS_对象方法_hasOwnProperty()_是否是自己的属性？</title>
</head>
<body>
<script>
    let emp = {
        empId: 1,
        empName: '王乃醒',

        run:function () {
            console.log('Go Running!')
        },
        dept:{
            deptId:3,
            deptName:'开发一部'
        }
    }

    //判断是这个对象本身的属性？
    console.log(emp.hasOwnProperty('empId'))
    console.log(emp.hasOwnProperty('empName'))
    console.log(emp.hasOwnProperty('run'))
    console.log(emp.hasOwnProperty('dept'))  //全为true!

    console.log(emp.hasOwnProperty('dept.deptId'))
    console.log(emp.hasOwnProperty('dept.deptName'))
    console.log(emp.hasOwnProperty('deptId'))
    console.log(emp.hasOwnProperty('deptName')) //全为false!

</script>
</body>
</html>
```

