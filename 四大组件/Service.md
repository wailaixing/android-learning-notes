#### Service

四大组件都是运行在主线程，Android中的服务在后台运行，可以理解成是在后台运行并且是没有界面的Activity。

>  `Foreground process` 前台进程  用户正在交互  可以理解成相 当于Activity执行`onResume`方法

>  `Visible process` 可视进程 用户没有在交互 但用户还一直能看得见页面相当于Activity执行了`onPause`方法 
  
>  `Service Process`  服务进程  通过`startService()`开启了一个服务
  
>  `Background process`  后台进程  当前用户看不见页面 相当于Activity执行了`onStop`方法
  
>  `Empty process` 空进程

##### startService方式开启服务特点
 - 服务通过`startservice`方式开启 第一次点击按钮开启服务 会执行服务的`onCreate` 和 `onStart`方法
 - 如果第二次开始在点击按钮开启服务 服务之后执行`onStrat`方法
 - 服务被开启后 会在设置页面里面的 running里面找得到这个服务 
 - **`startservice` 方式开启服务 服务就会在后台长期运行 直到用户手工停止 或者调用`StopService`方法 服务才会被销毁**

<br>
##### bindService方式开启服务特点
 - 当点击按钮第一次开启服务 会执行服务的`onCreate`方法 和 `onBind`方法
 - 当我第二次点击按钮在调用bindservice 服务没有响应 
 - 当activity销毁的时候服务也销毁 
 - 通过bind方式开启服务,服务不能再设置页面里面找到,相当于是一个隐形的服务
 - bindservice不能多次解绑 多次解绑会报错

##### Service生命周期

![framework](../image/service.png)


<br>
##### 例:
电话窃听

1. 创建一个服务，用来监听电话状态，开启服务。
2. 在服务的oncreate方法里面实例化TelephoneManager类的实例
    `TelephonyManager tm=(TelephonyManager)getSystemService(TELEPHONY_SERVICE);
//获取电话管理者实例`
3. `tm.listen(new MyPhoneStateListener(),PhoneStateListener.LISTEN_CALL_STATE)
//注册一个电话状态的监听`
4. 电话监听类

```java
private class MyPhoneStateListenrer extends PhoneStateListener{
		//当设备的状态发生改变的时候调用
		@Override
		public void onCallStateChanged(int state, String incomingNumber) {
			//具体判断一下电话是处于什么状态
			switch (state) {
			case TelephonyManager.CALL_STATE_IDLE:  //空闲状态
				if (recorder!=null) {
					 recorder.stop();    //停止录
					 recorder.reset();   // You can reuse the object by going back to setAudioSource() step
					 recorder.release(); // Now the object cannot be reused
					
				}
				break;
				
			case TelephonyManager.CALL_STATE_OFFHOOK:  //接听状态 
				//开启录
				recorder.start();   // Recording is now started
				break;
				
			case TelephonyManager.CALL_STATE_RINGING:  //响铃状态
				//获取MediaRecorder类的实例
				recorder = new MediaRecorder();
				//设置音频的来源
				recorder.setAudioSource(MediaRecorder.AudioSource.MIC); 
				//设置音频的输出格式 
				recorder.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
				//设置音频的编码方式 
				recorder.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);
				//保存的文件路径
				recorder.setOutputFile("/mnt/sdcard/luyin.3gp");
				//准备录音
				try {
					recorder.prepare();
				} catch (IllegalStateException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				}
				break;
			}
			super.onCallStateChanged(state, incomingNumber);
		}
```

- 在`BroadcastReceiver`中启动Service
```java
public class BootReceiver extends BroadcastReceiver {
	//当手机重启后 会执行该方法 
	@Override
	public void onReceive(Context context, Intent intent) {
		Intent intent1 = new Intent(context,PhoneService.class);
		//开启服务 
		context.startService(intent1);
	}
}
```

- 在`AndroidManifest.xml`中声明及加权限
    `<service android:name="com.example.phonelistener.PhoneService"></service>`

```xml
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

<br>

##### 通过服务注册特殊广播接受者

1 ) 创建要注册的广播接受者
```java
public class ScreenReceiver extends BroadcastReceiver{
    @Override
    public void onReceive(Context context,Intent intent){
        //获取广播事件类型
        String action=intent.getAction();
        if("android.intent.action.SCREEN_OFF".equals(action)){
            System.out.println("锁屏");
        }else if("android.intent.action.SCREEN_ON".equals(action)){
            System.out.println("解锁");
        }
    }
}
```

2 ) 创建一个服务，来注册广播接收者
```java
public class ScreenService extends Service{
    private ScreenReceiver receiver;
    @Override
    public IBinder onBind(Intent intent){
        return null;
    }
    
	//当服务第一次启动的时候调用
	@Override
	public void onCreate() {
		//在这个方法里面注册广播接收者
		//获取ScreenReceiver实例
        receiver = new ScreenReceiver();
        //创建IntentFilter对象
		IntentFilter filter = new IntentFilter();
		//添加注册的事件
		filter.addAction("android.intent.action.SCREEN_OFF");
		filter.addAction("android.intent.action.SCREEN_ON");
		//通过代码的方式注册
		registerReceiver(receiver, filter);
		super.onCreate();
	}
	//当服务销毁的时候调用
	@Override
	public void onDestroy() {
		//当actvivity销毁的时候  取消注册广播接收者 
		unregisterReceiver(receiver);
		super.onDestroy();
	}
}
```