# Install-MySQL 8-Software

# 1、Downloads

- 在官网下载`xxx.noarch.rpm`的yum文件.

![image-20211231231055823](https://s2.loli.net/2021/12/31/HBLZUDYwIA62jq7.png)

# 2、Install

- 解压rpm包，安装`mysql-community-server`

```shell
rpm -ivh mysql80-community-release-el7-4.noarch.rpm 
yum install mysql-community-server
```

# 3、Start

```shell
systemctl start mysqld.service
systemctl enable mysqld.service
```



# 4、unInstall

- 找出MySQL安装位置,查看MySQL组件和占用文件夹.

```shell
rpm -qa | grep -i mysql 
service mysqld status
find / -name mysql
```

- 卸载MySQL相关组件。

```shell
rpm -ev --nodeps mysql-community-libs-8.0.11-1.el7.x86_64
rpm -ev --nodeps mysql-community-common-8.0.11-1.el7.x86_64
rpm -ev --nodeps mysql-community-server-8.0.11-1.el7.x86_64
rpm -ev --nodeps mysql-community-client-8.0.11-1.el7.x86_64
rpm -ev --nodeps mysql-community-libs-compat-8.0.11-1.el7.x86_64
```

- 移除MySQL用的的文件夹

```shell
rm -rf  /etc/selinux/targeted/active/modules/100/mysql
rm -rf  /etc/logrotate.d/mysql
rm -rf  /usr/bin/mysql
rm -rf  /usr/lib64/mysql
rm -rf  /var/lib/mysql
rm -rf  /var/lib/mysql/mysql
```

- 二次验证，是否删除干净了。

```shell
rpm -qa | grep -i mysql
```

