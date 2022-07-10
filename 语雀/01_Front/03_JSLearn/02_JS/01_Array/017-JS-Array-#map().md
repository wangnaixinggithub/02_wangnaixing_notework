# 高阶函数-map映射函数

# 1、Number [ ]

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>08.map高阶函数</title>
    </head>
    <body>
    <script>
        let numArr = [1,2,3,4,5,6]
        numArr.map(num => console.log('item => ',num))
    </script>
    </body>
    </html>
```

![image-20220514094225143](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220514094225143.png)

# 2、String [ ]

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>08.map高阶函数</title>
    </head>
    <body>
    <script>
        let strArr = ['w','n','x']
        strArr.map(num => console.log('item => ',num))
    </script>
    </body>
    </html>
```

![image-20220514094353975](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220514094353975.png)

# 3、Object [ ]

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>08.map高阶函数</title>
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

        objArr.map(obj => console.log('obj => ',obj))

    </script>
    </body>
    </html>
```

![image-20220514094617129](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220514094617129.png)