# Use-BigDecimalApi-setScale-HALF_UP

# 1、设置精度，采用四舍五入模式。

```java
@Slf4j
public class BigDecimalTest {

    @Test
    public void test_HALF_UP(){
        //向上取大的
        BigDecimal b1 = new BigDecimal("1.1546787487464541");
        log.info(" 精度截取3位 => {} ",b1.setScale(3, RoundingMode.HALF_UP));
        log.info(" 精度截取4位 => {} ",b1.setScale(4, RoundingMode.HALF_UP));
        log.info(" 精度截取4位 => {} ",b1.setScale(5, RoundingMode.HALF_UP));

        //17:37:20.038 [main] INFO com.wnx.mall.test6.BigDecimalTest -  精度截取3位 => 1.155
        //17:37:20.041 [main] INFO com.wnx.mall.test6.BigDecimalTest -  精度截取4位 => 1.1547
        //17:37:20.041 [main] INFO com.wnx.mall.test6.BigDecimalTest -  精度截取4位 => 1.15468

        BigDecimal b2 = new BigDecimal("-1.1546787487464541");
        log.info(" 精度截取3位 => {} ",b2.setScale(3, RoundingMode.HALF_UP));
        log.info(" 精度截取4位 => {} ",b2.setScale(4, RoundingMode.HALF_UP));
        log.info(" 精度截取4位 => {} ",b2.setScale(5, RoundingMode.HALF_UP));
        
        //17:38:23.542 [main] INFO com.wnx.mall.test6.BigDecimalTest -  精度截取3位 => -1.155 
        //17:38:23.542 [main] INFO com.wnx.mall.test6.BigDecimalTest -  精度截取4位 => -1.1547 
        //17:38:23.542 [main] INFO com.wnx.mall.test6.BigDecimalTest -  精度截取4位 => -1.15468 
    }
    
    
}

```



