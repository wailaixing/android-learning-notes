#### BroadcastReceiver

BroadcastReceiver用来接收程序(包括用户开发的或者系统内建的程序)所发出的`Broadcast Intnet`。

当应用程序发出一个`Broadcast Intent` 之后，所有匹配该Intent的`BroadcastReceiver`都有可能被启动。

BroadcastReceiver本质就是一个监听器，只要实现`BroadcastReceiver`的`onReceiver`方法既可。实现了`BroadcastReceiver`就可以指定`BroadcastReceiver`所匹配的`Intent`。有两种方法：
> 一、通过代码动态指定，调用`BroadcastReceiver`的`Context`的`registerReceiver`方法指定。
例：
```java
IntentFilter filter=new IntentFilter("android.provider.Telephony.SMS_RECEIVED");
IncomingSMSReceiver receiver=new IncomingSMSReceiver();
registerReceiver(receiver,filter);
```

> 二、在`AndroidManifest.xml`文件中配置
例：
```xml
<receiver android:name=".IncomingSMSReceiver">
    <intent-filter>
        <action android:name="android.provider.Telephony.SMS_RECEIVED" />
    </intent-filter>
</receiver>
```

**注：如果`onReceive`方法无法在10秒内完成，Android会认为该程序无响应，所以不要再`onReceive`内做耗时操作**

<br>
例：
`BroadcastMain.java`
```java
public class BroadcastMain extends Activity{
    Button send;
    @Override
    public void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_main);
        send=(Button)findViewById(R.id.send);
        send.setOnclickListener(new OnclickListener(){
            @Override
            public void onClick(View v){
                //创建Intent
                Intent intent =new Intent();
                //设置Intent的action属性
                intent.setAction("org.example.action.BROADCAST");
                intent.putExtra("msg","消息");
                sendBroadcast(intent);
            }
        });
    }
}
```

`MyReceiver.java`
```java
public class MyReceiver extends BroadcastReceiver{
    @Override
    public void onReceiver(Content content,Intent intent){
        Toast.makeText(context,intent.getAction+intent.getStringExtra("msg",0)).show();
    }
}
```

`AndroidManifest.xml`
```xml
<receiver android:name=".MyReceiver">
    <intent-filter>
        <action android:name="org.example.action.BROADCAST" />
    </intent-filter>
</receiver>
```

<br>
##### 有序广播



##### 系统广播消息

|广播常量|描述|
|:--:|:---:|
|ACTION_TIME_CHANGED|系统时间被改变|
|ACTION_DATA_CHANGED|系统日期被改变|
|ACTION_TIMEZONE_CHANGED|系统时区被改变|
|ACTION_BOOT_COMPLETED|系统启动完成|
|ACTION_PACKAGE_ADDED|系统添加包|
|ACTION_PACKAGE_CHANGED|系统的包改变|
|ACTION_PACKAGE_REMOVEED|系统的包被删除|
|ACTION_PACKAGE_RESTARTED|系统的包被重启|
|ACTION_PACKAGE_DATA_CLEARED|系统包的数据被清空|
|ACTION_BATTERY_CHANGED|电池电量改变|
|ACTION_BATTERY_LOW|电池电量低|
|ACTION_POWER_CONNECTED|系统连接电源|
|ACTION_POWER_DISCONNECTED|系统和电源断开|
|ACTION_SHUTDOWN|系统关闭|