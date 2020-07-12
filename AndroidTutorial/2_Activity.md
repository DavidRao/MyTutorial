# 第二章 活动

## 2.1 Activity

### 2.2.1 手动创建活动

`包 → new → Activity → Empty Activity`

- 可选择性勾选
  - `Generate Layout File` （自动生成布局文件）
  - `Launch Activity` （设置为主活动）
  - `Backwards Compatibility` （向下兼容模式）

### 2.2.2 创建和加载布局

layout.xml

`src/main/res/layout → Layout resource file`

- `android:id`
  - `="@id/id_name"` 引用id
  - `="@+id/id_name"` 定义id
- `android:layout_width`、`android:layout_height`
  - `="match parent"`
  - `="wrap_content`"

```java
// Activity.class

// 加载一个页面
setContentView(R.layout.first_lyout)
```



### 2.2.3 在AndroidManifest文件中注册

```xml
<!--AndroidManifest-->

<application ... package="com.example.activitytest">
	...
    <!--name指定那一活动，label指定标题栏显示内容-->
	<activity android:name=".MainActivity"
		android:label = "This is FirstActivity"
         android:theme="@android:style/Theme.Dialog">
            <intent-filter>
                <!--配置主活动-->
            	<action android:name="android.intent.action.MAIN" />
            	<category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    <!--非主活动配置-->
    <activity android:name=".SecondActivity"></activity>
	...
</application>
```

### 2.2.4 在活动中使用Toast

### 2.2.5 在活动中使用Menu

``src/main/res/menu → Menu resource file`

```xml
<!--menu.xml-->

<menu ...>
    <item
          android:id="@+id/add_item"
          android:title="Add"/>
    <item
          android:id="@+id/remove_item"
          android:title="Remove">
</menu>
```

```java
// Activity.class

// 创建菜单
public boolean onCreateOptionsMenu(Menu menu){
	getMenuInflater().inflate(R.menu.my_menu, menu);
	return true;
}
```

- `getMenuInflater()` 得到`MenuInflater`对象，再调用`inflate`方法
- `return true` 允许显示菜单
- `return false` 禁止显示菜单

```java
// Activity.class

// 设置菜单响应
public boolean onOptionsItemSelected(MenuItem item){
    switch(item.getItemId()) {
        case R.id.add_item:
            break;
        case R.id.remove_item:
            break;
    }
    return true;
}
```

### 2.2.6 销毁一个活动

```java
//销毁当前活动,效果和按下Back键一样
finish();
```

## 2.3 Intent

### 2.3.1 使用显式Intent

```java
// Activity.class

Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
startActivity(intent)
```

- `FirstActivity.this`: 上下文
- `SecondActivity.class`: 目标活动

### 2.3.2 使用隐式Intent

```xml
<!--AndroidManifest.xml-->

<!--指定SecondActivity能够响应的action和category-->
<activity android: name=".SecondActivity">
	<intent-filter>
		<action android:name="com.example.activitytest.ACTION_START" />
        <action android:name="android.intent.category.DEFAULT" />
        <action android:name="com.example.activitytest.MY_CATEGORY">
        <!--只有<action>和<category>中的内容同时能够匹配上Intent中指定的action和category，该活动才能响应intent-->
	</intent-filter>
</activity>
```

```java
// Activity.class

Intent intent = new Intent("com.example.activitytest.ACTION_START");
// intent.addCategory("com.example.activitytest.MY_CATEGORY");
startActivity(intent)
```

### 2.3.3 更多隐式Intent的用法

```Java
// Activity.class

Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("http://www.baidu.com"));
startAcivity(intent);
```

- `Intent.ACTION_VIEW` 是常量值`android.intent.action.VIEW`
- `Uri.parse()`将一个网址字符串解析成一个Uri对象

### 2.3.4 向下一个活动传递数据

```java
// FirstActivity.class

String data = "Hello SecondActivity";
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
intent.putExtra("extra data", data);
startActivity(intent);
```

```java
// SecondActivity.class

Intent intent = getIntent();
String data = intent.getStringExtra("extra data");
//getExtra() getBooleanExtra
```

### 2.3.5 返回数据给上一个活动

```java
// FirstActivity.class

Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
startActivityForResult(intent, 1); // 1是请求码,唯一,用于给下一个活动识别

// requestCode请求码，即上面的1；resultCode
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data){
 	switch (requestCode){
        case 1:
            if(resultCode == RESULT_OK){
            	String returnData = data.getStringExtra("data_return");
                Log.d("FirstActivity", returnedData);
            }
            break;
    }   
}
```

```java
// SecondActivity.class

Intent intent = new Intent();
intent.putExtra("data_return", "Hello FirstActivity");
setResult(RESULT_OK， intent)；//RESULT_OK,RESULT_CANCELED
finish();

// 当返回上一活动，当前活动被销毁后自动回调上一个活动的onActivityResult()
// 可以重写onBackPressed()
@Override
public void onBackPressed() {
    Intent intent = new Intent();
    intent.putExtra("data_return", "Hello FirstActivity");
    setResult(RESULT_OK, intent);
    finish();
}
```



## 2.4 活动的生命周期

### 2.4.1 返回栈



### 2.4.2 活动状态

1. 运行状态（可见）
2. 暂停状态（可见）
3. 停止状态
4. 销毁状态

### 2.4.3 活动的生存期

<img src="D:\MyRepository\MyTutorial\img\AndroidTutorial\ActivityLife.jpg" style="zoom:67%;" />

### 2.4.4 体验活动的生命周期

### 2.4.5 活动被回收了怎么办

```java
// Activity.class

@Override
protected void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    String tempData = "Data is Here";
    outState.putString("data_key", tempData);
}

@Override
protected void onCreate(Bundle savedInstanceState){
    super.onCreate(savedInstanceState);
    Log.d(TAG, "onCreate");
    setContentView(R.layout.activity_main);
    if(savedInstanceState != null){
        String tempData = savedInstanceState.getString("data_key");
        Log.d(TAG, tempData);
    }
}
```







