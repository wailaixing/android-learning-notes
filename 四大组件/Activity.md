### Activity 相关

#### Activity的概念
是Android应用层开发的四大组件之一，主要负责和用户交互部分，有自己的生命周期，在其上可以布置按钮，文本框等各种控件，简单来说就是Android的UI部分。

Activity是Context的子类，同时实现了window.callback,keyevent.callback,可以处理与窗体用户交互的事件。

<br>

#### Activity与View的区别
1. Activity是四大组件中唯一一个用来和用户进行交互的组件。可以说Activity就是android的视图层。

2. 如果再细化，Activity相当于视图层中的控制层，是用来控制和管理View的，真正用来显示和处理事件的实际上是View。

3. 每个Activity内部都有一个Window对象， Window对象包含了一个DecorView(实际上就是FrameLayout)，我们通过setContentView给Activity设置显示的View实际上都是加到了DecorView中。

<br>
#### Activity生命周期
![framework](../image/activity1.png)

| 方法 | 描述 | 可否杀死 | 下一个 |
|-----|:-----|:-------:|-----:|
|onCreate()|在Activity第一次被创建的时候被调用。这里是你做所有初始化设置的地方—创建视图、绑定数据至列表等。如果曾经有状态记录（参阅后述Saving Activity State。），则调用此方法时会传入一个包含着此activity以前状态的包对象做为参数|否|onStart()|
|onStart()|在Activity正要变得为用户所见时被调用。数据刷新时候调用！！！当Activity转向前台时接着调用onResume()在Activity变为隐藏时接着知行onStop()|否|onResume()  onStop()|
|onResume|在Activity开始与用户进行交互之前被调用。此时Activity位于堆栈顶部，并接收用户输入。|否|onPause()|
|Activity running...|Activity 开始运行|||
|onPause()|当系统将要启动另一个 Activity时调用。此方法主要用来将未保存的变化进行持久化，停止类似动画这样耗费CPU的动作等。这一切动作应该在短时间内完成，因为下一个Activity必须等到此方法返回后才会继续。当Activity重新回到前台会执行onResume()【弹出对话框】。当Activity变为用户不可见时会执行onStop()。|是|onResume() onStop()|
|onStop()|当Activity不再为用户可见时调用此方法。这可能发生在它被销毁或者另一个Activity（可能是现存的或者是新的）回到运行状态并覆盖了它。如果Activity再次回到前台跟用户交互则行onRestart().如果关闭Activity则执行onDestroy()|是|onRestart() onDestroy()|
|onDestroy()|在Activity销毁前调用。这是Activity接收的最后一个调用。这可能发生在Activity结束（调用了它的finish()方法）或因为系统需要空间所以临时的销毁了此Activity的实例时。可以用isFinishing()方法来区分这两种情况。|是|完|
|||||
|onRestart()|在Activity停止后，在再次启动之前被调用。之后执行onStart().||onStart()|

<br>
#### Activity 四种启动模式

Task 打开一个Activity叫做进栈，关闭一个Activity叫做处出栈，任务是维护的Activity、操作的Activity永远是栈顶的Activity，程序退出任务栈被清空。

|启动模式|名称|作用|
|-------|:---:|--:|
|android:launchMode="singleTop"|单一顶部模式|如果任务栈的栈顶存在这个要开启的Activity，不会创建新的Activity，而是复用已经存在Activity。保证栈顶如果存在，不会重复创建。|
|android:launchMode="standard"|默认模式|可以不用写配置。在这个模式下，都会默认创建一个新的实例。因此，在这种模式下，可以有多个相同的实例，也允许多个相同Activity叠加。|
|android:launchMode="signleTask"|单一任务模式|当前任务栈中只有一个实例存在。当开启Activity的时候，就去检查任务栈里面是否已经存在，如果已经存在就复用，并把这个Activity上的所有的别的Activity都清空，复用这个已经存在的Activity。保证这个任务栈里只有一个实例存在。例： 浏览器的 activity|
|android:launchMode="singleInstance"||Activity会运行在自己的任务栈中，并且任务栈中只有一个实例存在，保证Activity在整个手机里只有一个实例存在  例： 来电页面|

#### 在配置文件中声明Activity
AndroidManifest.xml

```xml
    <manifest...>
      <application...>
        <activity android:name="xxx">
          ...
      </application...>
      ...
    </manifest>
```

可以给这个元素加入许多其他属性,如图标，名称或是activity的主题风格

