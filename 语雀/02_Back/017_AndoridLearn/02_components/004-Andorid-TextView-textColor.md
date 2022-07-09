# Andorid-TextView-textColor

- 设置文本颜色

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <!--设置组件文本颜色内容-->
    <TextView
        android:id="@+id/tv_one"
        android:text="王乃醒是一个大帅哥"
        android:textColor="@color/Red"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </TextView>

</LinearLayout>
```

- 能引用到`/resource/values/colors.xml`的颜色

```xml
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

![image-20220618143510902](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220618143510902.png)