# MyBatisPlus-Logic-Delete

# 1、@TableLogic

- 默认未删除是1，删除是0

```java
public class Common implements Serializable {

    private static final long serialVersionUID=1L;

    private Long id;

    private String name;

    @TableLogic
    private Integer deleted;

}

```

- 可以修改默认情况：未删除为null,删除为1.

```java
public class Null1 implements Serializable {

    private static final long serialVersionUID=1L;

    private Long id;

    private String name;

    @TableLogic(delval = "null", value = "1")
    private Integer deleted;


}

```

- 默认未删除为当前时间,删除为null.

```java
public class Null2 implements Serializable {

    private static final long serialVersionUID=1L;

    private Long id;

    private String name;

    @TableLogic(delval = "now()", value = "null")
    private Date delTime;


}

```

