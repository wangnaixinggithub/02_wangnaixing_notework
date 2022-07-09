# Install-Sqlyog12-Software

# 1、Download

```
官网：https://sqlyog.en.softonic.com/
```

# 2、Install

一路Next即可完成安装。这边提供激活账户

```
姓名（Name）：cr173

序列号（Code）：8d8120df-a5c3-4989-8f47-5afc79c56e7c

或者（OR）

姓名（Name）：cr173

序列号（Code）：59adfdfe-bcb0-4762-8267-d7fccf16beda

或者（OR）

姓名（Name）：cr173

序列号（Code）：ec38d297-0543-4679-b098-4baadf91f983
```

# 3、Start

# 3、Start

在使用SQlyog，可能无法连接会出现如下错误，则可以实现MySQL库命令进行操作下就可以。



![image-20220702195419479](013-Install-Sqlyog12-Software.assets/image-20220702195419479.png)

```sql
#修改加密规则
 ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; 
 #更新用户的密码
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root'; 
 #刷新权限
 FLUSH PRIVILEGES;
```

