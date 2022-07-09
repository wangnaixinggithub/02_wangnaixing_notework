# Liunx-Config-Network-Can-Use

# 1、Need Config

> 在Liunx虚拟机中，配置可以上网。

```shell
# 1、修改 ONBOOT=yes 让网络服务，当系统开机时就运行。
vim /etc/sysconfig/network-scripts/ifcfg-ens33
# 2、修改 ONBOOT=yes

# 3、重启 network 服务
systemctl restart network

#4、使用ping命令 进行 网络连通性测试
ping baidu.com
```

# 2 、Update Network Config

![image-20220704000523032](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704000523032.png)

# 3、Validation Test Success

![image-20220704000644273](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704000644273.png)



