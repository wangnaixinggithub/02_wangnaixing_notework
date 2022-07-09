# SpringBoot-Operation-H2-DataBase

> H2 基于内存的数据库，每次重启后数据库都会被清空，这样每次的数据库都是新的，不会造成干扰。

# 1、Need Config

```xml
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.4.5</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>

		<!--h2数据库-->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>
```

```yaml
spring:
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: none
  datasource:
    schema: classpath*:schema.sql
    data: classpath*:data.sql
    platform: h2

```

# 2、data.sql  schema.sql

- 在resources目录下分别创建`data.sql`文件以及`schema.sql`文件一个存放数据库保存数据，一个为建表数据。

```sql

DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
                                            (1, 'Jone', 18, 'test1@baomidou.com'),
                                            (2, 'Jack', 20, 'test2@baomidou.com'),
                                            (3, 'Tom', 28, 'test3@baomidou.com'),
                                            (4, 'Sandy', 21, 'test4@baomidou.com'),
                                            (5, 'Billie', 24, 'test5@baomidou.com');

```

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
    id BIGINT(20) NOT NULL COMMENT '主键ID',
    name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
    age INT(11) NULL DEFAULT NULL COMMENT '年龄',
    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY (id)
);
```







