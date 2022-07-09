# Android-TextView-background

- 设定控件的背景颜色，听说还可以设置为图片。在本例子中，我设置背景蓝色。	

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--设置控件的背景颜色-->
    <TextView
        android:id="@+id/tv_one"
        android:text="王乃醒是一个大帅哥"
        android:textColor="@color/Red"
        android:background="@color/purple_700"
        android:textStyle="bold"
        android:gravity="center_horizontal"
        android:textSize="@dimen/wangnaixing_margin"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

```xml
<!--res/values/colors.xml-->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
    <color name="Red">#BC5B2E</color>
</resources>
```

![image-20220618145437309](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618145437309.png)