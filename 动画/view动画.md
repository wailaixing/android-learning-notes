### View动画

View动画有四种变换效果，透明、旋转、位移、缩放。分别对应Animation的四个子类:`AlphaAnimation`、`RotateAnimation`、`TranslateAnimation`、`ScaleAnimation`。既可以通过XML来定义，也可以通过代码动态创建.

<br>

#### View 动画的四种变换

|名称|标签|子类|效果|
|---|:---:|:---:|----:|
|透明动画|`<translate>`|`TranslateAnimation`|改变View的透明度|
|旋转动画|`<rotate>`|`RotateAnimation`|旋转View|
|位移动画|`<translate>`|`TranslateAnimation`|位移View|
|缩放动画|`<scale>`|`ScaleAnimation`|缩小放大View|

<br>

两种用法:

****

* 第一种：通过代码动态创建

1. 透明动画
 
```java
    AlphaAnimation aa=new AlphaAnimation(0,1);
    aa.setDuration(1000);
    view.stratAnimation(aa);
```

2. 旋转动画

```java
        RotateAnimation ra=new RotateAnimation(0,360,100,100);  //参数分别为 起始角度、终止角度、旋转中心点的坐标
        /* RotateAnimation ra=new RotateAnimation(0,360,RotateAnimation.RELATIVE_TO_SELF,0.5F,RotateAnimation.RELATIVE_TO_SELF,0.5F);
        *   设置旋转参考系为自身中心
        */
        ra.setDuration(1000);
        view.startAnimation(ra);
```

3. 位移动画

```java
        TranslateAnimation ta=new TranslateAnimation(0,200,0,300);
        ta.setDuration(1000);
        view.startAnimation(ta);
```

4. 缩放动画

```java
        ScaleAnimation sa=new ScaleAnimation(0,2,0,2);
        /* ScaleAnimation sa=new ScaleAnimation(0,2,0,2,Animation.RELATIVE_TO_SELF,0.5F,Animation.RELATIVE_TO_SELF,0,5F);
        * 缩放的中心点为自身中心
        */
        sa.setDuration(1000);
        view.startAnimation(sa);
```

动画集合：

通过AnimationSet,将动画以组合的形式展现:

```java
    AnimationSet as=new AnimationSet(true);
    as.setDuration(1000);

    AlphaAnimation aa=new AlphaAnimation(0,1);
    aa.setDuration(1000);
    as.addAnimation(aa);

    TranslateAnimation ta=new TranslateAnimation(0,100,0,100);
    ta.setDuration(1000);
    as.addAnimation(ta);

    view.startAnimation(as);
```

<br>

***

* 第二种： 通过XML文件创建

在 res/ 下创建anim文件夹，在anim下创建xml文件。

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <set xmlns:android="http://schemas.android.com/apk/res/android"
        android:interpolator="@[package:]anim/interpolator_resource"
        android:shareInterpolator=["true"|"false"] >
        <alpha
            android:fromAlpha="float"
            android:toAlpha="float" />
        <scale
            android:fromXScale="float"
            android:toXScale="float"
            android:fromYScale="float"
            android:toYScale="float"
            android:pivotX="float"
            android:pivotY="float" />
        <translate
            android:fromDegrees="float"
            android:toDegrees="float"
            android:pivotX="float"
            android:pivotY="float" />
        <set>
            ...
        </set>
    </set>
```

<set>标签代表动画集合，对应AnimationSet类，内部也可以嵌套其他动画集合

<br>
标签属性含义:

`android:interpolator`  动画集合所用的插值器，插值器是影响动画的速度

`android:shareInterpolator`  表示集合中的动画是否和集合共用一个插值器，若为false或集合没有插值器，子动画需要单独定义插值器或默认值

`android:duration`  表示动画持续的时间

`android:fillAfter`  表示动画结束以后View是否停留在结束位置

<br>

* `<alpha>` 透明动画，对应Alpha类。

        android:fromAlpha 动画的透明度起始值
        android:toAlpha   动画的透明度的结束值

* `<translate>` 平移动画，对应TranslateAnimation类

        android:fromXDelta 表示x的起始值
        android:toXDelta   表示x的结束值
        android:fromYDelta 表示y的起始值
        android:toYDelta   表示y的结束值

* `<rotate>` 旋转动画.对应RotateAnimation类

        android:fromDegrees  表示旋转开始的角度
        android:toDegrees    表示旋转结束的角度
        android:pivotX       表示旋转的中心的X坐标
        android:pivotY       表示旋转的中心的y坐标

* `<scale>` 缩放动画，对应ScaleAnimation类

        android:fromXScale  表示水平方向缩放的起始值
        android:toXScale    表示水平方向缩放的结束值
        android:fromYScale  表示垂直方向缩放的起始值
        android:toYScale    表示垂直方向缩放的结束值

使用方法:

```java
	Button button=(Button)findViewById(R.id.button);
	Animation anim=AnimationUtils.loadAnimation(this,R.anim.animation_button);
	button.startAnimation(anim);
```

****


#### 特殊的Animation

* layoutAnimation

layoutAnimation作用于ViewGroup，使其每个子元素都拥有动画效果。通常用于ListView.

用法:

1.定义一个LayoutAnimation

    //res/anim/anim_layout  

```xml
    <layoutAnimation
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:delay="0.5"  //表示子动画开始的延迟
        android:animationOrder="normal" //表示子动画顺序，选项分别为normal、random、reverse
        android:animation=@anim/anim_item" />
    </layoutAnimation>
```

2.为子元素指定相应的动画

    // res/anim/anim_item

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <set xmlns:android="http://schemas.android.com/apk/res/android"
        android:duration="1000"
        android:interpolator="@android:anim/accelerate_interpolator"
        android:shareInterpolator="true">
        <alpha
            android:fromAlpha="0.0"
            android:toAlpha="1.0"
            />
        <scale
            android:fromXScale="0.5"
            android:toXScale="1.5"
            />
    </set>
```

3.为ViewGroup指定android:layoutAnimaation属性。

```xml
    <ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layoutAnimation="@anim/anim_layout"
        />
```

除了XML外，也可以通过LayoutAnimationController来实现。

```java
    ListView listView=(ListView)findViewById(R.id.listview);
    Animation animation=AnimationUtils.loadAnimation(this,R.anim.anim_item);
    LayoutAnimationController controller=new LayoutAnimationController(animation);
    controller.setDelay(0.5f);
    controller.setOrder(LayoutAnimationController.ORDER_NORMAL);
    listView.setLayoutAnimation(controller);
```
    
* 使用View动画实现Activity切换效果

通过`overridePendingTransition(int enterAnim,int exitAnim)`来实现。该方法必须在`startActivity(intent)`和`finish()`之后调用才生效。

例：

```java
    Intent intent=new Intent(this,Test1Activity.class);
    startActivity(intent);
    overridePendingTransition(R.anim.enter_anim,R.anim_exit_anim);
```

```java
    @Override
    public void finish(){
        super.finish();
        overridePendingTransition(R.anim.enter_anim,R.anim.exit_anim);
    }
```
