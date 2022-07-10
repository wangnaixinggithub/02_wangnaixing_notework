# ES6-高阶函数-filter过滤函数

# 1、Number [ ]

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>filter过滤函数</title>
</head>
<body>
<script>
    let numArr = [1,2,3,4,5,6]
    
    console.log('过滤之前 => ',numArr)      //过滤之前 =>  (6)  [1, 2, 3, 4, 5, 6]

    let newNumArr = numArr.filter(num => num >= 5);

    console.log('过滤之后 =>',newNumArr)   //过滤之后 => (2) [5, 6]


</script>
</body>
</html>
```

# 2、String [ ]

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>filter过滤函数</title>
</head>
<body>
<script>

    let strArr = ['w','n','x']
    console.log('过滤之前 => ',strArr)   //过滤之前 =>  (3)     ['w', 'n', 'x']

    let newStrArr = strArr.filter(str => str.includes('w'));

    console.log('过滤之后 => ',newStrArr) //过滤之后 =>  ['w']

</script>
</body>
</html>
```

# 3、Object [ ]

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>filter过滤函数</title>
</head>
<body>
<script>

    function User(name,age){
        this.name = name
        this.age =age
    }
    
    let user1 = new User('王乃醒',26)
    let user2 = new User('王小二',27)
    let user3 = new User('王小三',27)
    let objArr = [user1,user2,user3]

    console.log('过滤之前 => ',objArr)   //过滤之前 =>  (3)     ['w', 'n', 'x']
    
    let newObjArr =  objArr.filter(obj => obj.age >= 27)

   console.log('过滤之后 => ',newObjArr) //过滤之后 =>  ['w']



</script>
</body>
</html>
```

![image-20220514004127170](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220514004127170.png)

