# Install-Virtual-Machine-CentOs7

# 1、DownLoad

在王乃醒的阿里云盘也备份了一份的

```properties
下载地址：https://vault.centos.org/centos/7.4.1708/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso
```

## 1、Create Virtual Machine

![image-20220703233954526](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703233954526.png)

## 2、Choose Centos7 mini ISO

![image-20220703234101756](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234101756.png)

## 3、Set Storage location

![image-20220703234139449](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234139449.png)

## 4、Set Virtual Machines Disk Size

![image-20220703234233513](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234233513.png)

## 5、Other Option

在自定义硬件中，我们可以再次配置虚拟机的内存、CPU等硬件属性，默认为单核CPU、1G内存即可。

![image-20220703234310122](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234310122.png)

# 2、Install

## 1、Choose System Language

![image-20220703234547358](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234547358.png)

## 2、Partition configuration

![image-20220703234638753](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234638753.png)

![image-20220703234701585](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234701585.png)

![image-20220703234718985](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234718985.png)

## 3、Set Root Password And Create User Set Password

![image-20220703234803018](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234803018.png)

![image-20220703234818201](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234818201.png)

# 3、Start

安装完成之后，会自动重启，系统会让你输入用户登录，此时我以`root`账户执行登录。

```properties
账户：root
密码：wzxy1234
```

![image-20220703234917003](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220703234917003.png)