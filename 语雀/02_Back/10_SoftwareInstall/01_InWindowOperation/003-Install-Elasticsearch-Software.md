# Install-Elasticsearch-Software

# 1、Download

- 下载 Elasticsearch 并进行解压操作。

```properties
下载地址：https://www.elastic.co/cn/downloads/past-releases/elasticsearch-6-2-2
```

# 2、Install

- 解压下载的包

- 安装中文分词插件

```properties
需要在elasticsearch-6.2.2\bin目录下执行以下命令：elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.2.2/elasticsearch-analysis-ik-6.2.2.zip
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211212104629.jpg)

# 3、Start

- 下载Kibana,作为访问Elasticsearch的客户端

```properties
下载地址：https://artifacts.elastic.co/downloads/kibana/kibana-6.2.2-windows-x86_64.zip
```

- 运行bin目录下的kibana.bat，访问 `http://localhost:5601 即可打开Kibana的用户界面` 启动Kibana的用户界面

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211212105113.jpg)