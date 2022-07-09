# ip2region-Source-Ip2regionSearcher-Why-Can-Use-@Autowired

# 1、Ip2regionProperties

```java
package net.dreamlu.mica.ip2region.config;

import lombok.Getter;
import lombok.Setter;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.cloud.context.config.annotation.RefreshScope;

/**
 * ip2region 配置类
 *
 * @author L.cm
 */
@Getter
@Setter
@RefreshScope
@ConfigurationProperties(Ip2regionProperties.PREFIX)
public class Ip2regionProperties {
	public static final String PREFIX = "mica.ip2region";

	/**
	 * ip2region.db 文件路径
	 */
	private String dbFileLocation = "classpath:ip2region/ip2region.db";

}

```

# 2、Ip2regionConfiguration

这里看到其实它写了一个配置类` Ip2regionConfiguration` ，利用 `@Configuration` 注解，这样的话，`SpringBoot `在启动的时候就会扫描注入，而`Ip2regionConfiguration`中其注入了一个Bean，默认Bean名称为` ip2regionSearcher` ，即我们就可以可以根据` @Autowired `来注入Bean

```java
package net.dreamlu.mica.ip2region.config;

import net.dreamlu.mica.ip2region.core.Ip2regionSearcher;
import net.dreamlu.mica.ip2region.impl.Ip2regionSearcherImpl;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.ResourceLoader;
import org.springframework.nativex.hint.NativeHint;
import org.springframework.nativex.hint.ResourceHint;

/**
 * ip2region 自动化配置
 *
 * @author L.cm
 */
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(Ip2regionProperties.class)
@NativeHint(resources = @ResourceHint(patterns = "^ip2region/ip2region.db"))
public class Ip2regionConfiguration {

	@Bean
	public Ip2regionSearcher ip2regionSearcher(ResourceLoader resourceLoader,
											   Ip2regionProperties properties) {
		return new Ip2regionSearcherImpl(resourceLoader, properties);
	}

}

```

