# Nginx-Update-System-Host-Can-Use-DomainName-Access

# 1、Update hosts File Permissions

修改本机系统文件的控制权限为:完全控制。

另一种方法是：移动到其他有权限的盘，比如D盘，然后再覆盖原文件。

![image-20220704123419582](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220704123419582.png)

# 2、Update Host File



```yaml
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
127.0.0.1       activate.navicat.com
192.168.206.130 wangnaixing.com

```



# 3、Validation Success



![image-20220704125026869](006-Nginx-Update-System-Host-Can-Use-DomainName-Access.assets/image-20220704125026869.png)
