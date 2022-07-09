# Linux-Config-MySQL

- 查看root账号零时登录密码

```
more /var/log/mysqld.log
```

- 使用零时密码登录MySQL

```
mysql -uroot -p 
```

- 登录成功后，进入MySQL控制台，修改root密码为`WZXYwnx1234*`

```sql
alter user'root'@'localhost' identified by "WZXYwnx1234*";
```

- 配置远程连接MySQL数据库。

```sql
use mysql;
update user set host='%' where user='root' limit 1;
flush privileges;
```

