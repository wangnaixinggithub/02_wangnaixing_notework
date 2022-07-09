# Use-getFinalNum()-onlyNumber

- 我们希望，SQDBH字段能够根据表记录进行一个自增。并记录创建的时间。以及单位。

```java
   @Test
    public void get_Next_SQDBH(){
        //假定：tNum从数据库中查询得到  假定areaCode从数据库中获取
        String preSQDBH = "佛山供电局-2022050004";
        String areaCode = "福山供电局";

        //1.截取preSQDBH得到后四位 0004
        tNum = tNum.substring(12);
        Assertions.assertEquals("0004",preSQDBH);

        //2.拼接当前时间作为SQDBH前缀
        Date currentTime = new Date();
        SimpleDateFormat formatter = new SimpleDateFormat("yyyyMM");
        String preHB = formatter.format(currentTime) ;

        preHB = areaCode + "-" + preSQDBH;
  		Assertions.assertEquals("福山供电局-",preHB);
        
        //3.根据上一个编码得到一个唯一的编号。
        String SQDBH = getFinalNum(preHB,tNum);
        
        
        Assertions.assertEquals("福山供电局-2022050005",SQDBH);


    }

```

# 1、Algorithm

- 算法逻辑很简单,转换preHB得到Interge,加+1

```java
   private String getFinalNum(String preHB, String tNum) {
        String bhFlag;
        if (!"".equals(tNum)) {

            int flag = Integer.parseInt(tNum);

            //强制类型转换后 0004 => 4
            Assertions.assertEquals(4,flag);


            if (flag < 9) {

                bhFlag = preHB + "000" + (flag + 1);

            } else if (flag < 99) {

                bhFlag = preHB + "00" + (flag + 1);

            } else if (flag < 999) {

                bhFlag = preHB + "0" + (flag + 1);

            } else {

                bhFlag = preHB + (flag + 1);
            }
        } else {

            bhFlag = preHB + "0001";
        }

        return bhFlag;
    }
```

