# Install-Oracle 11g-Software

# 1、Download

```
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

# 2、Install

- 运行

```shell
docker run -d -p 1521:1521 --name oracle11g registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g 
```

- 配置Oracle 变量

```shell
# 1、进入容器内部
docker exec -it oracle11g bash
# 2、切换为Root用户 密码为：helowin
su root
# 3、编辑配置文件
vi /etc/profile 
```

```shell
# 4、输入如下内容
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2 
export ORACLE_SID=helowin 
export PATH=$ORACLE_HOME/bin:$PATH
```

```shell
# 5、重新加载环境变量
source /etc/profile 
# 6、创建软连接
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin 
#7、切换为Oracle用户
su oracle 
```

```shell
# 8、登录SqlPlus
sqlplus /nolog 

# 9、连接Oracle
conn /as sysdba 

# 10、修改system用户账号密码
alter user system identified by system;

#11、修改sys用户账号密码
alter user sys identified by system;

#12、创建一个我们自己的test账号 密码也是test
create user test identified by test;

#13、给test账号赋予数据库权限
grant connect,resource,dba to test;

# 14、修改密码规则策略为密码永不过期
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;

#15、修改数据库最大连接数
alter system set processes=1000 scope=spfile;


#16、退出SqlPlus
exit

#16、退出Oracle 内部
exit

#18、退出Oracle容器
exit

#19、得到Oracle容器ID
docker ps 

#20、重启Oracle容器
docker restart 容器ID

```

# 3、Start

使用远程连接  写入`SID`  账号密码。记得把防火墙1521端口打开，如果是云服务器，安全组记得防线1521端口。

- In DataGr



![image-20220703162725477](010-Install-Oracle%2011g-Software.assets/image-20220703162725477.png)

- Navicat

![image-20220703163415074](010-Install-Oracle%2011g-Software.assets/image-20220703163415074.png)
