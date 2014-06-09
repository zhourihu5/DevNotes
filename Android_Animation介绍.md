# Android Animation介绍

Android平台提供了一套完整的动画框架，使得应用开发者可以用它来实现各种动画效果。比如：按扭的弹入弹出效果、Activity的切换动画、文本图片的旋转效果等。

Android平台的动画分三个部分，在Android 3.0版本以前支持两种动画，分别为补间动画（Tween Animation）和逐帧动画（Frame Animation）；在Android 3.0版本中新加入的动画叫属性动画（Property Animation）。下面分别介绍这三种动画。

## 一、Tween Animation（View Animation）

Tween Animation是通过对场景中的对象不断做图像变换（平移、缩放、旋转、改变透明度）产生动画效果，但是该动画只能应用于View对象（所以又叫`View Animation`），并且只支持一部分属性，如支持缩放旋转而不支持背景颜色的改变。该动画实现方式其实就是预先定义一组指令，这些指令指定了图形变换的类型、触发时间、持续时间。这些指令可以是以 XML 文件方式定义，也可以是以源代码方式定义。程序沿着时间线执行这些指令就可以实现动画效果。下面首先说明如何用XML来定Tween Animation：

### 1. 使用XML来定义Tween Animation

动画的XML文件在工程中**`res/anim`**目录，这个文件必须包含一个根元素，可以使`<alpha>``<scale>` `<translate>` `<rotate>`插值元素或者是把上面的元素都放入`<set>`元素组中，默认情况下，所以的动画指令都是同时发生的，为了让他们按序列发生，需要设置一个特殊的属性`startOffset`。动画的指令定义了你想要发生什么样的转换，当他们发生了，应该执行多长时间，转换可以是连续的也可以使同时的。例如，你让文本内容从左边移动到右边，然后旋转180度，或者在移动的过程中同时旋转，每个转换需要设置一些特殊的参数（开始和结束的大小尺寸的大小变化，开始和结束的旋转角度等等，也可以设置些基本的参数（例如，开始时间与周期），如果让几个转换同时发生，可以给它们设置相同的开始时间，如果按序列的话，计算开始时间加上其周期。

关于如何使用XML来定义见如下代码：

