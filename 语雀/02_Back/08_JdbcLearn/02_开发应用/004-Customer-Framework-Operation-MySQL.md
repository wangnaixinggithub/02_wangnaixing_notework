# Customer-Framework-Operation-MySQL

# 1、BaseDao

```java
public abstract class BaseMapper<T> {
    private final String DRIVER = "com.mysql.cj.jdbc.Driver";
    private final String URL = "jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&useSSL=false";
    private final String USER = "root";
    private final String PWD = "root" ;


    private Connection conn ;
    private PreparedStatement psmt ;
    private ResultSet rs ;
    private Class entityClass ;

    public BaseMapper(){
        //1.获取程序运行时，T实际指代的类型。
        Type genericType = getClass().getGenericSuperclass();
        Type[] actualTypeArguments = ((ParameterizedType) genericType).getActualTypeArguments();
        Type actualType = actualTypeArguments[0];

        // 2.构造该T的类对象
        try {
            entityClass = Class.forName(actualType.getTypeName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取数据库连接
     * @return 一个数据库连接
     */
    private Connection getConn(){
        try {
            Class.forName(DRIVER);
            return DriverManager.getConnection(URL,USER,PWD);
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }
        return null ;
    }


    /**
     * 释放数据库相关资源
     * @param rs 结果集对象
     * @param psmt 预编译陈述对象
     * @param conn 数据库连接
     */
    private void close(ResultSet rs, PreparedStatement psmt, Connection conn){
        try {
            if (rs != null) {
                rs.close();
            }
            if(psmt!=null){
                psmt.close();
            }
            if(conn!=null && !conn.isClosed()){
                conn.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    /***
     * 给预处理陈述对象的占位符?设置参数
     * @param psmt 预处理陈述对象
     * @param params 参数
     * @throws SQLException SQL异常
     */
    private void setParams(PreparedStatement psmt , Object... params) throws SQLException {
        if(params!=null && params.length>0){
            for (int i = 0; i < params.length; i++) {
                psmt.setObject(i+1,params[i]);
            }
        }
    }

    /***
     * 执行数据库更新操作
     * @param sql 数据库操作SQL
     * @param params 更新参数
     * @return 受到影响的行数
     */
    protected int executeUpdate(String sql , Object... params){
        boolean insertFlag = false ;

        try {
            conn = getConn();
            psmt = conn.prepareStatement(sql,Statement.RETURN_GENERATED_KEYS);
            setParams(psmt,params);
            int count = psmt.executeUpdate() ;
            rs = psmt.getGeneratedKeys();

            if(rs.next()){
                return ((Long)rs.getLong(1)).intValue();
            }
            return count ;
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            close(rs,psmt,conn);
        }
        return 0;
    }

    /**
     * 通过反射技术给obj对象的property属性赋propertyValue值
     * @param obj 对象model
     * @param property model字段
     * @param propertyValue model值
     */
    private void setValue(Object obj ,  String property , Object propertyValue){
        //1.获取对象的类
        Class clazz = obj.getClass();
        try {
            //2.根据成员变量名获取成员变量
            Field field = clazz.getDeclaredField(property);

            //3.反射赋值
            if(field!=null){
                field.setAccessible(true);
                field.set(obj,propertyValue);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 执行复杂查询，返回例如统计结果
     * @param sql 数据库操作SQL
     * @param params 更新参数
     * @return 对象数组
     */
    protected Object[] executeComplexQuery(String sql , Object... params){
        try {
            //1.执行查询
            conn = getConn() ;
            psmt = conn.prepareStatement(sql);
            setParams(psmt,params);
            rs = psmt.executeQuery();

            //2.构造列容器
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            Object[] columnValueArr = new Object[columnCount];

            //3.查询结果放入列容器
            if(rs.next()){
                for(int i = 0 ; i<columnCount;i++){
                    Object columnValue = rs.getObject(i+1);
                    columnValueArr[i]=columnValue;
                }
                //4.返回列容器
                return columnValueArr ;
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            close(rs,psmt,conn);
        }
        return null ;
    }

    /**
     * 执行查询，返回单个实体对象
     * @param sql 数据库操作SQL
     * @param params 查询参数
     * @return 实体Model
     */
    protected T load(String sql , Object... params){
        try {
            //1.查询操作
            conn = getConn() ;
            psmt = conn.prepareStatement(sql);
            setParams(psmt,params);
            rs = psmt.executeQuery();

            //2.对象赋值
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            if(rs.next()){
                T entity = (T)entityClass.newInstance();

                for(int i = 0 ; i<columnCount;i++){
                    String columnName = rsmd.getColumnName(i+1);

                    //3.驼峰命名的转化
                    while (columnName.contains("_")) {
                        int index = columnName.indexOf("_");
                        String target = columnName.substring(index + 1, index + 2);
                        columnName = columnName.replace(("_" + target), target.toUpperCase());
                    }

                    Object columnValue = rs.getObject(i+1);
                    setValue(entity,columnName,columnValue);
                }
                //3.返回对象
                return entity ;
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            close(rs,psmt,conn);
        }
        return null ;
    }


    /***
     * 执行查询，返回List
     * @param sql  数据库操作SQL
     * @param params 查询参数
     * @return 实体集合List
     */
    protected List<T> executeQuery(String sql , Object... params){
        List<T> list = new ArrayList<>();
        try {
            //1.执行查询
            conn = getConn() ;
            psmt = conn.prepareStatement(sql);
            setParams(psmt,params);
            rs = psmt.executeQuery();

            //2.对象赋值
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            while(rs.next()){
                T entity = (T)entityClass.newInstance();
                for(int i = 0 ; i<columnCount;i++){

                    String columnName = rsmd.getColumnName(i+1);

                    //3.驼峰命名的转化
                    while (columnName.contains("_")) {
                        int index = columnName.indexOf("_");
                        String target = columnName.substring(index + 1, index + 2);
                        columnName = columnName.replace(("_" + target), target.toUpperCase());
                    }

                    Object columnValue = rs.getObject(i+1);
                    setValue(entity,columnName,columnValue);
                }

                //4.添加到集合
                list.add(entity);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            close(rs,psmt,conn);
        }
        //5.返回集合
        return list ;
    }


}

```

