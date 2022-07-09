# MySQL-DML-show create table 

可查看创建表的定义SQL。

```sql
show create table t_data_type
```

可以得到如下结果：

```SAS
CREATE TABLE `t_data_type` (
  `number_type` tinyint DEFAULT NULL,
  `number_type2` smallint DEFAULT NULL,
  `number_type3` mediumint DEFAULT NULL,
  `number_type4` int DEFAULT NULL,
  `number_type5` bigint DEFAULT NULL,
  `number_type6` float DEFAULT NULL,
  `number_type7` double DEFAULT NULL,
  `number_type8` decimal(10,0) DEFAULT NULL,
  `string_type` char(1) DEFAULT NULL,
  `string_type2` varchar(255) DEFAULT NULL,
  `string_type3` tinyblob,
  `string_type4` tinytext,
  `string_type5` blob,
  `string_type6` text,
  `string_type7` mediumblob,
  `string_type8` mediumtext,
  `string_type9` longblob,
  `string_type10` longtext,
  `date_type` date DEFAULT NULL,
  `date_type2` time DEFAULT NULL,
  `date_type3` year DEFAULT NULL,
  `date_type4` datetime DEFAULT NULL,
  `date_type5` timestamp NULL DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3
```

