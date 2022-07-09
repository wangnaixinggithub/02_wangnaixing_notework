# 1、JavaBean-Boolean-Property

涉及到布尔类型的成员变量。建议用原字段，比如`deleted`。而不要使用`isDeleted` 用`success`,而不要使用`isSuccess`

# 1、deleted

```java
public class User implements Serializable {
    private Long id;
    private String username;
    private Boolean deleted;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Boolean getDeleted() {
        return deleted;
    }

    public void setDeleted(Boolean deleted) {
        this.deleted = deleted;
    }
}

```

# 2、success

```java
public class User implements Serializable {
    private Long id;
    private String username;
    private Boolean success;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Boolean getSuccess() {
        return success;
    }

    public void setSuccess(Boolean success) {
        this.success = success;
    }
}
```

