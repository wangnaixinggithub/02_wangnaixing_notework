# Tomcat-Config-server.xml-Executor

# 1、Executor Config

```xml
<Executor name="tomcatThreadPool"
namePrefix="catalina‐exec‐"
maxThreads="200"
minSpareThreads="100"
maxIdleTime="60000"
maxQueueSize="Integer.MAX_VALUE"
prestartminSpareThreads="false"
threadPriority="5"
className="org.apache.catalina.core.StandardThreadExecutor"/>
```

属性说明：

![image-20220706103841445](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706103841445.png)

# 2、In Default server.xml Config

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
  <GlobalNamingResources>
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>
  <Service name="Catalina">
      
      <!-- 配置 Executor线程池 -->
	<Executor name="tomcatThreadPool"
		namePrefix="catalina‐exec‐"
		maxThreads="200"
		minSpareThreads="100"
		maxIdleTime="60000"
		maxQueueSize="Integer.MAX_VALUE"
		prestartminSpareThreads="false"
		threadPriority="5"
		className="org.apache.catalina.core.StandardThreadExecutor"/>
		
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
   
    <Engine name="Catalina" defaultHost="localhost">

    
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      </Host>
    </Engine>
  </Service>
</Server>
```

# 3、Start Tomcat And JdkConsole See Thread

找到JDK安装目录下的bin目录：

![image-20220706105358340](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706105358340.png)

配置线程池之前：只有自己Server组件自己定义的线程：

![image-20220706105841449](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706105841449.png)

配置线程池之后：多了许多我配置的线程

![image-20220706110554140](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706110554140.png)