# MyBatis-Use-SqlSessionFactoryUtils

# 1、SqlSessionFactoryUtils

- 可以使用单例模式，创建会话工厂。

```java
package com.wnx.mall.tiny.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;

public class SqlSessionFactoryUtils {
    
    private static SqlSessionFactory SQL_SESSION_FACTORY = null;

    /**
     * 获取会话工厂实例
     * @return 会话工厂
     */
    public static SqlSessionFactory getInstance(){
        if (SQL_SESSION_FACTORY == null){
            try {
                SQL_SESSION_FACTORY =  new SqlSessionFactoryBuilder()
                        .build(Resources.getResourceAsStream("mybatis-config.xml"));
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
        return SQL_SESSION_FACTORY;
    }
}

```

# 2、In Your Business

```java
package com.wnx.mall.tiny.mapper;

import com.wnx.mall.tiny.model.PmsBrand;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.session.RowBounds;

import java.util.List;

public interface PmsBrandMapper {
    /***
     * 查询全部品牌
     * @return 品牌集合
     */
    List<PmsBrand> findAll();

    /**
     * 新增品牌信息
     * @param pmsBrand 品牌模型
     */
    void insert(PmsBrand pmsBrand);

    /**
     * 根据ID删除品牌信息
     * @param id ID
     */
    void deleteById(Long id);

    /**
     * 更新品牌信息
     * @param pmsBrand 品牌模型
     */
    void update(PmsBrand pmsBrand);


    /**
     * 根据ID查询品牌信息
     * @return 品牌对象
     */
    PmsBrand findById(Long id);

    /**
     * 查询总记录
     * @return 表总记录数
     */
    long count();


    /**
     *  分页查询
     * @param rowBounds 行界限对象
     * @return 品牌集合
     */
    List<PmsBrand> findByPage(RowBounds rowBounds);

    /**
     * 根据关键字查询总记录
     * @param keyword 查询关键字
     * @return 表总记录数
     */
    long countByKeyWord(@Param("keyword") String keyword);

    /**
     *  根据关键字分页查询
     * @param keyword 查询关键字
     * @param rowBounds 行界限对象
     * @return 品牌集合
     */
    List<PmsBrand> findByPageByKeyWord(@Param("keyword") String keyword, RowBounds rowBounds);
}

```

- 调用接口的方法，MyBatis会自动扫描在`com.wnx.mall.tiny.mapper.PmsBrandMapper.xml`文件，然后通过方法名找到SQL执行。

```java
package com.wnx.mall.tiny.mapper.impl;

import com.wnx.mall.tiny.mapper.PmsBrandMapper;
import com.wnx.mall.tiny.model.PmsBrand;
import com.wnx.mall.tiny.utils.SqlSessionFactoryUtils;
import org.apache.ibatis.session.RowBounds;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;

import java.util.List;

public class PmsBrandMapperImpl implements PmsBrandMapper {
    private SqlSessionFactory factory = SqlSessionFactoryUtils.getInstance();

    @Override
    public List<PmsBrand> findAll() {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        List<PmsBrand> list = mapper.findAll();
        sqlSession.close();
        return list;
    }

    @Override
    public void insert(PmsBrand pmsBrand) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
         mapper.insert(pmsBrand);
        sqlSession.close();
    }

    @Override
    public void deleteById(Long id) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        mapper.deleteById(id);
        sqlSession.close();
    }

    @Override
    public void update(PmsBrand pmsBrand) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        mapper.update(pmsBrand);
        sqlSession.close();
    }

    @Override
    public PmsBrand findById(Long id) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        PmsBrand pmsBrand = mapper.findById(id);
        sqlSession.close();
        return pmsBrand;
    }

    @Override
    public long count() {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        Long count = mapper.count();
        sqlSession.close();
        return count;
    }

    @Override
    public List<PmsBrand> findByPage(RowBounds rowBounds) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        List<PmsBrand> brands = mapper.findByPage(rowBounds);
        sqlSession.close();
        return brands;
    }


    @Override
    public long countByKeyWord(String keyword) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        long count = mapper.countByKeyWord(keyword);
        sqlSession.close();
        return count;
    }

    @Override
    public List<PmsBrand> findByPageByKeyWord(String keyword, RowBounds rowBounds) {
        SqlSession sqlSession = factory.openSession();
        PmsBrandMapper mapper = sqlSession.getMapper(PmsBrandMapper.class);
        List<PmsBrand> list = mapper.findByPageByKeyWord(keyword, rowBounds);
        sqlSession.close();
        return list;
    }

}
```

