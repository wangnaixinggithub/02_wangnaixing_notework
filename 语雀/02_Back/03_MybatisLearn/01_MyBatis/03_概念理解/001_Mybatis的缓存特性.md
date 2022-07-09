# MyBatis的缓存特性

## 1、一级缓存

1、同一个SqlSession如果执行相同查询SQL，第二次会在缓存拿而不走数据库查询

```java
    @Test
    public void OneCacheTest01(){
            SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
            EmpMapper  empMapper = sqlSession.getMapper(EmpMapper.class);
            EmpMapper  empMapper2 = sqlSession.getMapper(EmpMapper.class);
            Emp emp =   empMapper.findById(2);
            Emp emp1 =   empMapper2.findById(2);
    }

```

2、如果中间增删改操作，会导致一级缓存失效

```java
    @Test
    public void OneCacheTest02(){ 
        SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
        EmpMapper  empMapper = sqlSession.getMapper(EmpMapper.class);
        EmpMapper  empMapper2 = sqlSession.getMapper(EmpMapper.class);
        Emp emp =   empMapper.findById(2);
        empMapper.deleteById(3);
        Emp emp1 =   empMapper2.findById(2);
    }

```

3、可以手动让一级缓存失效

```java

    @Test
    public void OneCacheTest03(){
        SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
        EmpMapper  empMapper = sqlSession.getMapper(EmpMapper.class);
        EmpMapper  empMapper2 = sqlSession.getMapper(EmpMapper.class);
        Emp emp =   empMapper.findById(2);
        sqlSession.clearCache();
        Emp emp1 =   empMapper2.findById(2);
    }

```

4、每一个SqlSession都有自己独立的缓存块，不干预彼此

```java

    @Test
    public void OneCacheTest04(){
        SqlSession sqlSession = SqlSessionFactoryUtil.openSession();
        EmpMapper  empMapper = sqlSession.getMapper(EmpMapper.class);
        SqlSession sqlSession2 = SqlSessionFactoryUtil.openSession();
        EmpMapper  empMapper2 = sqlSession2.getMapper(EmpMapper.class);
        Emp emp =   empMapper.findById(2);
        Emp emp1 =   empMapper2.findById(2);
    }


```

## 2、二级缓存

> 基于SqlSessionFactory级别的，必须SqlSessionFactory为同一个！保证控制台打印命中缓存的机率在如下代码情况下，越来越高。

```java
    @SneakyThrows
    @Test
    public void TwoCacheTest01(){
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
        SqlSession sqlSession1 = sqlSessionFactory.openSession(true);
        SqlSession sqlSession2 = sqlSessionFactory.openSession(true);
        SqlSession sqlSession3 = sqlSessionFactory.openSession(true);
        SqlSession sqlSession4 = sqlSessionFactory.openSession(true);
        EmpMapper mapper1 = sqlSession1.getMapper(EmpMapper.class);
        EmpMapper mapper2 = sqlSession2.getMapper(EmpMapper.class);
        EmpMapper mapper3 = sqlSession3.getMapper(EmpMapper.class);
        EmpMapper mapper4 = sqlSession4.getMapper(EmpMapper.class);
        mapper1.findById(1);
        sqlSession1.close();
        mapper2.findById(1);
        sqlSession2.close();
        mapper3.findById(1);
        sqlSession3.close();
        mapper4.findById(1);
        sqlSession4.close();
    }
```

```dart
DEBUG 03-02 21:57:24,941 Cache Hit Ratio [com.wnx.mybatis.mapper.EmpMapper]: 0.0  (LoggingCache.java:60) 
DEBUG 03-02 21:57:25,478 ==>  Preparing: select * from tb_emp where eid = ?  (BaseJdbcLogger.java:137) 
DEBUG 03-02 21:57:25,500 ==> Parameters: 1(Integer)  (BaseJdbcLogger.java:137) 
DEBUG 03-02 21:57:25,514 <==      Total: 1  (BaseJdbcLogger.java:137) 
DEBUG 03-02 21:57:25,521 Cache Hit Ratio [com.wnx.mybatis.mapper.EmpMapper]: 0.5  (LoggingCache.java:60) 
DEBUG 03-02 21:57:25,521 Cache Hit Ratio [com.wnx.mybatis.mapper.EmpMapper]: 0.6666666666666666  (LoggingCache.java:60) 
DEBUG 03-02 21:57:25,521 Cache Hit Ratio [com.wnx.mybatis.mapper.EmpMapper]: 0.75  (LoggingCache.java:60) 

进程已结束，退出代码为 0

```

