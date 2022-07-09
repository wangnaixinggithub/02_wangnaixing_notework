# Android-TextView-shadowXXX

- 和阴影有关的三个控制量：阴影颜色，阴影半径 阴影覆盖X轴 阴影覆盖Y轴。调节合理就能拥有阴影效果。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--设定阴影颜色 阴影半径 阴影覆盖X轴 Y轴-->
    <TextView
        android:id="@+id/tv_one"
        android:text="@string/tv_one"
        android:textColor="@color/Red"
        android:background="@color/purple_700"
        android:textStyle="bold"
        android:shadowColor="@color/black"
        android:shadowRadius="50"
        android:shadowDx="10"
        android:shadowDy="10"
        android:gravity="center_horizontal"
        android:textSize="@dimen/wangnaixing_margin"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

![image-20220618151604070](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618151604070.png)