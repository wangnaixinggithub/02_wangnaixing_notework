# MyBatisPlus-雪花算法-分布式ID

# 1、Need Config

- one way

```yaml
mybatis-plus:
  global-config:
    db-config:
      id-type: auto
```

- two way 

```java
 @TableId(value = "id",type = IdType.AUTO)
  private Long id;
```