```xml
    <activity android:allowTaskReparenting=["true" | "false"]
      android:alwaysRetainTaskState=["true" | "false"]
      android:clearTaskOnLaunch=["true" | "false"]
      android:configChanges=["mcc", "mnc", "locale",
                               "touchscreen", "keyboard", "keyboardHidden",
                               "navigation", "screenLayout", "fontScale", "uiMode",
                               "orientation", "screenSize", "smallestScreenSize"]
      android:enabled=["true" | "false"]
      android:excludeFromRecents=["true" | "false"]
      android:exported=["true" | "false"]
      android:finishOnTaskLaunch=["true" | "false"]
      android:hardwareAccelerated=["true" | "false"]
      android:icon="drawable resource"
      android:label="string resource"
      android:launchMode=["multiple" | "singleTop" |
                            "singleTask" | "singleInstance"]
      android:multiprocess=["true" | "false"]
      android:name="string"
      android:noHistory=["true" | "false"]  
      android:parentActivityName="string"
      android:permission="string"
      android:process="string"
      android:screenOrientation=["unspecified" | "user" | "behind" |
                                   "landscape" | "portrait" |
                                   "reverseLandscape" | "reversePortrait" |
                                   "sensorLandscape" | "sensorPortrait" |
                                   "sensor" | "fullSensor" | "nosensor"]
      android:stateNotNeeded=["true" | "false"]
      android:taskAffinity="string"
      android:theme="resource or theme"
      android:uiOptions=["none" | "splitActionBarWhenNarrow"]
      android:windowSoftInputMode=["stateUnspecified",
                                     "stateUnchanged", "stateHidden",
                                     "stateAlwaysHidden", "stateVisible",
                                     "stateAlwaysVisible", "adjustUnspecified",
                                     "adjustResize", "adjustPan"] >   

    </activity>
```

#### filter
`<activity>`也可以用很多`<intent-filter>`来指定其他的组件怎样激活它。

通过android SDK tools创建一个程序，主activity将自动包含一个被分类为“launcher”的`<intent filter>`

```xml
    <activity android:name="xxx" android:icon="@drawable/app_icon">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category Android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
```

`<action>`元素指定程序的入口。`<category>`指出该activity应该能让系统启动应用程序(允许用户启动activity)。

详见 ![intent](./intent.md)

****
#### 启动、关闭 Activity

Activity启动有两个方法：
> * `startActivity(Intent intent);`  //启动其他 Activity
> * `startSctivityForResult(Intent intent,int requestCode);`  //以指定的请求码(requestCode)启动Activity,而且程序会等到新启动Activity的结果(通过重写requestCode()来获取)。

**例：短信发送器**

(1 ) 写MainActivity页面:
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity" >

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content" >

        <EditText
            android:id="@+id/et_number"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="请输入要发送的号码 " />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignBottom="@id/et_number"
            android:layout_alignParentRight="true"
            android:onClick="click"
            android:text="+" />
    </RelativeLayout>

    <EditText
        android:id="@+id/et_content"
        android:layout_width="match_parent"
        android:layout_height="250dp"
        android:gravity="top"
        android:hint="请输入要发送的内容 " />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="insert"
        android:text="插入短信模板" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="send"
        android:text="发送" />

</LinearLayout>
```

(2 ) 通过ListView完成联系人页面
(3 ) 写MainActivity逻辑
```java
import android.app.Activity;
import android.content.Intent;
import android.view.View;
import android.widget.EditText;