```
<?xml version="1.0" encoding="UTF-8"?>   
    <set xmlns:android="http://schemas.android.com/apk/res/android">   
       
        <!--    
            Tween Animation：通过对场景里的对象不断做图像变换(平移、缩放、旋转)产生动画效   
            Alpha：渐变透明度动画效果   
            Scale：渐变尺寸伸缩动画效果   
            Translate：画面转换位置移动动画效果   
            Rotate：画面旋转动画效果   
               
            Tween Animation 通用属性[类型]    功能     
                Duration[long]  属性为动画持续时间   时间以毫秒为单位   
                fillAfter [boolean] 当设置为true ，该动画转化在动画结束后被应用   
                fillBefore[boolean] 当设置为true ，该动画转化在动画开始前被应用   
                   
                interpolator    指定一个动画的插入器  有一些常见的插入器   
                accelerate_decelerate_interpolator   
                加速-减速 动画插入器   
                accelerate_interpolator   
                加速-动画插入器   
                decelerate_interpolator   
                减速- 动画插入器   
                其他的属于特定的动画效果   
                repeatCount[int]    动画的重复次数    
                RepeatMode[int] 定义重复的行为 1：重新开始  2：plays backward   
                startOffset[long]   动画之间的时间间隔，从上次动画停多少时间开始执行下个动画   
                zAdjustment[int]    定义动画的Z Order的改变 0：保持Z Order不变   
                1：保持在最上层   
                -1：保持在最下层 
         -->   
        <!--   
            透明控制动画    
         -->   
        <alpha   
            android:fromAlpha="0.1"    
            android:toAlpha="1.0"   
            android:duration="3000"   
        />   
               
        <!-- 尺寸伸缩动画效果 scale   
          
            属性：interpolator 指定一个动画的插入器   
       
            有三种动画插入器:   
             accelerate_decelerate_interpolator  加速-减速 动画插入器   
             accelerate_interpolator        加速-动画插入器   
             decelerate_interpolator        减速- 动画插入器   
       
            其他的属于特定的动画效果   
                fromXScale 属性为动画起始时 X坐标上的伸缩尺寸       
                toXScale   属性为动画结束时 X坐标上的伸缩尺寸        
                fromYScale 属性为动画起始时Y坐标上的伸缩尺寸       
                toYScale   属性为动画结束时Y坐标上的伸缩尺寸       
       
                说明:   
                     以上四种属性值       
                        0.0表示收缩到没有    
                        1.0表示正常无伸缩        
                        值小于1.0表示收缩     
                        值大于1.0表示放大   
                           
                pivotX     属性为动画相对于物件的X坐标的开始位置   
                pivotY     属性为动画相对于物件的Y坐标的开始位置   
                说明:   
                        以上两个属性值 从0%-100%中取值   
                        50%为物件的X或Y方向坐标上的中点位置   
            长整型值：   
                duration  属性为动画持续时间   
                说明:   时间以毫秒为单位   
       
            布尔型值:   
                fillAfter 属性 当设置为true ，该动画转化在动画结束后被应用   
        -->   
        <scale 
            android:interpolator="@android:anim/accelerate_decelerate_interpolator"   
            android:repeatCount="1"   
               
            android:fromXScale="0.5"   
            android:fromYScale="0.5"   
            android:toXScale="1.4"         
            android:toYScale="1.4"   
            android:pivotX="50%"   
            android:pivotY="50%"   
            android:fillAfter="false"   
            android:duration="3000"   
               
        />   
        <!--    
            画面转换位置移动动画效果 translate   
           
            fromXDelta toXDelta 为动画、结束起始时 X坐标上的位置      
            fromYDelta toYDelta 为动画、结束起始时 Y坐标上的位置   
         -->   
        <translate   
            android:repeatCount="2"   
            android:fromXDelta="-30"   
            android:fromYDelta="-30"   
            android:toXDelta="-80"         
            android:toYDelta="200"   
            android:duration="3000"   
        />   
        <!--    
            画面转移旋转动画效果 rotate   
               
            fromDegrees 为动画起始时物件的角度 说明   
                当角度为负数——表示逆时针旋转   
                当角度为正数——表示顺时针旋转   
                (负数from——to正数:顺时针旋转)   
                (负数from——to负数:逆时针旋转)   
                (正数from——to正数:顺时针旋转)   
                (正数from——to负数:逆时针旋转)   
                toDegrees   属性为动画结束时物件旋转的角度 可以大于360度   
            pivotX   
            pivotY  为动画相对于物件的X、Y坐标的开始位  说明：以上两个属性值 从0%-100%中取值   
            50%为物件的X或Y方向坐标上的中点位置   
         -->   
        <rotate   
            android:interpolator="@android:anim/accelerate_interpolator"   
            android:repeatCount="2"   
            android:fromDegrees="0"   
            android:toDegrees="+270"   
            android:pivotX="50%"   
            android:pivotY="50%"   
            android:duration="3000"   
        />   
</set>   
```

在JAVA程序中引用XML资源核心代码如下：

```
Animation mAnimation ;   mAnimation = AnimationUtils.loadAnimation(this, R.anim.anim);   TextView text = (TextView)findViewById(R.id.textview00);   text.setAnimation(mAnimation);   
```

### 2. 在JAVA代码中定义动画

核心代码如下：

```
private Animation myAnimationAlpha; 
private Animation myAnimationScale; 
private Animation myAnimationTranslate; 
private Animation myAnimationRotate; 

//根据各自的构造方法来初始化一个实例对象 
myAnimationAlpha = new AlphaAnimation(0.1f, 1.0f); 
myAnimationScale = new ScaleAnimation(0.0f, 1.4f, 0.0f, 1.4f, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f); 
myAnimationTranslate = new TranslateAnimation(30.0f, -80.0f, 30.0f, 300.0f); 
myAnimationRotate = new RotateAnimation(0.0f, +350.0f, Animation.RELATIVE_TO_SELF,0.5f,Animation.RELATIVE_TO_SELF, 0.5f);
```

### 3. 实现原理

Tween 动画是建立在View的级别上的，在 View 类中有一个接口 startAnimation 来使动画开始，startAnimation 函数会将一个 Animation 类别的参数传给 View，这个 Animation 是用来指定我们使用的是哪种动画，现有的动画有平移，缩放，旋转以及 alpha 变换等。如果需要更复杂的效果，可以将这些动画组合起来。

