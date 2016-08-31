### Activity 相关

#### Activity的概念
是Android应用层开发的四大组件之一，主要负责和用户交互部分，有自己的生命周期，在其上可以布置按钮，文本框等各种控件，简单来说就是Android的UI部分。

Activity是Context的子类，同时实现了window.callback,keyevent.callback,可以处理与窗体用户交互的事件。

#### Activity与View的区别
1. Activity是四大组件中唯一一个用来和用户进行交互的组件。可以说Activity就是android的视图层。

2. 如果再细化，Activity相当于视图层中的控制层，是用来控制和管理View的，真正用来显示和处理事件的实际上是View。

3. 每个Activity内部都有一个Window对象， Window对象包含了一个DecorView(实际上就是FrameLayout)，我们通过setContentView给Activity设置显示的View实际上都是加到了DecorView中。

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

    <manifest...>
      <application...>
        <activity android:name="xxx">
          ...
      </application...>
      ...
    </manifest>

可以给这个元素加入许多其他属性,如图标，名称或是activity的主题风格

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

#### filter
`<activity>`也可以用很多`<intent-filter>`来指定其他的组件怎样激活它。

通过android SDK tools创建一个程序，主activity将自动包含一个被分类为“launcher”的intent filter

    <activity android:name="xxx" android:icon="@drawable/app_icon">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category Android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>

`<action>`元素指定程序的入口。`<category>`指出该activity应该能让系统启动应用程序(允许用户启动activity)。

 