public class MainActivity extends Activity {
	private EditText et_number;	
	private EditText et_content; 	
	@Override	
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);		
		setContentView(R.layout.activity_main);				                        //显示短信发送的内容		
		et_content = (EditText) findViewById(R.id.et_content);				        //显示号码 		
		et_number = (EditText) findViewById(R.id.et_number);		 			
	} 	
	//点击按钮 跳转到联系人页面  	
	public void click(View v){		
		//创建一个意图对象  跳转联系人页面 		
		Intent intent = new Intent(this,ContactActivity.class);		
		//如果你调用这个方法(startActivity())  就是简单开启页面  不能拿到返回的数据  注意：如果你想开启一个activity 并且想要获取开启的这个activity返回的数据 要调用这个方法 				
		startActivityForResult(intent, 1);
		//		startActivity(intent);	
	}		
	//点击按钮 实现发送短信短信的逻辑 	
	public void send(View v){		
		//TODO 一会实现 	
	}		
	
	//点击按钮 打开插入短信模板 页面	
	public void insert(View v){		
		Intent intent = new Intent(this,SmsTemplateActivity.class);		
		startActivityForResult(intent, 2);				
	}				 	
	
	//当这个activity(MainActivity) 开启联系人页面时   当联系人页面关闭的时候这个方法会调用	
	@Override	
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {				
		System.out.println("onActivityResult");				
		/*if (resultCode == 10) {			
		  //代表这个数据 是从联系人页面获取的 			
		 //(1)取出我们在联系人页面设置的数据  			
		 String phone = data.getStringExtra("phone");			
		 et_number.setText(phone);		
		 }else if (resultCode == 20) {			
		 //(2)把短信模板页面的数据显示到et_content控件上 			
		 String smscontent = data.getStringExtra("smscontent");			
		 et_content.setText(smscontent);								
		 }		
		 */				
		if (requestCode == 1) {			
			//代表请求联系人页面  			
			//(1)取出我们在联系人页面设置的数据  			
			String phone = data.getStringExtra("phone");			
			et_number.setText(phone);		
		}else if(requestCode == 2){			
			//请求短信模板页面 			
			//(2)把短信模板页面的数据显示到et_content控件上 			
			String smscontent = data.getStringExtra("smscontent");			
			et_content.setText(smscontent);		
		}						
		super.onActivityResult(requestCode, resultCode, data);	
	}	

}
```

(4 ) 写短信页面，通过一个ListView展示
```java
public class SmsTemplateActivity extends Activity { 		
	String objects[] = {"我在开会，请稍后在打...","我在吃饭，请稍后在打...","我在打酱油，请稍后在打...","我在约会，请稍后在打...","我在打麻将，请稍后在打..."};	
	@Override	
	protected void onCreate(Bundle savedInstanceState) {		
		super.onCreate(savedInstanceState);				
		//加载这个布局 		
		setContentView(R.layout.activity_smstemplate);				
		//(1)找到listview		
		ListView lv = (ListView) findViewById(R.id.lv);				
		//(2)把我们定义的好数据显示到listview上 		
		lv.setAdapter(new ArrayAdapter<String>(getApplicationContext(), R.layout.smstemlpate_item, objects));		
		//(3)给listview设置点击事件  		
		lv.setOnItemClickListener(new OnItemClickListener() { 			
			@Override			
			public void onItemClick(AdapterView<?> parent, View view,int position, long id) {				
				//(4)取出点击条目的数据 				
				String smscontent = objects[position];				
				//(5)当页面关闭的时候把数据返回 				
				Intent intent = new Intent();				
				intent.putExtra("smscontent", smscontent);				
				//(6)把数据返回给调用者  				
				setResult(20, intent);				
				//(7)关闭这个Activity 				
				finish();											
			}
		});
	}
}
```

(5 ) 实现短信发送
```java
	//(2)实现发送短信的功能   smsManager 获取这个类的实例		
	SmsManager smsManager = SmsManager.getDefault();				
	//(3)分割短信  分条发送 		
	ArrayList<String> divideMessages = smsManager.divideMessage(content);				
	for (String div : divideMessages) {			
	//（4）发送短信			
	smsManager.sendTextMessage(number, null, div, null, null);				
	}
```

**注：**
权限: `android:permission.SEND_SMS`

****
Activity关闭的两个方法：
> * `finish();`  //结束当前的Activity
> * `finishActivity(int requestCode);`  //结束以startActivityForResult(Intent intent,int requestCode);方法启动Activity

****
#### Activity间传递数据

Activity之间传递数据使用Intent，将数据放入Intent之中就可。Intent提供了了一些重载的方法：

> * `putExtra(Bundle data);`  //向Intent中放入需要传递的数据包
> * `Bundle getExtra();`  //取出Intent 中传递的数据包
> * `putExtra(String key,xxx value);`  //向Intent中放入key-value类型的数据
> * `getxxxExtra(String key);`  //从Intent中按key取出指定类型的数据

`Bundle`中存取数据：
> * `putXxx(String key,Xxx data);`  //向Bundle放入数据
> * `putSerializable(String key,Serializable data);`  //向Bundle中放入一个可序列化的对象
> * `getXxx(String key);`  //从Bundle中取出数据
> * `getSerializzble(String key,Serializable data);`  //从Bundle中取出一个可序列化的对象

**例:**

Intent中携带数据
```java
Person p = new Person(name.getText().toString(),passwd.getText().toString,gender); 
//创建一个Bundle对象
Bundle data = new Bundle();
data.putSerializable("person",p);
//创建一个Intent
Intent intent = new Intent(Test.this,Result.class);
intent.putExtra(data);
startActivity(intent);
```

接收数据
```java
//获取启动该Activity的Intent
Intent intent = getIntent();
//通过Intent取出所携带的Bundle中的数据
Person p = (Person)intent.getSerializableExtra("person");
String name = p.getName();
String passwd = p.getPasswd();
String sex = p.getGender();
```

****
#### 启动其他Avtivity并返回结果
Activity提供一个`startActivityForResult(Intent intent,int requestCode)`方法来启动其他的Activity。该方法启动指定Activity，而且获得指定的Activity返回的结果。
为了获取被启动的Activity所返回的数据，两方面:
> * 重写`onActivityResult(int requestCode,int resultCode,Intent intent);`，当被启动的Activity返回结果时，该方法被调用。`requestCode`代表请求码，`resultCode`代表Activity返回的结果码。
> * 被启动的Activity要调用`setResult()`，设置处理结果和`ResultCode`。

一个Activity可能需要多个`startActivityForResult()`开启多个Activity。当这些新Activity关闭后，系统会回调初Activity的`onActivityResult()`。为了知道这个方法是由哪个Activity触发的，可以利用`RequestCode`或者`ResultCode`，见上例 (短信发送器)。