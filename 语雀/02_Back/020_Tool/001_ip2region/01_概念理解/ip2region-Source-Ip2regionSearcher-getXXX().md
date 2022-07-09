# ip2region-Source-Ip2regionSearcher-getXXX()

通过查询算法查询后，返回`IpInfo`对象，然后Ip2regionSearcher，提供了`#getXXX()` 能获取到`IpInfo`中封装的具体信息

# 1、Ip2regionSearcher

```java

package net.dreamlu.mica.ip2region.core;

import net.dreamlu.mica.ip2region.utils.IpInfoUtil;
import org.springframework.lang.Nullable;

import java.util.function.Function;

/**
 * ip 搜索器
 *
 * @author dream.lu
 */
public interface Ip2regionSearcher {

	//1、提供了三种查询算法 memorySearch btreeSearch binarySearch
	@Nullable
	IpInfo memorySearch(long ip);
	@Nullable
	IpInfo memorySearch(String ip);
	@Nullable
	IpInfo btreeSearch(long ip);
	@Nullable
	IpInfo btreeSearch(String ip);
	@Nullable
	IpInfo binarySearch(long ip);
	@Nullable
	IpInfo binarySearch(String ip);
    
    
    @Nullable
	IpInfo getByIndexPtr(long ptr);
    


	//2、 读取 ipInfo 中的信息
	@Nullable
	default String getInfo(long ip, Function<IpInfo, String> function) {
		return IpInfoUtil.readInfo(memorySearch(ip), function);
	}
	@Nullable
	default String getInfo(String ip, Function<IpInfo, String> function) {
		return IpInfoUtil.readInfo(memorySearch(ip), function);
	}
	
	//2、 读取 address 中的信息
	@Nullable
	default String getAddress(long ip) {
		return getInfo(ip, IpInfo::getAddress);
	}
	@Nullable
	default String getAddress(String ip) {
		return getInfo(ip, IpInfo::getAddress);
	}
    //2、读取 addressAndIsp 中的信息
	@Nullable
	default String getAddressAndIsp(long ip) {
		return getInfo(ip, IpInfo::getAddressAndIsp);
	}
	@Nullable
	default String getAddressAndIsp(String ip) {
		return getInfo(ip, IpInfo::getAddressAndIsp);
	}

}

```

# 2、IpInfoUtil#readInfo()

```java
/**
 * ip 信息详情
 *
 * @author L.cm
 */
public class IpInfoUtil {
	@Nullable
	public static String readInfo(@Nullable IpInfo ipInfo, Function<IpInfo, String> function) {
		if (ipInfo == null) {
			return null;
		}
		return function.apply(ipInfo);
	}

}
```

# 3、IpInfo

```java
package net.dreamlu.mica.ip2region.core;

import lombok.Data;
import net.dreamlu.mica.core.utils.StringPool;
import net.dreamlu.mica.core.utils.StringUtil;

import java.io.Serializable;
import java.util.LinkedHashSet;
import java.util.Objects;
import java.util.Set;

/**
 * ip 信息详情
 *
 * @author L.cm
 */
@Data
public class IpInfo implements Serializable {

	/**
	 * 城市id
	 */
	private Integer cityId;
	/**
	 * 国家
	 */
	private String country;
	/**
	 * 区域
	 */
	private String region;
	/**
	 * 省
	 */
	private String province;
	/**
	 * 城市
	 */
	private String city;
	/**
	 * 运营商
	 */
	private String isp;
	/**
	 * region ptr in the db file
	 */
	private int dataPtr;
	/**
	 * 拼接完整的地址
	 *
	 * @return address
	 */
	public String getAddress() {
		Set<String> regionSet = new LinkedHashSet<>();
		regionSet.add(country);
		regionSet.add(region);
		regionSet.add(province);
		regionSet.add(city);
		regionSet.removeIf(Objects::isNull);
		return StringUtil.join(regionSet, StringPool.EMPTY);
	}
	/**
	 * 拼接完整的地址
	 *
	 * @return address
	 */
	public String getAddressAndIsp() {
		Set<String> regionSet = new LinkedHashSet<>();
		regionSet.add(country);
		regionSet.add(region);
		regionSet.add(province);
		regionSet.add(city);
		regionSet.add(isp);
		regionSet.removeIf(Objects::isNull);
		return StringUtil.join(regionSet, StringPool.SPACE);
	}
}

```

