# Use-Stream-Filter-Group-List

- 数据库中存在两条一模一样的记录（多条），重点在于我用一个字段进行标识`ZDBS` 我们在处理业务的时候，可以把他们都查询出来，然后通过流，根据此字段进行过滤就可以得到两类数据的集合了。

```java
    @Test
    public void stream_filter_group_List(){
        Map<String,Object> dataModel1 = new HashMap<>();
        Map<String,Object> dataModel2 = new HashMap<>();
        Map<String,Object> dataModel3 = new HashMap<>();
        Map<String,Object> dataModel4 = new HashMap<>();
        Map<String,Object> dataModel5 = new HashMap<>();
        Map<String,Object> dataModel6 = new HashMap<>();
        Map<String,Object> dataModel7 = new HashMap<>();
        Map<String,Object> dataModel8 = new HashMap<>();
        Map<String,Object> dataModel9 = new HashMap<>();
        Map<String,Object> dataModel10 = new HashMap<>();


        dataModel1.put("userName","张三");
        dataModel2.put("userName","张三");
        dataModel1.put("ZDBS","电厂侧");
        dataModel2.put("ZDBS","上报中调");


        dataModel3.put("userName","李四");
        dataModel4.put("userName","李四");
        dataModel3.put("ZDBS","电厂侧");
        dataModel4.put("ZDBS","上报中调");

        dataModel5.put("userName","王五");
        dataModel6.put("userName","王五");
        dataModel5.put("ZDBS","电厂侧");
        dataModel6.put("ZDBS","上报中调");

        dataModel7.put("userName","赵六");
        dataModel8.put("userName","赵六");
        dataModel7.put("ZDBS","电厂侧");
        dataModel8.put("ZDBS","上报中调");

        dataModel9.put("userName","燕七");
        dataModel10.put("userName","燕七");
        dataModel9.put("ZDBS","电厂侧");
        dataModel10.put("ZDBS","上报中调");

        List<Map<String, Object>> dbList = Lists.newArrayList(
                dataModel1, dataModel2, dataModel3, dataModel4, dataModel5,
                dataModel6, dataModel7, dataModel8, dataModel9, dataModel10);


        Assertions.assertEquals(10,dbList.size());

        List<Map<String, Object>> dcData = dbList.stream()
                .filter(item -> StringUtils.equals((String) item.get("ZDBS"), "电厂侧"))
                .collect(Collectors.toList());

        List<Map<String, Object>> zdData = dbList.stream()
                .filter(item -> StringUtils.equals((String) item.get("ZDBS"), "上报中调"))
                .collect(Collectors.toList());

        Assertions.assertEquals(5,dcData.size());
        Assertions.assertEquals(5,zdData.size());
    }

```

