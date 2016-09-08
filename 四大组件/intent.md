### intent及intent-filter

#### intent介绍

程序的3个核心组件——Activity、services、广播接收器——是通过intent传递消息的。

<br>
#### 隐式意图

通过指定一组动作或数据,开启Activity，一般用与开启系统应用。

<br>
#### 显式意图

通过指定具体的包名和类名,开启Activity，一般用于开启用户自己的界面。

```java
    Intent intent=new Intent(this,TestActivity.class);
    startActivity(intent);
```

<br>
#### Intent 属性介绍

Intent的Action、Categroy属性都是字符串，Action代表Intent要完成的一个抽象的"动作"，Categroy用于为Action添加额外的附加信息。
程序要启动哪个Activity，取决于Activity配置中的`<intent-filter.../>`元素中的配置。



#### 数据传递

**例：**

`mainActivity.java` 片段
```java
    Intent intent = new Intent(this,ResultActivity.class);
    //将name、sex传递到结果页面
    intent.putExtra("name",name);
    intent.putExtra("sex",sex);
    //开启Activity
    startActivity(intent);
```

`ResultActivity.java`片段
```java
    //获取开启此Activity的对象
    Intent intent = getIntent();
    //获取携带的数据
    String name=intent.getStringExtra("name");
    int sex=intent.getIntExtra("sez");
```

<br>
#### intent-filter
`<intent-filter.../>`元素是AndroidManifest.xml文件中的`<activity.../>`、`<service.../>`或`<receiver.../>`元素的子元素,用于配置该Activity所能"响应的"Intent。`<intent-filter.../>`元素通常包含以下子元素: `<action.../>`、`<category.../>`、`<data.../>`。

Intent 中的`Action`、`Categroy`属性是Intent的启动要求，每个Intent 只能指定一个`Action`,多个`Categroy`要求。IntentFilter(通过`<intent-filter>`配置)，申明该组件(`Activity`、`Service`、`BroadcastReceiver`)能满足的要求，每个组件可以声明满足多个`Action`、`Categroy`要求。只要某个组件能满足的要求大于或等于Intent所要的要求，Intent就可以启用该组件。

##### 启动Activity的标准Action：

|Action常量|对应字符串|简单说明|
|:---:|:---:|:---:|
|ACTION_MAIN|android.intent.action.MAIN|程序入口|
|ACTION_VIEW|android.intent.action.VIEW|显示指定数据|
|ACTION_DEIT|android.intent.action.DEIT|编辑指定数据|
||||

##### 启动Activity的标准Categroy:




#### Data、Type属性

`Data`属性通常为`Action`属性提供操作数据，`Data`属性通常接收一个`Uri`对象，`Uri`格式:
`scheme://host:port/path`

`Type`属性用来指定该`Data`所指的`Uri`对应的`MIME`类型，该类型可以是任何自定义的`MIME`类型(符合`xxx/xxx`格式)。

**注：**
> * 如果为Intent先设置Data属性，后设置Type属性，则Type会覆盖Data属性。
> * 如果为Intnet先设置Type属性，后设置Data属性，则Data回覆盖Type属性。
> * 如果想要同时有两个属性，通过Intetn的setDataAndType()方法。

在`AndroidManifest.xml`文件中为组件声明Data、Type属性都通过`<data.../>`元素：
```xml
    <data android:mimeType=""
        android:scheme=""
        android:host=""
        android:port=""
        android:path=""
        android:pathPrefix=""
        android:pathPattern="" >
```

> `mimeType`:声明该组件所能匹配的Intent的Type属性。
> `scheme`:声明该组件所能匹配的Intent的Data属性的scheme部分。
> `host`:声明该组件所能匹配的Intent的Data属性的host部分。
> `port`:声明该组件所能匹配的Intent的Data属性的port部分。
> `path`:声明该组件所能匹配的Intent的Data属性的path部分。
> `pathPrefix`:声明该组件所能匹配的Intent的Data属性的path前缀。
> `pathPattern`:声明该组件所能匹配的Intent的Data属性的scheme部分。

**例：**

`AndroidManifest.xml`

```xml
    <intent-filter>
        <action android:name="xx" />
        <categroy android:name="android.intent.categroy.DEFAULT" />
        <data android:scheme="scheme"
            android:host="host"
            android:port="port"
            android:path="path"
            android:mimeType="abc/xxx"  />
    </intent-fliter>
```

`testActivity.java`

```java
    Intent intent = new Intent();
    //同时设置Data、Type属性
    intent.setData(Uri.parse("scheme://host:port/path"),"abc/xxx");
    startActivity(intent);
```

**注：**

在配置`<Intent-filter../>`时，`<action../>`子元素的name是随意指定的。如果希望`<data../>`能正常工作，那么必须配置`<action../>`的一个子元素，该元素的`android:name`可以随意指定。

