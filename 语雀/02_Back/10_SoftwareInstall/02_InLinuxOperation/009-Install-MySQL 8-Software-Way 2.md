# Install-MySQL 8-Software-Way 2

# 1、Download

至官网下载`mysql-8.0.27-1.el7.x86_64.rpm-bundle.tar`

![image-20211231194924038](009-Install-MySQL%208-Software-Way%202.assets/HDrjGNfPQXgy9ST.png)

- 上传至Linux

![image-20211231231055823](009-Install-MySQL%208-Software-Way%202.assets/HBLZUDYwIA62jq7.png)

# 2、Install

- 解压npm总包

```shell
tar -xvf mysql-8.0.27-1.el7.x86_64.rpm-bundle.tar 
```

- 按顺序解压,如下npm包。

```shell
rpm -ivh mysql-community-common-8.0.11-1.el7.x86_64.rpm /
rpm -ivh mysql-community-libs-8.0.11-1.el7.x86_64.rpm /
rpm -ivh mysql-community-libs-compat-8.0.11-1.el7.x86_64.rpm /
rpm -ivh mysql-community-client-8.0.11-1.el7.x86_64.rpm /
rpm -ivh mysql-community-server-8.0.11-1.el7.x86_64.rpm /
```

- 如遇到失败,出现依赖问题.可尝试清除yum里所有MySQL依赖包.

```shell
yum remove mariadb-libs
```

# 3、Start

- 启动服务，设置为开机自启动模式。

```shell
systemctl start mysqld.service
systemctl enable mysqld.service
```

# 4、UnInstall

- 找出MySQL安装位置,查看MySQL组件和占用文件夹.

```shell
rpm -qa | grep -i mysql 
service mysqld status
find / -name mysql
```

- 移除MySQL组件

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

- 验证是否清理干净

```shell
rpm -qa | grep -i mysql
```

