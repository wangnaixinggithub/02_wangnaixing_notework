# Customer-Framework-SSM

# 1、Pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>


    <groupId>com.wnx.mall.tiny</groupId>
    <artifactId>web-my-ssm</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <name>web-my-ssm</name>

    <description>个人仿制的SSM框架</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--servlet-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version>
        </dependency>
        <!--thymeleaf-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf</artifactId>
            <version>3.0.14.RELEASE</version>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                    <!--反射能得到真的方法名-->
                    <compilerArgs>
                        <arg>-parameters</arg>
                    </compilerArgs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

# 2、Spring

> 放置在resources\applicationContext.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<beans>
    <bean id="pmsBrandMapper" class="com.wnx.mall.tiny.mapper.impl.PmsBrandMapperImpl"/>
    <bean id="PmsBrandService" class="com.wnx.mall.tiny.service.impl.PmsBrandServiceImpl">
        <property name="pmsBrandMapper" ref="pmsBrandMapper"/>
    </bean>
    <bean id="brand" class="com.wnx.mall.tiny.controller.PmsBrandController">
        <property name="fruitService" ref="fruitService"/>
    </bean>
</beans>
```

# 3、Web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 配置上下文参数 -->
    <context-param>
        <param-name>view-prefix</param-name>
        <param-value>/</param-value>
    </context-param>d
    <context-param>
        <param-name>view-suffix</param-name>
        <param-value>.html</param-value>
    </context-param>
    <!--Spring配置文件的位置-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>applicationContext.xml</param-value>
    </context-param>
</web-app>
```

# 4、ContextLoaderListener



> 负责 在上下文启动的时候去创建IOC容器,然后将其保存到application作用域

```java
package com.wnx.myssm.mylistner;



import com.wnx.myssm.myioc.BeanFactory;
import com.wnx.myssm.myioc.ClassPathXmlApplicationContext;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;



/**
 * 上下文加载监听器
 */
@WebListener
public class ContextLoaderListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {

        //1.在上下文启动的时候去创建IOC容器,然后将其保存到application作用域
        ServletContext application = sce.getServletContext();
        String path = application.getInitParameter("contextConfigLocation");
        BeanFactory beanFactory = new ClassPathXmlApplicationContext(path);
        application.setAttribute("beanFactory",beanFactory);

    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {

    }
}

```

# 5、DispatcherServlet

```java
package com.wnx.myssm.mysrpingmvc;




import com.wnx.myssm.myioc.BeanFactory;
import com.wnx.myssm.utils.StringUtil;
import lombok.SneakyThrows;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.Method;
import java.lang.reflect.Parameter;

/**
 * 所有*.do的请求会进入到该Servlet!!!
 */
@WebServlet("*.do")
public class DispatcherServlet extends ViewBaseServlet {

    private BeanFactory beanFactory ;

    public DispatcherServlet(){
    }

    public void init() throws ServletException {
        //1.Servlet在构建底层会调用Init()方法，集成ViewBaseServlet,会去调用ViewBaseServlet的Init()
        super.init();
        //2.从application作用域去获取IOC容器
        ServletContext application = getServletContext();
        Object beanFactoryObj = application.getAttribute("beanFactory");
        if(beanFactoryObj!=null){
            beanFactory = (BeanFactory)beanFactoryObj ;
        }else{
            throw new RuntimeException("IOC容器获取失败！");
        }
    }

    @SneakyThrows
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1.获取Bean ID
        String servletPath = request.getServletPath();
        servletPath = servletPath.substring(1);
        int lastDotIndex = servletPath.lastIndexOf(".do") ;
        servletPath = servletPath.substring(0,lastDotIndex);

        //2.根据ID从容器中取出Bean
        Object controllerBeanObj = beanFactory.getBean(servletPath);

        //3.获取方法名
        String operate = request.getParameter("operate");
        if(StringUtil.isEmpty(operate)){ operate = "index" ; }

            Method[] methods = controllerBeanObj.getClass().getDeclaredMethods();
            for (Method method : methods) {
                //4.获取Handler方法对象
                if (operate.equals(method.getName())) {
                    Parameter[] parameters = method.getParameters();
                    Object[] parameterValues = new Object[parameters.length];


                    //5.完成Controller方法参数绑定
                    for (int i = 0; i < parameters.length; i++) {
                        Parameter parameter = parameters[i];
                        String parameterName = parameter.getName();

                        if ("request".equals(parameterName)) {
                            parameterValues[i] = request;
                        } else if ("response".equals(parameterName)) {
                            parameterValues[i] = response;
                        } else if ("session".equals(parameterName)) {
                            parameterValues[i] = request.getSession();
                        } else {
                            String parameterValue = request.getParameter(parameterName);
                            String typeName = parameter.getType().getName();
                            Object parameterObj = parameterValue;

                            if (parameterObj != null) {
                                if ("java.lang.Integer".equals(typeName)) {
                                    parameterObj = Integer.parseInt(parameterValue);
                                }
                            }

                            if (parameterObj != null) {
                                if ("java.lang.Long".equals(typeName)) {
                                    parameterObj = Long.parseLong(parameterValue);
                                }
                            }
                            parameterValues[i] = parameterObj;
                        }
                    }
                    //6.反射执行方法
                    method.setAccessible(true);
                    Object returnObj = method.invoke(controllerBeanObj, parameterValues);


                    //7.视图处理
                    String methodReturnStr = (String) returnObj;
                    if (methodReturnStr.startsWith("redirect:")) {
                        String redirectStr = methodReturnStr.substring("redirect:".length());
                        response.sendRedirect(redirectStr);
                    } else {
                        super.processTemplate(methodReturnStr, request, response);
                    }
                }


            }


    }
}


```

