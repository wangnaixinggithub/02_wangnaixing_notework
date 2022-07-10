# JS-Array-Method-splice

# 1、delete Element

- 删除单个元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-split()</title>
</head>
<body>
<script>
	
    let arr = ['aaa','bbb','ccc']
	//删除 索引为1所在的一个元素，即bbb
    arr.splice(1,1)

    console.log(arr)  //['aaa','ccc']
</script>
</body>
</html>
```

- 删除多个元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-split()</title>
</head>
<body>
<script>

    let arr = ['aaa','bbb','ccc']
	//删除 索引为1所在的二个元素，即bbb ccc
    arr.splice(1,2) // ['aaa']

    console.log(arr)


</script>
</body>
</html>
```

# 2、Update Element

- 替换索引为1的一个元素为`ddd.`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-split()</title>
</head>
<body>
<script>

    let arr = ['aaa','bbb','ccc']

    arr.splice(1,1,'ddd') //['aaa','ddd','ccc']

    console.log(arr)


</script>
</body>
</html>
```

- 同理可以替换多个元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-split()</title>
</head>
<body>
<script>

    let arr = ['aaa','bbb','ccc']

    arr.splice(1,2,'ddd','eee') //['aaa','ddd','eee']

    console.log(arr)


</script>
</body>
</html>
```

# 3、Insert Element

- 添加一个元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-split()</title>
</head>
<body>
<script>

    let arr = ['aaa','bbb','ccc']

    arr.splice(1,0,'ddd') //['aaa','ddd','bbb','ccc']

    console.log(arr)


</script>
</body>
</html>
```

- 同理可以添加多个元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-split()</title>
</head>
<body>
<script>

    let arr = ['aaa','bbb','ccc']

    arr.splice(1,0,'ddd','eee') //['aaa','ddd','eee','bbb','ccc']

    console.log(arr)


</script>
</body>
</html>
```

