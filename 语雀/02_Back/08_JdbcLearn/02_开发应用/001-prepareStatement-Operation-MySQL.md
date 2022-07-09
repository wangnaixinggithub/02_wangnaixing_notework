# prepareStatement-Operation-MySQL



```xml
        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version>
        </dependency>
```

```properties
#resources\jdbc.properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/mall?useUnicode=true&characterEncoding=utf-8&useSSL=false&allowPublicKeyRetrieval=true
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
```

# 1、JDBCUtils

```java
package com.wnx.mall.tiny.utils;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCUtils {

   private static  String  URL;
   private static  String  USERNAME;
   private static  String  PASSWORD;
   private static  String  DRIVERCLASSNAME;



	static {
		Properties properties= new Properties();
		InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");

		try {
			properties.load(is);
			URL = properties.getProperty("jdbc.url");
			USERNAME = properties.getProperty("jdbc.username");
			PASSWORD = properties.getProperty("jdbc.password");
			DRIVERCLASSNAME = properties.getProperty("jdbc.driverClassName");
		} catch (IOException e) {
			e.printStackTrace();
		}

	}


	/**
	 * 加载驱动，并建立数据库连接
	 * @return 连接
	 * @throws SQLException SQL异常
	 * @throws ClassNotFoundException 类找不到异常
	 */
	public static Connection getConnection() throws SQLException,
				ClassNotFoundException {
			Class.forName(DRIVERCLASSNAME);
			return DriverManager.getConnection(URL, USERNAME,PASSWORD);

		}
    
		/**
		 * 释放数据库连接
		 * @param stmt 预编译陈述对象
		 * @param conn 数据库连接对象
		 */
	public static void release(Statement stmt, Connection conn) {
			if (stmt != null) {
				try {
					stmt.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
				stmt = null;
			}
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
				conn = null;
			}
		}

		/**
		 * 释放数据库连接
		 * @param rs 结果集对象
		 * @param stmt 预编译陈述对象
		 * @param conn 数据库连接对象
		 */
	     public static void release(ResultSet rs, Statement stmt, 
	     		Connection conn){
			if (rs != null) {
				try {
					rs.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
				rs = null;
			}
			release(stmt, conn);
		}
}

```

# 2、Your Business

