# MySQL-DataType

- MySQL中的数据类型

```mysql
create  table if not exists t_data_type(
    number_type tinyint,
    number_type2 smallint,
    number_type3 mediumint,
    number_type4 int,
    number_type5 bigint,
    number_type6 float,
    number_type7 double,
    number_type8 decimal,

    string_type char,
    string_type2 varchar(255),
    string_type3 tinyblob,
    string_type4 tinytext,
    string_type5 blob,
    string_type6 text,
    string_type7 mediumblob,
    string_type8 mediumtext,
    string_type9 longblob,
    string_type10 longtext,

    date_type date,
    date_type2 time,
    date_type3 year,
    date_type4 datetime,
    date_type5 timestamp

)
```

# 1、Number Type

![image-20220702100240080](013-MySQL-DataType.assets/image-20220702100240080.png)

# 2、String Type

![image-20220702100302236](013-MySQL-DataType.assets/image-20220702100302236.png)

# 3、Date Type

![image-20220702100321419](013-MySQL-DataType.assets/image-20220702100321419.png)