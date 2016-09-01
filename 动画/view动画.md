### View动画

View动画有四种变换效果，透明、旋转、位移、缩放。分别对应Animation的四个子类:AlphaAnimation、RotateAnimation、TranslateAnimation、ScaleAnimation。既可以通过XML来定义，也可以通过代码动态创建.

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
    
        AlphaAnimation aa=new AlphaAnimation(0,1);
        aa.setDuration(1000);
        view.stratAnimation(aa);

2. 旋转动画

        RotateAnimation ra=new RotateAnimation(0,360,100,100);  //参数分别为 起始角度、终止角度、旋转中心点的坐标
        /* RotateAnimation ra=new RotateAnimation(0,360,RotateAnimation.RELATIVE_TO_SELF,0.5F,RotateAnimation.RELATIVE_TO_SELF,0.5F);
        *   设置旋转参考系为自身中心
        */
        ra.setDuration(1000);
        view.startAnimation(ra);

3. 位移动画

        TranslateAnimation ta=new TranslateAnimation(0,200,0,300);
        ta.setDuration(1000);
        view.startAnimation(ta);

4. 缩放动画

        ScaleAnimation sa=new ScaleAnimation(0,2,0,2);
        /* ScaleAnimation sa=new ScaleAnimation(0,2,0,2,Animation.RELATIVE_TO_SELF,0.5F,Animation.RELATIVE_TO_SELF,0,5F);
        * 缩放的中心点为自身中心
        */
        sa.setDuration(1000);
        view.startAnimation(sa);

动画集合：

通过AnimationSet,将动画以组合的形式展现:

        AnimationSet as=new AnimationSet(true);
        as.setDuration(1000);

        AlphaAnimation aa=new AlphaAnimation(0,1);
        aa.setDuration(1000);
        as.addAnimation(aa);

        TranslateAnimation ta=new TranslateAnimation(0,100,0,100);
        ta.setDuration(1000);
        as.addAnimation(ta);

        view.startAnimation(as);

<br>

***

* 第二种： 通过XML文件创建

在 res/ 下创建anim文件夹，在anim下创建xml文件。

    <?xml version="1.0" encoding="utf-8"?>
