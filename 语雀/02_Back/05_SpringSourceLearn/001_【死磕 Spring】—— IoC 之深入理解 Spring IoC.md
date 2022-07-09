# 【死磕 Spring】—— IoC 之深入理解 Spring IoC

# 1、IOC组件关系图

![image-20220504154319850](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154319850.png)

# 2、组件们

## 2.1 Resource 体系

`org.springframework.core.io.Resource`，对资源的抽象。它的每一个实现类都代表了一种资源的访问策略，如 ClassPathResource、RLResource、FileSystemResource 等。

![image-20220504154439537](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154439537.png)

## 2.2 ResourceLoader 体系

有了资源，就应该有资源加载，Spring 利用 `org.springframework.core.io.ResourceLoader` 来进行统一资源加载，类图如下：

![image-20220504154600601](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154600601.png)

## 2.3 BeanFactory 体系

`org.springframework.beans.factory.BeanFactory`，是一个非常纯粹的 bean 容器，它是 IoC 必备的数据结构，其中 BeanDefinition 是它的基本结构。BeanFactory 内部维护着一个BeanDefinition map ，并可根据 BeanDefinition 的描述进行 bean 的创建和管理。

![image-20220504154632561](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154632561.png)

- BeanFactory 有三个直接子类 ListableBeanFactory、HierarchicalBeanFactory 和 AutowireCapableBeanFactory 。
- DefaultListableBeanFactory 为最终默认实现，它实现了所有接口。

## 2.4 BeanDefinition 体系

`org.springframework.beans.factory.config.BeanDefinition` ，用来描述 Spring 中的 Bean 对象。

![image-20220504154723751](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154723751.png)

## 2.5 BeanDefinitionReader 体系

`org.springframework.beans.factory.support.BeanDefinitionReader` 的作用是读取 Spring 的配置文件的内容，并将其转换成 Ioc 容器内部的数据结构 ：BeanDefinition 。

![image-20220504154750495](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154750495.png)

## 2.6 ApplicationContext 体系

`org.springframework.context.ApplicationContext` ，这个就是大名鼎鼎的 Spring 容器，它叫做应用上下文，与我们应用息息相关。它继承 BeanFactory ，所以它是 BeanFactory 的扩展升级版，如果BeanFactory 是屌丝的话，那么 ApplicationContext 则是名副其实的高富帅。由于 ApplicationContext 的结构就决定了它与 BeanFactory 的不同，其主要区别有：

1. 继承 `org.springframework.context.MessageSource` 接口，提供国际化的标准访问策略。
2. 继承 `org.springframework.context.ApplicationEventPublisher` 接口，提供强大的**事件**机制。
3. 扩展 ResourceLoader ，可以用来加载多种 Resource ，可以灵活访问不同的资源。
4. 对 Web 应用的支持。

![image-20220504154814829](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220504154814829.png)

## 2.6 小结

通过上面五个体系，我们可以看出，IoC 主要由 `spring-beans` 和 `spring-context` 项目，进行实现。