# 6、ViewBaseServlet

```java
package com.wnx.myssm.mysrpingmvc;

import org.thymeleaf.TemplateEngine;
import org.thymeleaf.context.WebContext;
import org.thymeleaf.templatemode.TemplateMode;
import org.thymeleaf.templateresolver.ServletContextTemplateResolver;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ViewBaseServlet extends HttpServlet {

    private TemplateEngine templateEngine;

    @Override
    public void init() throws ServletException {

        // 1.获取ServletContext对象
        ServletContext servletContext = this.getServletContext();

        // 2.创建Thymeleaf解析器对象
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(servletContext);

        // 3.给解析器对象设置参数
        templateResolver.setTemplateMode(TemplateMode.HTML);
        templateResolver.setPrefix(servletContext.getInitParameter("view-prefix"));
        templateResolver.setSuffix(servletContext.getInitParameter("view-suffix"));
        templateResolver.setCacheTTLMs(60000L);
        templateResolver.setCacheable(true);
        templateResolver.setCharacterEncoding("utf-8");

        // 4.创建模板引擎对象
        templateEngine = new TemplateEngine();

        // 5.给模板引擎对象设置模板解析器
        templateEngine.setTemplateResolver(templateResolver);

    }

    protected void processTemplate(String templateName, HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // 1.设置响应体内容类型和字符集
        resp.setContentType("text/html;charset=UTF-8");

        // 2.创建WebContext对象
        WebContext webContext = new WebContext(req, resp, getServletContext());

        // 3.处理模板数据
        templateEngine.process(templateName, webContext, resp.getWriter());
    }
}
```

# 7、ClassPathXmlApplicationContext

- 抽象Bean工厂

```java
package com.wnx.myssm.myioc;

/**
 * Bean构建工厂
 */
public interface BeanFactory {
    Object getBean(String id);
}

```

- 实现：ClassPathXmlApplicationContext

```java
package com.wnx.myssm.myioc;


import com.wnx.myssm.utils.StringUtil;
import lombok.SneakyThrows;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import java.io.InputStream;
import java.lang.reflect.Field;
import java.util.HashMap;
import java.util.Map;

public class ClassPathXmlApplicationContext implements BeanFactory {

    private Map<String,Object> beanMap = new HashMap<>();


    @SneakyThrows
    public ClassPathXmlApplicationContext(){
        this("applicationContext.xml");
    }
    @SneakyThrows
    public ClassPathXmlApplicationContext(String path){
            if(StringUtil.isEmpty(path)){ throw new RuntimeException("IOC容器的配置文件没有指定..."); }

            //1.根据Resource目录下applicationContext.xml，构造文档对象
            InputStream inputStream = getClass().getClassLoader().getResourceAsStream(path);
            DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder() ;
            Document document = documentBuilder.parse(inputStream);

            //2.扫描所有Bean标签
            NodeList beanNodeList = document.getElementsByTagName("bean");
            for(int i = 0 ; i<beanNodeList.getLength() ; i++){
                Node beanNode = beanNodeList.item(i);
                if(beanNode.getNodeType() == Node.ELEMENT_NODE){
                    //1.根据Bean标签，由反射构造对象
                    Element beanElement = (Element)beanNode ;
                    String beanId =  beanElement.getAttribute("id");
                    String className = beanElement.getAttribute("class");
                    Class beanClass = Class.forName(className);
                    Object beanObj = beanClass.newInstance() ;

                    //2.存入Map容器
                    beanMap.put(beanId , beanObj) ;

                }
            }
            //3.组装bean之间的依赖关系
            for(int i = 0 ; i<beanNodeList.getLength() ; i++){
                Node beanNode = beanNodeList.item(i);
                if(beanNode.getNodeType() == Node.ELEMENT_NODE) {
                    Element beanElement = (Element) beanNode;
                    String beanId = beanElement.getAttribute("id");
                    NodeList beanChildNodeList = beanElement.getChildNodes();
                    for (int j = 0; j < beanChildNodeList.getLength() ; j++) {

                        //4.从容器中取出被依赖Bean 注入到被依赖Bean中
                        Node beanChildNode = beanChildNodeList.item(j);
                        if(beanChildNode.getNodeType()==Node.ELEMENT_NODE && "property".equals(beanChildNode.getNodeName())){
                            Element propertyElement = (Element) beanChildNode;
                            String propertyName = propertyElement.getAttribute("name");
                            String propertyRef = propertyElement.getAttribute("ref");
                            Object refObj = beanMap.get(propertyRef);
                            Object beanObj = beanMap.get(beanId);
                            Class beanClazz = beanObj.getClass();
                            Field propertyField = beanClazz.getDeclaredField(propertyName);
                            propertyField.setAccessible(true);
                            propertyField.set(beanObj,refObj);
                        }
                    }
                }
            }

    }

    /**
     * 根据ID获取Bean
     * @param id ID
     * @return Bean
     */
    @Override
    public Object getBean(String id) {
        return beanMap.get(id);
    }
}

```

