# Spring-自定义SpringIOC

# 1、工厂模式

- BeanFactory

```java
package com.wnx.spring.factory;

import com.wnx.spring.dao.UserDao;


public class BeanFactory {

    public static UserDao getUserDao(){
        return new UserDao();
    }
}
```

```java
package com.wnx.spring.service;

import com.wnx.spring.dao.UserDao;
import com.wnx.spring.factory.BeanFactory;

public class UserService {

    void addUser(){
        UserDao userDao = BeanFactory.getUserDao();
        userDao.addUser();
    }
}
```

# 2、XML解析+容器思想+反射+工厂模式

```xml
<?xml version="1.0" encoding="utf-8"?>
<beans>
  <bean id = "userDao" class="com.wnx.spring.dao.UserDao"/>
  <bean id = "uerService" class="com.wnx.spring.service.UserService">
    <property name="userDao" ref="userDao"/>
  </bean>
</beans>
```

```java
public interface BeanFactory {
    Object getBean(String id);
}


public class ClassPathXmlApplicationContext implements BeanFactory{
    private Map<String,Object> beanMap = new HashMap<>();

    @SneakyThrows
    public ClassPathXmlApplicationContext(String path){
        if(StringUtil.isEmpty(path)){
            throw new RuntimeException("IOC容器的配置文件没有指定...");
        }	
			//XML+反射+工厂模式
            InputStream inputStream = getClass().getClassLoader().getResourceAsStream(path);
            DocumentBuilderFactory documentBuilderFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder documentBuilder = documentBuilderFactory.newDocumentBuilder() ;
            Document document = documentBuilder.parse(inputStream);


            NodeList beanNodeList = document.getElementsByTagName("bean");
            for(int i = 0 ; i<beanNodeList.getLength() ; i++){
                Node beanNode = beanNodeList.item(i);
                if(beanNode.getNodeType() == Node.ELEMENT_NODE){
                    Element beanElement = (Element)beanNode ;
                    String beanId =  beanElement.getAttribute("id");
                    String className = beanElement.getAttribute("class");
                    Class beanClass = Class.forName(className);
                    Object beanObj = beanClass.newInstance() ;
                 
                    beanMap.put(beanId , beanObj) ;
                 
                }
            }
            //5.组装bean之间的依赖关系
            for(int i = 0 ; i<beanNodeList.getLength() ; i++) {
                Node beanNode = beanNodeList.item(i);
                if (beanNode.getNodeType() == Node.ELEMENT_NODE) {
                    Element beanElement = (Element) beanNode;
                    String beanId = beanElement.getAttribute("id");
                    NodeList beanChildNodeList = beanElement.getChildNodes();
                    for (int j = 0; j < beanChildNodeList.getLength(); j++) {
                        Node beanChildNode = beanChildNodeList.item(j);
                        if (beanChildNode.getNodeType() == Node.ELEMENT_NODE && "property".equals(beanChildNode.getNodeName())) {
                            Element propertyElement = (Element) beanChildNode;
                            String propertyName = propertyElement.getAttribute("name");
                            String propertyRef = propertyElement.getAttribute("ref");
                            //1) 找到propertyRef对应的实例
                            Object refObj = beanMap.get(propertyRef);
                            //2) 将refObj设置到当前bean对应的实例的property属性上去
                            Object beanObj = beanMap.get(beanId);
                            Class beanClazz = beanObj.getClass();
                            Field propertyField = beanClazz.getDeclaredField(propertyName);
                            propertyField.setAccessible(true);
                            propertyField.set(beanObj, refObj);
                        }
                    }
                }
            }
    }

    @Override
    public Object getBean(String id) {
        return beanMap.get(id);
    }
}

```

# 3、低配的SpringIOC完成

```java
package com.wnx.spring.service;

import com.wnx.spring.dao.UserDao;

public class UserService {

    private UserDao userDao;

    public void addUser(){
        userDao.addUser();
    }
}


```

```java
package com.wnx.spring;

import com.wnx.spring.factory.ClassPathXmlApplicationContext;
import com.wnx.spring.service.UserService;


public class Test {
    public static void main(String[] args) {
        ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring.xml");
        UserService userService = (UserService) ioc.getBean("uerService");
        userService.addUser();
    }
}
```

