# Install-Nginx-Software

# 1、Download

下载Linux平台的稳定版本

```properties
官网：https://nginx.org/en/download.html
```

![image-20220703173254780](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703173254780.png)

# 2、Install

Nginx是C写的，在安装前，必须保证Linux具备C的环境。

```shell
yum -y install gcc-c++ 
yum -y install pcre pcre-devel
yum -y install zlib zlib-devel
yum -y install openssl openssl-devel  
```



```shell
#1、解压Nginx.gz包
tar -zxvf nginx-1.22.0.tar.gz

#2、进入此Nginx文件
cd nginx-1.22.0

#3、使用Nginx的自动配置命令
./configure

#4、执行make命令
make

#5、执行make install 命令
make install

#6、查看 Nginx 安装以后的位置
whereis nginx

#7、	进入安装目录
cd /usr/local/nginx

#8、进入sbin目录，目的是要执行可执行的文件
cd sbin

#9、查找sbin目录下，可执行文件
ll

#10、执行
./nginx

```

# 3、Start

放行Linux系统的80端口，如果是云服务器，在安全组配置放行80端口。

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
systemctl restart firewalld.service
```

访问 http://hostname:80 ,成功就说明安装成功

![](011-Install-Nginx-Software.assets/image-20220703181626717.png)