要了解 Android 动画是如何画出来的，首先要了解 Android 的 View 是如何组织在一起，以及他们是如何画自己的内容的。每一个窗口就是一棵 View 树，绘制整个窗口需要按顺序执行以下几个步骤：

1. 绘制背景；
2. 如果需要，保存画布（canvas）的层为淡入或淡出做准备；
3. 绘制 View 本身的内容，通过调用 View.onDraw(canvas) 函数实现，通过这个我们应该能看出来 onDraw 函数重载的重要性，onDraw 函数中绘制线条 / 圆 / 文字等功能会调用 Canvas 中对应的功能。
4. 绘制自己的子元素（通常也是一个 view 系统），通过 dispatchDraw(canvas) 实现，参看 ViewGroup.Java 中的代码可知，dispatchDraw->drawChild->child.draw(canvas) 这样的调用过程被用来保证每个子 View 的 draw 函数都被调用，通过这种递归调用从而让整个 View 树中的所有 View 的内容都得到绘制。在调用每个子 View 的 draw 函数之前，需要绘制的 View 的绘制位置是在 Canvas 通过 translate 函数调用来进行切换的，窗口中的所有 View 是共用一个 Canvas 对象
5. 如果需要，绘制淡入淡出相关的内容并恢复保存的画布所在的层（layer）
6. 绘制修饰的内容（例如滚动条），这个可知要实现滚动条效果并不需要 ScrollView，可以在 View 中完成的。
当一个 ChildView 要重画时，它会调用其成员函数 invalidate() 函数将通知其 ParentView 这个 ChildView 要重画，这个过程一直向上遍历到 ViewRoot，当 ViewRoot 收到这个通知后就会调用上面提到的 ViewRoot 中的 draw 函数从而完成绘制。View::onDraw() 有一个画布参数 Canvas, 画布顾名思义就是画东西的地方，Android 会为每一个 View 设置好画布，View 就可以调用 Canvas 的方法，比如：drawText, drawBitmap, drawPath 等等去画内容。每一个 ChildView 的画布是由其 ParentView 设置的，ParentView 根据 ChildView 在其内部的布局来调整 Canvas，其中画布的属性之一就是定义和 ChildView 相关的坐标系，默认是横轴为 X 轴，从左至右，值逐渐增大，竖轴为 Y 轴，从上至下，值逐渐增大 。

Android 动画就是通过 ParentView 来不断调整 ChildView 的画布坐标系来实现的，下面以平移动画来做示例，假设在动画开始时 ChildView 在 ParentView 中的初始位置在 (100,200) 处，这时 ParentView 会根据这个坐标来设置 ChildView 的画布，在 ParentView 的 dispatchDraw 中它发现 ChildView 有一个平移动画，而且当前的平移位置是 (100, 200)，于是它通过调用画布的函数 traslate(100, 200) 来告诉 ChildView 在这个位置开始画，这就是动画的第一帧。如果 ParentView 发现 ChildView 有动画，就会不断的调用 invalidate() 这个函数，这样就会导致自己会不断的重画，就会不断的调用 dispatchDraw 这个函数，这样就产生了动画的后续帧，当再次进入 dispatchDraw 时，ParentView 根据平移动画产生出第二帧的平移位置 (500, 200)，然后继续执行上述操作，然后产生第三帧，第四帧，直到动画播完。

用户可以定义自己的动画类，只需要继承 Animation 类，然后重载`applyTransformation`这个函数。对动画来说其行为主要靠差值点来决定的，比如，我们想开始动画是逐渐加快的或者逐渐变慢的，或者先快后慢的，或者是匀速的，这些功能的实现主要是靠差值函数来实现的，Android 提供了一个`Interpolator`的基类，你要实现什么样的速度可以重载其函数 getInterpolation，在 Animation 的 getTransformation 中生成差值点时，会用到这个函数。
从上面的动画机制的分析可知**某一个 View 的动画的绘制并不是由他自己完成的而是由它的父 view 完成，所以我们要注意上面 TextView 旋转一周的动画示例程序中动画的效果并不是由 TextView 来绘制的，而是由它的父 View 来做的。**findViewById(R.id.TextView01).startAnimation(anim) 这个代码其实是给这个 TextView 设置了一个 animation，而不是进行实际的动画绘制，代码如下 :
```
public void startAnimation(Animation animation) {
	animation.setStartTime(Animation.START_ON_FIRST_FRAME); 
	setAnimation(animation); 
	invalidate(); 
}
```
## 二、Frame Animation（Drawable Animation）