```java
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
public class PmsBrandMapperImpl implements PmsBrandMapper {

    public List<PmsBrand> findAll() {
        
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;
        
        String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory FROM pms_brand";

        ArrayList<PmsBrand> list = new ArrayList<>();
        
        try {

            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);
            rs = stmt.executeQuery();


            while (rs.next()) {
                PmsBrand brand = PmsBrand.builder()
                        .id(rs.getLong("id"))
                        .name(rs.getString("name"))
                        .firstLetter(rs.getString("firstLetter"))
                        .sort(rs.getInt("sort"))
                        .factoryStatus(rs.getInt("factoryStatus"))
                        .showStatus(rs.getInt("factoryStatus"))
                        .productCount(rs.getInt("productCount"))
                        .productCommentCount(rs.getInt("productCommentCount"))
                        .logo(rs.getString("logo"))
                        .bigPic(rs.getString("bigPic"))
                        .brandStory(rs.getString("brandStory")).build();

                list.add(brand);
            }
            
            return list;
            
        } catch (Exception e) {
            
            e.printStackTrace();
        } finally {
            
            JDBCUtils.release(rs, stmt, conn);
        }
        return null;
    }

    public void insert(PmsBrand pmsBrand) {
        
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;

        String sql = "INSERT INTO pms_brand ( name, first_letter, sort, factory_status, show_status, product_count, product_comment_count, logo, big_pic, brand_story )\n" +
                "        VALUES ( ?,  ?,  ?,  ?,  ?,  ?,  ?,  ?, ?, ? )";

        try {

            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);

            stmt.setString(1,pmsBrand.getName());
            stmt.setString(2, pmsBrand.getFirstLetter());
            stmt.setInt(3, pmsBrand.getSort());
            stmt.setInt(4, pmsBrand.getShowStatus());
            stmt.setInt(5, pmsBrand.getFactoryStatus());
            stmt.setInt(6, pmsBrand.getProductCount());
            stmt.setInt(7, pmsBrand.getProductCommentCount());
            stmt.setString(8, pmsBrand.getLogo());
            stmt.setString(9, pmsBrand.getBigPic());
            stmt.setString(10, pmsBrand.getBrandStory());
            int count = stmt.executeUpdate();

        } catch (Exception e) {
            e.printStackTrace();
            
        }finally {
            
            JDBCUtils.release(rs, stmt, conn);
        }


    }

    public void deleteById(Long id) {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;
        
        String sql = " DELETE FROM pms_brand WHERE id= ?";

        try {
            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);
            stmt.setLong(1,id);
            int count = stmt.executeUpdate();

        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.release(rs, stmt, conn);
        }


    }

    public void update(PmsBrand pmsBrand) {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;

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


        try {

            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);

            stmt.setString(1,pmsBrand.getName());
            stmt.setString(2, pmsBrand.getFirstLetter());
            stmt.setInt(3, pmsBrand.getSort());
            stmt.setInt(4, pmsBrand.getShowStatus());
            stmt.setInt(5, pmsBrand.getFactoryStatus());
            stmt.setInt(6, pmsBrand.getProductCount());
            stmt.setInt(7, pmsBrand.getProductCommentCount());
            stmt.setString(8, pmsBrand.getLogo());
            stmt.setString(9, pmsBrand.getBigPic());
            stmt.setString(10, pmsBrand.getBrandStory());
            stmt.setLong(11, pmsBrand.getId());

            int count = stmt.executeUpdate();

        } catch (Exception e) {
            
            e.printStackTrace();
            
        }finally {
            
            JDBCUtils.release(rs, stmt, conn);
        }


    }

    public PmsBrand findById(Long id) {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;

        String sql = " SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand WHERE id= ?";

        try {

            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);
            stmt.setLong(1,id);
            rs = stmt.executeQuery();


            while (rs.next()) {
                return PmsBrand.builder()
                        .name(rs.getString("name"))
                        .firstLetter(rs.getString("firstLetter"))
                        .sort(rs.getInt("sort"))
                        .factoryStatus(rs.getInt("factoryStatus"))
                        .showStatus(rs.getInt("factoryStatus"))
                        .productCount(rs.getInt("productCount"))
                        .productCommentCount(rs.getInt("productCommentCount"))
                        .logo(rs.getString("logo"))
                        .bigPic(rs.getString("bigPic"))
                        .brandStory(rs.getString("brandStory")).build();
            }

        } catch (Exception e) {
            
            e.printStackTrace();
            
        } finally {
            
            JDBCUtils.release(rs, stmt, conn);
        }
        
        return null;
    }

    public long count() {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;


        String sql = "SELECT COUNT(1) FROM pms_brand";
        try {

            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);
            rs = stmt.executeQuery();


            while (rs.next()) {
                return rs.getInt(1);
            }

        } catch (Exception e) {
            
            e.printStackTrace();
            
        } finally {
            
            JDBCUtils.release(rs, stmt, conn);
        }
        
        return 0;
    }

    public List<PmsBrand> findByPage(Integer pageNo, Integer pageSize) {
        Connection conn = null;
        PreparedStatement stmt = null;
        ResultSet rs = null;
        ArrayList<PmsBrand> list = new ArrayList<>();

        Integer index = (pageNo - 1) * pageSize;
        String sql = "SELECT id,name,first_letter firstLetter,sort,factory_status factoryStatus,show_status showStatus,product_count productCount,product_comment_count productCommentCount,logo,big_pic bigPic,brand_story brandStory  FROM pms_brand LIMIT ?,?";

        try {

            conn = JDBCUtils.getConnection();
            stmt = conn.prepareStatement(sql);
            stmt.setInt(1,index);
            stmt.setInt(2,pageSize);

            rs = stmt.executeQuery();

            while (rs.next()) {
                PmsBrand brand = PmsBrand.builder()
                        .id(rs.getLong("id"))
                        .name(rs.getString("name"))
                        .firstLetter(rs.getString("firstLetter"))
                        .sort(rs.getInt("sort"))
                        .factoryStatus(rs.getInt("factoryStatus"))
                        .showStatus(rs.getInt("factoryStatus"))
                        .productCount(rs.getInt("productCount"))
                        .productCommentCount(rs.getInt("productCommentCount"))
                        .logo(rs.getString("logo"))
                        .bigPic(rs.getString("bigPic"))
                        .brandStory(rs.getString("brandStory")).build();

                list.add(brand);
            }
            
            return list;
        } catch (Exception e) {
            
            e.printStackTrace();
            
        } finally {
            
            JDBCUtils.release(rs, stmt, conn);
        }
        return null;
    }
}

```



