太久没有写安卓项目，差点不会写，哭辽。先随便记点东西，以后忘记了就直接来查博客。

### 按钮使用、活动跳转

先在布局文件定义一个按钮，id要在java程序中用到

```xml
<Button
        android:id="@+id/button_article"
        android:text="微信文章"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
```

在java中使用按钮

```java
Button articleButton = findViewById(R.id.button_article);
articleButton.setOnClickListener(new View.OnClickListener()
{
	@Override
	public void onClick(View v)
	{
		Intent intent = new Intent(MainActivity.this, ArticleListActivity.class);
		startActivity(intent);
	}
});
```

### 隐藏标题栏主题

### 权限设置

先在`AndroidManifest.xml`中声明权限，在`minifest`下一级，与`application`同级

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.cc.myapplication">
    
    <!--在此处申请权限-->
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name">
        <activity android:name=".activity.MainActivity">
        </activity>
    </application>
</manifest>
```

