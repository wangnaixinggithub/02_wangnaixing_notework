# TS-Install-Environment

# 1、Download

```shell
 npm install -g typescript
```

# 2、Start

- helloworld

```javascript
//hello.ts
let str:string = 'Hello TypeScript!!'

console.log(str)
```

```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>TypeScript-HelloWorld</title>
</head>
<body>
<script src="./hello.js"></script>
</body>
</html>
```

- 编译执行

```shell
tsc .\hello.ts
```

