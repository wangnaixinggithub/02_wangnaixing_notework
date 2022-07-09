# Spring MVC 父子容器

## 1、父子容器

1、在Spring的web应用，Spring会创建出来两个SpringIOC容器。关于WebMvc配置的就放在ServletWebApplication中。

2、关于Service。Dao 就放在RootWebApplicationContext中。

![image-20220508121302991](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/017_SpringMVC-SpringWeb%E5%B7%A5%E7%A8%8B%E7%9A%84%E4%B8%A4%E4%B8%AA%E5%AE%B9%E5%99%A8.assets/image-20220508121302991.png)

## 2、父子关系源码

#### 1、加载RootWebApplicationContext

```java
public class ContextLoaderListener extends ContextLoader implements ServletContextListener {}
```

```java
public class ContextLoader {
public WebApplicationContext initWebApplicationContext(ServletContext servletContext) {
    //1。保证只能由一个Root WebApplicationContext
		if (servletContext.getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE) != null) {
			throw new IllegalStateException(
					"Cannot initialize context because there is already a root application context present - " +
					"check whether you have multiple ContextLoader* definitions in your web.xml!");
		}

		servletContext.log("Initializing Spring root WebApplicationContext");
		Log logger = LogFactory.getLog(ContextLoader.class);
		if (logger.isInfoEnabled()) {
			logger.info("Root WebApplicationContext: initialization started");
		}
		long startTime = System.currentTimeMillis();

		try {
			//2.创建web WebApplicationContext
			if (this.context == null) {
                 //读取<context-param>
				this.context = createWebApplicationContext(servletContext);
			}
			if (this.context instanceof ConfigurableWebApplicationContext) {
				ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext) this.context;
				if (!cwac.isActive()) {
					
					if (cwac.getParent() == null) {
					
						ApplicationContext parent = loadParentContext(servletContext);
						cwac.setParent(parent);
					}
                    //3.把spring.xml配置读入WebApplicationContext
					configureAndRefreshWebApplicationContext(cwac, servletContext);
				}
			}
            //4. Root WebApplicationContext存入servletContext中
			servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, this.context);

			ClassLoader ccl = Thread.currentThread().getContextClassLoader();
			if (ccl == ContextLoader.class.getClassLoader()) {
				currentContext = this.context;
			}
			else if (ccl != null) {
				currentContextPerThread.put(ccl, this.context);
			}

			if (logger.isInfoEnabled()) {
				long elapsedTime = System.currentTimeMillis() - startTime;
				logger.info("Root WebApplicationContext initialized in " + elapsedTime + " ms");
			}

			return this.context;
		}
		catch (RuntimeException | Error ex) {
			logger.error("Context initialization failed", ex);
			servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, ex);
			throw ex;
		}
	}
}
```

#### 2、加载Servlet WebApplicationContext

```java
public abstract class FrameworkServlet extends HttpServletBean implements ApplicationContextAware {
protected WebApplicationContext initWebApplicationContext() {
        WebApplicationContext rootContext = WebApplicationContextUtils.getWebApplicationContext(this.getServletContext());
        WebApplicationContext wac = null;
        ....
        if (wac == null) {
            //根据Root WebApplicationContext 创建webApplicationContext.
            wac = this.createWebApplicationContext(rootContext);
        }

		....
        return wac;
    }
}
```

## 3、So.子容器可访问父容器的Bean.

```java
public class DefaultListableBeanFactory extends AbstractAutowireCapableBeanFactory implements ConfigurableListableBeanFactory, BeanDefinitionRegistry, Serializable {
	@Nullable
    private <T> T resolveBean(ResolvableType requiredType, @Nullable Object[] args, boolean nonUniqueAsNull) {
        //1.首先在子容器中查询Bean
        NamedBeanHolder<T> namedBean = this.resolveNamedBean(requiredType, args, nonUniqueAsNull);
        if (namedBean != null) {
            return namedBean.getBeanInstance();
        } else { //没有，其次则到父容器中查询Bean
            BeanFactory parent = this.getParentBeanFactory();
            if (parent instanceof DefaultListableBeanFactory) {
                return ((DefaultListableBeanFactory)parent).resolveBean(requiredType, args, nonUniqueAsNull);
            } else if (parent != null) {
                ObjectProvider<T> parentProvider = parent.getBeanProvider(requiredType);
                if (args != null) {
                    return parentProvider.getObject(args);
                } else {
                    return nonUniqueAsNull ? parentProvider.getIfUnique() : parentProvider.getIfAvailable();
                }
            } else {
                //最后返回null
                return null;
            }
        }
    }
}
```