Frame Animation是顺序播放事先准备好的图像，类似于放电影。其实现方式比较简单，实现过程如下：在XML中的定义方式如下：

```
<animation-list xmlns:android=http://schemas.android.com/apk/res/android android:oneshot="true">
	    <item android:drawable="@drawable/pic1" android:duration="200" />
	    <item android:drawable="@drawable/pic2" android:duration="200" />
	    <item android:drawable="@drawable/pic3" android:duration="200" />
</animation-list>
```
必须以`<animation-list>`为根元素，以`<item>`表示要轮换显示的图片，`duration`属性表示各项显示的时间。XML文件要放在**`/res/drawable/`**目录下。

在JAVA中引用示例如下：

```
ImageView imageView = (ImageView) findViewById(R.id.imageView1);imageView.setBackgroundResource(R.drawable.drawable_anim);anim = (AnimationDrawable) imageView.getBackground();anim.start();
```
此处要注意一点：要用AnimationDrawable 的start()方法来启动动画，不管动画是否完毕，想要第二次启动动画一定要先调用它的stop()方法才可以再次启动动画。

## 三、Property Animation
Property动画是在Android 3.0中才引进的，它更改的是对象的实际属性，在Tween 动画中，其改变的是View的绘制效果，真正的View的属性保持不变，比如无论在对话中如何缩放Button的大小，Button的有效点击区域还是没有应用动画时的区域，其位置与大小都不变。而在Property 动画中，改变的是对象的实际属性，如Button的缩放，Button的位置与大小属性值都改变了。而且**Property 动画不止可以应用于View，还可以应用于任何对象。**Property 动画只是表示一个值在一段时间内的改变，当值改变时要做什么事情完全是自己决定的。

在Property Animation中，可以对动画应用以下属性：
- *Duration*：动画的持续时间- *TimeInterpolation*：属性值的计算方式，如先快后慢- *TypeEvaluator*：根据属性的开始、结束值与TimeInterpolation计算出的因子计算出当前时间的属性值- *Repeat Country and behavoir*：重复次数与方式，如播放3次、5次、无限循环，可以此动画一直重复，或播放完时再反向播放- *Animation sets*：动画集合，即可以同时对一个对象应用几个动画，这些动画可以同时播放也可以对不同动画设置不同开始偏移- *Frame refreash delay*：多少时间刷新一次，即每隔多少时间计算一次属性值，默认为10ms，最终刷新时间还受系统进程调度与硬件的影响
### 1、Property 动画的工作方式 

对于下图的动画，这个对象的X坐标在40ms内从0移动到40 pixel.按默认的10ms刷新一次，这个对象会移动4次，每次移动40/4=10pixel。