# 2、Your Business

```java
package com.wnx.mall.tiny.mapper;

import com.wnx.mall.tiny.model.PmsBrand;

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
     *分页查询
     * @param pageNo 当前页
     * @param pageSize 每页大小
     * @return 品牌集合
     */
    List<PmsBrand> findByPage(Integer pageNo, Integer pageSize);
}

```

```java
package com.wnx.mall.tiny.mapper.impl;
import com.wnx.mall.tiny.mapper.BaseMapper;
import com.wnx.mall.tiny.mapper.PmsBrandMapper;
import com.wnx.mall.tiny.model.PmsBrand;
import java.util.List;

public class PmsBrandMapperImpl extends BaseMapper<PmsBrand> implements PmsBrandMapper {


    public List<PmsBrand> findAll() {
        String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory FROM pms_brand";
        return executeQuery(sql);

    }

    public void insert(PmsBrand pmsBrand) {
        String sql = "INSERT INTO pms_brand ( name, first_letter, sort, factory_status, show_status, product_count, product_comment_count, logo, big_pic, brand_story )\n" +
                "        VALUES ( ?,  ?,  ?,  ?,  ?,  ?,  ?,  ?, ?, ?)";

            executeUpdate(sql,
                    pmsBrand.getName(),
                    pmsBrand.getFirstLetter(),
                    pmsBrand.getSort(),
                    pmsBrand.getFactoryStatus(),
                    pmsBrand.getShowStatus(),
                    pmsBrand.getProductCount(),
                    pmsBrand.getProductCommentCount(),
                    pmsBrand.getLogo(),
                    pmsBrand.getBigPic(),
                    pmsBrand.getBrandStory());


    }

    public void deleteById(Long id) {
        String sql = " DELETE FROM pms_brand WHERE id= ?";
        executeUpdate(sql,id);
    }

    public void update(PmsBrand pmsBrand) {
        String sql = "  UPDATE pms_brand SET\n" +
                "                             name= ?,\n" +
                "                             first_letter = ?,\n" +
                "                             sort = ?,\n" +
                "                             factory_status = ?,\n" +
                "                             show_status = ? ,\n" +
                "                             product_count = ? ,\n" +
                "                             product_comment_count = ?,\n" +
                "                             logo = ?,\n" +
                "                             big_pic= ?,\n" +
                "                             brand_story= ?\n" +
                "                             WHERE id= ? ";

            executeUpdate(sql,
                    pmsBrand.getName(),
                    pmsBrand.getFirstLetter(),
                    pmsBrand.getSort(),
                    pmsBrand.getFactoryStatus(),
                    pmsBrand.getShowStatus(),
                    pmsBrand.getProductCount(),
                    pmsBrand.getProductCommentCount(),
                    pmsBrand.getLogo(),
                    pmsBrand.getBigPic(),
                    pmsBrand.getBrandStory(),
                    pmsBrand.getId());

    }

    public PmsBrand findById(Long id) {
        String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand WHERE id= ?";
        return load(sql,id);

    }

    public long count() {
        String sql = "SELECT COUNT(1) FROM pms_brand";
        return  (long) executeComplexQuery(sql)[0];

    }

    public List<PmsBrand> findByPage(Integer pageNo, Integer pageSize) {
        Integer index = (pageNo - 1) * pageSize;
        String sql = "SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand LIMIT ?,?";
        return executeQuery(sql, index, pageSize);
    }

    @Override
    public long count(String keyword) {
         keyword = "%" + keyword + "%";
        String sql = "SELECT COUNT(1) FROM pms_brand where name like ?";
        return  (long) executeComplexQuery(sql,keyword)[0];
    }

    @Override
    public List<PmsBrand> findByPage(Integer pageNo, int pageSize, String keyword) {
        keyword = "%" + keyword + "%";
        Integer index = (pageNo - 1) * pageSize;
        String sql = "SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand where name like  ? LIMIT ?,? ";
        return executeQuery(sql,keyword,index,pageSize);
    }
}

```



## 