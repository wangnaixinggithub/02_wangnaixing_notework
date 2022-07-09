# 012-JDK-keyTool-generate-Https-certificate

# 1、JDK-keytool

- 使用keytool 命令
- -genkey来生成密钥
-  -alias tomcathttps 设置别名为tomcathttps
- -keyalg  RSA 使用算法名称RSA
- -keysize 2048 密钥位数为2048
-  -keystore D:\wangnaixing.p12 保存在D盘
- -validity 365 有效天数为365天

```xml
-alias <alias>                  要处理的条目的别名
 -keyalg <keyalg>                密钥算法名称
 -keysize <keysize>              密钥位大小
 -sigalg <sigalg>                签名算法名称
 -destalias <destalias>          目标别名
 -dname <dname>                  唯一判别名
 -startdate <startdate>          证书有效期开始日期/时间
 -ext <value>                    X.509 扩展
 -validity <valDays>             有效天数
 -keypass <arg>                  密钥口令
 -keystore <keystore>            密钥库名称
 -storepass <arg>                密钥库口令
 -storetype <storetype>          密钥库类型
 -providername <providername>    提供方名称
 -providerclass <providerclass>  提供方类名
 -providerarg <arg>              提供方参数
 -providerpath <pathlist>        提供方类路径
 -v                              详细输出
 -protected                      通过受保护的机制的口令
```

```shell
keytool -genkey -alias tomcathttps -keyalg  RSA -keysize 2048 -keystore D:\wangnaixing.p12 -validity 365
```

```
输入密钥口令：wzxy1234
再次输入密钥口令：wzxy1234
您的名字与姓氏是什么? wangnaixing
您的组织单位名称是什么? huasu
您的组织名称是什么? huasu
您所在的城市或区域名称是什么? guangzhou 
您所在的省/市/自治区名称是什么? guangdong
该单位的双字母国家/地区代码是什么? Zh
```

进入到D盘下，发现已经出现一个wangnaixing.p12的文件哦！

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/112-%E6%B1%9F%E5%8D%97%E4%B8%80%E7%82%B9%E9%9B%A8-%E4%BD%BF%E7%94%A8JDK%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7keytool%E7%94%9F%E6%88%90%E4%B8%80%E4%B8%AA%E5%85%8D%E8%B4%B9%E7%9A%84Https%E7%9A%84%E8%AF%81%E4%B9%A6.assets/image-20220327115103986.png)