![Example of a linear animation](http://developer.android.com/images/animation/animation-linear.png)

也可以改变属性值的改变方法，即设置不同的interpolation，在下图中运动速度先逐渐增大再逐渐减小：

![Example of a non-linear animation](http://developer.android.com/images/animation/animation-nonlinear.png)

下图显示了与上述动画相关的关键对象

![How animations are calculated](http://developer.android.com/images/animation/valueanimator.png)

`ValueAnimator`即表示一个动画，包含动画的开始值，结束值，持续时间等属性。
ValueAnimator封装了一个`TimeInterpolator`，TimeInterpolator定义了属性值在开始值与结束值之间的插值方法。
ValueAnimator还封装了一个`TypeAnimator`，根据开始、结束值与TimeIniterpolator计算得到的值计算出属性值。
ValueAnimator根据动画已进行的时间跟动画总时间（duration）的比计算出一个时间因子（0~1），然后根据TimeInterpolator计算出另一个因子，最后TypeAnimator通过这个因子计算出属性值，如上例中10ms时：
- 首先计算出时间因子，即经过的时间百分比：t=10ms/40ms=0.25- 经插值计算(inteplator)后的插值因子:大约为0.15，上述例子中用了`AccelerateDecelerateInterpolator`，计算公式为（input即为时间因子）：`(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f`;  - 最后根据TypeEvaluator计算出在10ms时的属性值：0.15*（40-0）= 6pixel。上例中TypeEvaluator为FloatEvaluator，计算方法为 ：
```
public Float evaluate(float fraction, Number startValue, Number endValue) {
    float startFloat = startValue.floatValue();
    return startFloat + fraction * (endValue.floatValue() - startFloat);
}```
参数分别为上一步的插值因子，开始值与结束值。

### 2、ValueAnimator
ValueAnimator包含Property Animation动画的所有核心功能，如动画时间，开始、结束属性值，相应时间属性值计算方法等。应用Property Animation有两个步聚：

1. 计算属性值2. 根据属性值执行相应的动作，如改变对象的某一属性。
ValuAnimiator只完成了第一步工作，如果要完成第二步，需要实现ValueAnimator.onUpdateListener接口，如：```
ValueAnimator animation = ValueAnimator.ofFloat(0f, 1f);animation.setDuration(1000);animation.addUpdateListener(new AnimatorUpdateListener() {    @Override    public void onAnimationUpdate(ValueAnimator animation) {    }});animation.setInterpolator(new CycleInterpolator(3));animation.start();```
`Animator.AnimatorListener`中含有下面四种操作：
- onAnimationStart()- onAnimationEnd()- onAnimationRepeat()- onAnimationCancel()

`ValueAnimator.AnimatorUpdateListener`中有下面一个操作
- onAnimationUpdate()　　//通过监听这个事件在属性的值更新时执行相应的操作，对于ValueAnimator一般要监听此事件执行相应的动作，不然Animation没意义（可用于计时），而在ObjectAnimator（继承自ValueAnimator）中会自动更新属性，如无必要不必监听。在函数中会传递一个ValueAnimator参数，通过此参数的getAnimatedValue()取得当前动画属性值。

可以继承`AnimatorListenerAdapter`而不是实现AnimatorListener接口来简化操作，这个类对AnimatorListener中的函数都定义了一个空函数体，这样就只用定义想监听的事件而不用实现每个函数却只定义一空函数体。

```
ObjectAnimator oa=ObjectAnimator.ofFloat(tv, "alpha", 0f, 1f);oa.setDuration(3000);oa.addListener(new AnimatorListenerAdapter(){    public void on AnimationEnd(Animator animation){    }});oa.start();```
### 3、ObjectAnimator
ObjectAnimator继承自ValueAnimator，要指定一个对象及该对象的一个属性，当属性值计算完成时自动设置为该对象的相应属性，即完成了Property Animation的全部两步操作。实际应用中一般都会用ObjectAnimator来改变某一对象的某一属性，但用ObjectAnimator有一定的限制，要想使用ObjectAnimator，应该满足以下条件：

- 对象应该有一个setter函数：set<PropertyName>（驼峰命名法）- 如上面的例子中，像ofFloat之类的工场方法，第一个参数为对象名，第二个为属性名，后面的参数为可变参数，如果values…参数只设置了一个值的话，那么会假定为目的值，属性值的变化范围为当前值到目的值，为了获得当前值，该对象要有相应属性的getter方法：get<PropertyName>- 如果有getter方法，其应返回值类型应与相应的setter方法的参数类型一致。如果上述条件不满足，则不能用ObjectAnimator，应用ValueAnimator代替。

```
tv=(TextView)findViewById(R.id.textview1);btn=(Button)findViewById(R.id.button1);btn.setOnClickListener(new OnClickListener() {　　@Override　　public void onClick(View v) {　　　　ObjectAnimator oa=ObjectAnimator.ofFloat(tv, "alpha", 0f, 1f);　　　　oa.setDuration(3000);　　　　oa.start();　　}});```
上面的代码把一个TextView的透明度在3秒内从0变至1。

根据应用动画的对象或属性的不同，可能需要在onAnimationUpdate函数中调用invalidate()函数刷新视图。

### 4、通过AnimationSet应用多个动画
`AnimationSet`提供了一个把多个动画组合成一个组合的机制，并可设置组中动画的时序关系，如同时播放，顺序播放等。
以下例子同时应用5个动画：
1. 播放anim1；2. 同时播放anim2,anim3,anim4；3. 播放anim5。

```
AnimatorSet bouncer = new AnimatorSet();bouncer.play(anim1).before(anim2);bouncer.play(anim2).with(anim3);bouncer.play(anim2).with(anim4)bouncer.play(anim5).after(amin2);animatorSet.start();```
### 5、TypeEvalutors
根据属性的开始、结束值与TimeInterpolation计算出的因子计算出当前时间的属性值，android提供了以下几个evalutor：

- *IntEvaluator*：属性的值类型为int；- *FloatEvaluator*：属性的值类型为float；- *ArgbEvaluator*：属性的值类型为十六进制颜色值；- *TypeEvaluator*：一个接口，可以通过实现该接口自定义Evaluator。
自定义TypeEvalutor很简单，只需要实现一个方法，如FloatEvalutor的定义：
	public class FloatEvaluator implements TypeEvaluator {	    public Object evaluate(float fraction, Object startValue, Object endValue) {	        float startFloat = ((Number) startValue).floatValue();	        return startFloat + fraction * (((Number) endValue).floatValue() - startFloat);	    }	}

根据动画执行的时间跟应用的Interplator，会计算出一个0~1之间的因子，即evalute函数中的fraction参数，通过上述FloatEvaluator应该很好看出其意思。

### 6、TimeInterplator

time interplator定义了属性值变化的方式，如线性均匀改变，开始慢然后逐渐快等。在Property Animation中是TimeInterplator，在View Animation中是Interplator，这两个是一样的，在3.0之前只有Interplator，3.0之后实现代码转移至了TimeInterplator。Interplator继承自TimeInterplator，内部没有任何其他代码。

- AccelerateInterpolator　　　　　     加速，开始时慢中间加速- DecelerateInterpolator　　　 　　   减速，开始时快然后减速- AccelerateDecelerateInterolator　   先加速后减速，开始结束时慢，中间加速- AnticipateInterpolator　　　　　　  反向 ，先向相反方向改变一段再加速播放- AnticipateOvershootInterpolator　 反向加超越，先向相反方向改变，再加速播放，会超出目的值然后缓慢移动至目的值- BounceInterpolator　　　　　　　  跳跃，快到目的值时值会跳跃，如目的值100，后面的值可能依次为85，77，70，80，90，100- CycleIinterpolator　　　　　　　　  循环，动画循环一定次数，值的改变为一正弦函数：Math.sin(2 * mCycles * Math.PI * input)- LinearInterpolator　　　　　　　　  线性，线性均匀改变- OvershottInterpolator　　　　　　  超越，最后超出目的值然后缓慢改变到目的值- TimeInterpolator　　　　　　　　　 一个接口，允许你自定义interpolator，以上几个都是实现了这个接口

### 7、当Layout改变时应用动画

ViewGroup中的子元素可以通过setVisibility使其Visible、Invisible或Gone，当有子元素可见性改变时，可以向其应用动画，通过LayoutTransition类应用此类动画：`transition.setAnimator(LayoutTransition.DISAPPEARING, customDisappearingAnim);`通过setAnimator应用动画，第一个参数表示应用的情境，可以以下4种类型：

- APPEARING　　　　　　　 当一个元素变为Visible时对其应用的动画- CHANGE_APPEARING　　　 当一个元素变为Visible时，因系统要重新布局有一些元素需要移动，这些要移动的元素应用的动画- DISAPPEARING　　　　　　当一个元素变为InVisible时对其应用的动画- CHANGE_DISAPPEARING　 当一个元素变为Gone时，因系统要重新布局有一些元素需要移动，这些要移动的元素应用的动画 disappearing from the container.

### 8、KeyFrames
keyFrame是一个 *时间/值* 对，通过它可以定义一个在特定时间的特定状态，而且在两个KeyFrame之间可以定义不同的Interpolator，就相当多个动画的拼接，第一个动画的结束点是第二个动画的开始点。KeyFrame是抽象类，要通过ofInt(),ofFloat(),ofObject()获得适当的KeyFrame，然后通过PropertyValuesHolder.ofKeyframe获得PropertyValuesHolder对象，如以下例子：

```Keyframe kf0 = Keyframe.ofInt(0, 400);Keyframe kf1 = Keyframe.ofInt(0.25f, 200);Keyframe kf2 = Keyframe.ofInt(0.5f, 400);Keyframe kf4 = Keyframe.ofInt(0.75f, 100);Keyframe kf3 = Keyframe.ofInt(1f, 500);PropertyValuesHolder pvhRotation = PropertyValuesHolder.ofKeyframe("width", kf0, kf1, kf2, kf4, kf3);ObjectAnimator rotationAnim = ObjectAnimator.ofPropertyValuesHolder(btn2, pvhRotation);rotationAnim.setDuration(2000);
```
上述代码的意思为：设置btn对象的width属性值使其：1. 开始时 Width=4002. 动画开始1/4时 Width=2003. 动画开始1/2时 Width=4004. 动画开始3/4时 Width=1005. 动画结束时 Width=500
第一个参数为时间百分比，第二个参数是在第一个参数的时间时的属性值。
定义了一些Keyframe后，通过PropertyValuesHolder类的方法ofKeyframe封装，然后通过`ObjectAnimator.ofPropertyValuesHolder`获得`Animator`。用下面的代码可以实现同样的效果：
```ObjectAnimator oa=ObjectAnimator.ofInt(btn2, "width", 400,200,400,100,500);oa.setDuration(2000);oa.start();
```

### 9、Animating Views
在View Animation中，对View应用Animation并没有改变View的属性，动画的实现是通过其Parent View实现的，在View被drawn时Parents View改变它的绘制参数，draw后再改变参数invalidate，这样虽然View的大小或旋转角度等改变了，但View的实际属性没变，所以有效区域还是应用动画之前的区域，比如你把一按钮放大两倍，但还是放大这前的区域可以触发点击事件。为了改变这一点，在Android 3.0中给View增加了一些参数并对这些参数增加了相应的getter/setter函数（ObjectAnimator要用这些函数改变这些属性）：
- translationX,translationY:转换坐标（control where the View is located as a delta from its left and top coordinates which are set by its layout container.）- rotation,rotationX,rotationY:旋转，rotation用于2D旋转角度，3D中用到后两个- scaleX,scaleY:缩放- x,y:View的最终坐标（utility properties to describe the final location of the View in its container, as a sum of the left and top values and translationX and translationY values.）- alpha:透明度　
跟位置有关的参数有3个，以X坐标为例，可以通过getLeft(),getX(),getTranslateX()获得，若有一Button btn2，布局时其坐标为（40,0）：

```
//应用动画之前 btn2.getLeft();    //40 btn2.getX();    //40 btn2.getTranslationX();    //0//应用translationX动画 ObjectAnimator oa=ObjectAnimator.ofFloat(btn2,"translationX", 200);oa.setDuration(2000);oa.start();//应用translationX动画后btn2.getLeft();    //40btn2.getX();    //240btn2.getTranslationX();    //200```
- 无论怎样应用动画，原来的布局时的位置通过getLeft()获得，保持不变；
- X是View最终的位置；- translationX为最终位置与布局时初始位置之差。所以若就用translationX即为在原来基础上移动多少，X为最终多少，getX()的值为getLeft()与getTranslationX()的和。
对于X动画，源代码是这样的：
```
case X:       info.mTranslationX = value - mView.mLeft;       break;```
Property Animation也可以在XML中定义:
- `<set>` - AnimatorSet- `<animator>` - ValueAnimator- `<objectAnimator>` - ObjectAnimator
XML文件应放大**``/res/animator/``**中，通过以下方式应用动画：
```
AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext, R.anim.property_animator);
set.setTarget(myObject);
set.start();```

### 10、ViewPropertyAnimator
如果需要对一个View的多个属性进行动画可以用ViewPropertyAnimator类，该类对多属性动画进行了优化，会合并一些invalidate()来减少刷新视图，该类在3.1中引入。以下两段代码实现同样的效果：

```
PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat("x", 50f);PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat("y", 100f);ObjectAnimator.ofPropertyValuesHolder(myView, pvhX, pvyY).start();myView.animate().x(50f).y(100f);
```

## 总结
从上面的描述可以看出：

- View Animation：上手相对简单，能满足大部分基本的动画要求；
- Drawable Animaiton：使用简单，一般用于进度条的显示，需要准备大量的图片，大量使用会增加应用安装包的体积；
- Property Animation：使用最灵活、功能最强大的动画系统，但Android 3.0以后才可以使用。不过可以通过**[NineOldAndroids](https://github.com/JakeWharton/NineOldAndroids)**库来让3.0之前的系统版本也使用Property Animation的强大功能，只需要将import的`android.animation.*`包改成`com.nineoldandroids.animation.*`即可，更多细节请参考[http://nineoldandroids.com/](http://nineoldandroids.com/)。














