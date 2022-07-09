# Ip2region-HelloWorld

在进行系统开发时，我们一般会涉及到获取到用户的具体位置信息，一般有两个方法：

- 根据GPS 定位的信息 （一般用于手机端）
- 用户的 IP 地址解析

每个手机都不一定会打开 GPS，而且有时并不太需要太精确的位置（到城市这个级别即可），所以根据 IP 地址入手来分析用户位置是个不错的选择。

# 1、What Is Ip2Region ?

![image-20220707192206262](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220707192206262.png)

其实就是一个库，用于 IP地址解析定位用的，支持很多客户端，速度很快也不占内存，目前`Github`已经到了`10k star` 了

特性：

准确率高，数据聚合多个供应商的数据
体积小，空间优化形成了仅仅几兆的 ip2region.db 文件
速度快，内置三种查询算法，支持毫秒级查询
支持多客户端，支持很多客户端，`java、C#、php、c、python、nodejs、php扩展(php5和php7)、golang、rust、lua、lua_c, nginx`
它的数据聚合了一些知名IP到地名查询提供商的数据，例如：

```properties
淘宝IP地址库， http://ip.taobao.com/
GeoIP， https://geoip.com/
纯真IP库， http://www.cz88.net/

Tip : 如果一下开放API或者数据都不给开放数据时ip2region将停止数据的更新服务
```

# 2、IP Get Location Info

IP 提供了地理位置的信息。IPv4使用32位（4字节）地址，关于位置信息，一般由 `国家|区域|省份|城市|ISP`组成，如：

```javascript
// ip地址|国家|区域|省份|城市|ISP
0.0.0.0|0|0|0|内网IP|内网IP
0.0.0.1|0|0|0|内网IP|内网IP

1.0.15.255|中国|0|广东省|广州市|电信

255.255.255.255|0|0|0|内网IP|内网IP
```



