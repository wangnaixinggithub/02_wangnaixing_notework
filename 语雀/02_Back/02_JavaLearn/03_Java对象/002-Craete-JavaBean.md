# Craete-JavaBean

# 1、JavaBeans Specitifcaiton

```java
package com.wnx.model;

import java.io.Serializable;

 //1. implement    Serializable
public class User implements Serializable {
                                   
    //2. provider NoArgsConstructor And  AllArgsConstructor
    public User() {
        
    }

    public User(Long userId, String userName, Double height, Boolean graduate) {
        this.userId = userId;
        this.userName = userName;
        this.height = height;
        this.graduate = graduate;
    }

    //3. property is Private
    private Long userId;
    private String userName;
    private Double height;
    private Boolean graduate;
    
    //4. provider Getter And Setter Method

    public Long getUserId() {
        return userId;
    }

    public void setUserId(Long userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public Double getHeight() {
        return height;
    }

    public void setHeight(Double height) {
        this.height = height;
    }

    public Boolean getGraduate() {
        return graduate;
    }

    public void setGraduate(Boolean graduate) {
        this.graduate = graduate;
    }

    
}

```

