# Install-MySQL 8-Software

# 1、Download

进入官网，选择好MySQL的版本，以及使用的操作系统。下载211.7M的文件压缩包。

```
官网：https://downloads.mysql.com/archives/community/
```

![image-20220702121324965](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702121324965.png)

# 2、Install

- 将MySQL软件包解压在没有中文和空格的目录下:

![image-20220702121602733](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702121602733.png)

- 在解压目录创建my.ini文件并添加内容如下:

![image-20220702121806974](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702121806974.png)

```ini
[mysqld]
#设置3306端口号
port=3306

#服务端使用的字符集默认为utf-8
character-set-server=utf8mb4

#设置MySQL的安装目录
basedir=D:\\my_software\\mysql-8.0.28-winx64

#设置MySQL数据库的数据存放目录
datadir=D:\\my_software\\mysql-8.0.28-winx64\\data

#运行最大连接数
max_connections=200

#运行连接失败的次数。这也是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10

[mysql]
#MySQL服务端使用的字符集默认为utf8
default-character-set=utf8mb4

[client]
#客户端默认端口号为3306
port=3306

#客户端使用的字符集默认为utf8
default-character-set=utf8mb4
```

- 新增系统变量，添加到PATH环境变量中。

```
MYSQL_HOME
D:\my_software\mysql-8.0.28-winx64
```

![image-20220702122838555](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702122838555.png)

```
%MYSQL_HOME%\bin
```

![image-20220702123107267](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702123107267.png)

# 3、Start

以管理员身份进入控制台，执行下面得操作流程，就可以连接到MySQL数据库了。

```sql
#1、对mysql进行初始化，请注意，这里会生产一个临时密码，后边要使用这个临时密码  _#cbk-VMF3!u
mysqld --initialize --user=mysql --console

#2、安装mysql服务
mysqld --install 

#3、启动mysql服务_
net start mysql

#4、登录mysql，这里需要使用之前生产的临时密码 
mysql -uroot –p 

#5、修改root用户密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';

#6、修改root用户权限
create user 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
```

