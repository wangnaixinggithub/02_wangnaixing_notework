# Date-plusDays-Three-Way

## 1、JDK8之前



```java
	 @Test
    public void nowDate_after_one_day(){
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");

        Date bzsj = new Date();
        log.info("bzsj=>{}",simpleDateFormat.format(bzsj));


        Calendar sjrqCalendar = Calendar.getInstance();
        sjrqCalendar.setTime(bzsj);//默认数据日期=编制时间+1天
        sjrqCalendar.add(Calendar.DATE, 1);
        sjrqCalendar.set(Calendar.HOUR_OF_DAY, 0);
        sjrqCalendar.set(Calendar.MINUTE, 0);
        sjrqCalendar.set(Calendar.SECOND, 0);
        Date  sjrq = sjrqCalendar.getTime();

        log.info("sjrq=>{}", simpleDateFormat.format(sjrq));


    }
```



## 2、JDK8之后

```java
	@Test
    public void nowDate_after_one_day_JDK8(){
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ISO_LOCAL_DATE;

        LocalDate bzsj = LocalDate.now();
        log.info("bzsj=>{}",bzsj.format(dateTimeFormatter)); //默认数据日期=编制时间+1天

        LocalDate sjrq = bzsj.plusDays(1L);
        log.info("sjrq=>{}", sjrq.format(dateTimeFormatter));

    }
```

## 3、DateUtil

```java
  @Test
    public void nowDate_after_one_day_DateUtil() throws ParseException {
        Date bzsj = new Date();
        Date sjrq = DateUtils.addDays(bzsj, 1);
        log.info("sjrq => {}",sjrq);
        
    }
```

```java
package org.apache.commons.lang3.time;
public class DateUtils {

    public static Date addDays(final Date date, final int amount) {
        return add(date, Calendar.DAY_OF_MONTH, amount);
    }
    
    private static Date add(final Date date, final int calendarField, final int amount) {
        validateDateNotNull(date);
        final Calendar c = Calendar.getInstance();
        c.setTime(date);
        c.add(calendarField, amount);
        return c.getTime();
    }
	
    
}
```