# 8、CharacterEncodingFilter

```java
package com.wnx.myssm.myfilter;




import com.wnx.myssm.utils.StringUtil;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * 字符编码过滤器 过滤  *.do 的请求
 */
@WebFilter(urlPatterns = {"*.do"},initParams = {@WebInitParam(name = "encoding",value = "UTF-8")})
public class CharacterEncodingFilter implements Filter {

    private String encoding = "UTF-8";

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        String encodingStr = filterConfig.getInitParameter("encoding");
        if(StringUtil.isNotEmpty(encodingStr)){
            encoding = encodingStr ;
        }
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        ((HttpServletRequest)servletRequest).setCharacterEncoding(encoding);
        filterChain.doFilter(servletRequest,servletResponse);

    }

    @Override
    public void destroy() {

    }
}

```

# 9、BaseMapper

```java
package com.wnx.mall.tiny.mapper;

import com.wnx.myssm.mymapper.ConnUtil;

import java.lang.reflect.Field;
import java.lang.reflect.ParameterizedType;
import java.lang.reflect.Type;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

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
        return ConnUtil.getConn();
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

# 10、TransactionManager

```java
	package com.wnx.myssm.mytransaction;



import com.wnx.myssm.mymapper.ConnUtil;

import java.sql.Connection;
import java.sql.SQLException;

public class TransactionManager {

    //开启事务
    public static void beginTrans() throws SQLException {
        ConnUtil.getConn().setAutoCommit(false);
    }

    //提交事务
    public static void commit() throws SQLException {
        Connection conn = ConnUtil.getConn();
        conn.commit();
        ConnUtil.closeConn();
    }

    //回滚事务
    public static void rollback() throws SQLException {
        Connection conn = ConnUtil.getConn();
        conn.rollback();
        ConnUtil.closeConn();
    }
}

```

# 11、ConnUtil

> 使用一个连接Connection才能说事务，一次请求对应一次线程。重写获取数据库连接类

- ConnUtil

```java
package com.wnx.myssm.mymapper;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConnUtil {
    private static ThreadLocal<Connection> threadLocal = new ThreadLocal<>();

    private static final String DRIVER = "com.mysql.cj.jdbc.Driver";
    private static final String URL = "jdbc:mysql://localhost:3306/javaweb?useUnicode=true&characterEncoding=utf-8&useSSL=false&allowPublicKeyRetrieval=true";
    private static final String USER = "root";
    private static final String PWD = "root";

    /**
     * 创建数据库连接
     * @return 返回数据库连接
     */
    private static Connection createConn() {
        try {
            Class.forName(DRIVER);
            return DriverManager.getConnection(URL, USER, PWD);
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     * 获取数据库连接
     * @return 数据库连接
     */
    public static Connection getConn() {
        //1.有则用本地线程的连接，没有就创建一个。
        Connection conn = threadLocal.get();
        if (conn == null) {
            conn = createConn();
            threadLocal.set(conn);
        }
        return threadLocal.get();

    }


    /**
     * 关闭数据库连接
     * @throws SQLException SQL异常
     */
    public static void closeConn() throws SQLException {
        Connection conn = threadLocal.get();
        if(conn==null){
            return ;
        }
        if(!conn.isClosed()){
            conn.close();
            threadLocal.set(null);
        }
    }

}

```

> 一但出现异常，让当前数据库连接马上回滚。

- OpenSessionInViewFilter

```java
package com.wnx.myssm.myfilter;



import com.wnx.myssm.mytransaction.TransactionManager;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;
import java.sql.SQLException;

@WebFilter("*.do")
public class OpenSessionInViewFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        try{
            TransactionManager.beginTrans();
            System.out.println("开启事务....");
            filterChain.doFilter(servletRequest, servletResponse);
            TransactionManager.commit();
            System.out.println("提交事务...");
        }catch (Exception e){
            e.printStackTrace();
            try {
                TransactionManager.rollback();
                System.out.println("回滚事务....");
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
        }
    }

    @Override
    public void destroy() {

    }
}

```

​	

# 	