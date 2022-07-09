# NodeJs-Expree-Use-Nodemon-Hot-Deployment

# 1、Update System Config

- 以管理员身份，打开`Windows PowerShell`

```shell
#1、修改执行策略为远程执行
set-ExecutionPolicy RemoteSigned  

#2、确认你的操作
Y

#3、验证策略已修改
get-ExecutionPolicy
```

# 2、Download

```shell
 npm i  -g nodemon
```

# 3、Start

- 启动命令不再使用node命令， 采用`nodemon`命令，就能实现修改Express项目内容时，同步刷新，进行热部署。

```shell
nodemon .\app.js 
```

