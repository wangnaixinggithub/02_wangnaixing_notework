# Dynamic_Join_Update_SQL

- 假如传入的为Map，可以进行动态拼接`UPDATE`语句。

```java
    @Test
    public void dynamic_Join_Test(){
        Map<String,Object> dataModel = new HashMap<>();
        dataModel.put("id","a7336fe2db0811ec9a6dea539f65ac17");
        dataModel.put("nickname","王乃醒");
        dataModel.put("password","123456");
        dataModel.put("sex","男");
        dataModel.put("email","827376239@qq.com");
        String sql = "UPDATE t_user SET  ";
        List<String> updateParam = new ArrayList<>();

        dataModel.forEach((k,v) -> updateParam.add(k+ " = '" + v + "'"));
        sql = sql + String.join(", ", updateParam) + " where id = '" + dataModel.get("id") + "'";

   log.info("{}", sql);
    //UPDATE t_user SET  password = '123456', sex = '男', nickname = '王乃醒', id = 'a7336fe2db0811ec9a6dea539f65ac17', email = '827376239@qq.com' where id = 'a7336fe2db0811ec9a6dea539f65ac17'

    }
```

