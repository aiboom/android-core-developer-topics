ConstraintLayout的目标是避免复杂布局视图的过度绘制。ConstraintLayout布局可以完全使用Android Studio的布局器实现。

## 如何在Android Studio中使用ConstriantLayout

首先在Project的build.gradle中做如下改变：
```
buildscript {
    repositories {
        google()
        ...
    }
    dependencies {
        ...
    }
}

allprojects {
    repositories {
        google()
         ...
    }
}
```
然后再App module的build.gradle文件中添加依赖：
```
dependencies {
    ...
    implementation 'com.android.support.constraint:constraint-layout:1.1.1'
}
```
## 了解ConstraintLayout布局的基本要素：
一个被布局的View相对位置关系使用`start`，`end`，`top`，`bottom`四个量取表征。

![image](https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/image_00.png)

ConstraintLayout在确定一个View的位置关系时基本上是通过上面四个量去实现的。此外还有基于角度的位置关系，我们会在接下来的小结里一一说明它们如何使用。

接下来我们来了解一下如何在Android Studio中简单实用ConstraintLayout。很简单，如下所示，使用鼠标连接要约束的关系即可（本例中建立了imageView的Start和parent的Start的约束，以及imageView的End和parent的End的约束）：

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto"src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_constraint_with_parent.webm" type="video/webm"/>
</html>

如上并不能建立一个具有完整约束关系的View，完整约束关系的View应该满足如下条件之一：
* start-end对，top-botom对都建立了约束
*  start-end，top-botom只有一对建立了约束，另一对中只有一个建立了约束并指定了对应的margin值
*  start-end对只有一个建立了约束，top-botom关系对中只有一个建立了约束，并指定了对应的margin值（详情可见项目中的imageView3）

否则会Android Studio会提示你相关的错误：


## 相对位置关系：
ConstraintLayout强调视图之间的约束关系，常见的约束关系就是视图的相对位置了，基于`start`，`end`，`top`，`bottom`四个要素，衍生出如下的位置关系属性：
```
layout_constraintLeft_toLeftOf
layout_constraintLeft_toRightOf
layout_constraintRight_toLeftOf
layout_constraintRight_toRightOf
layout_constraintTop_toTopOf
layout_constraintTop_toBottomOf
layout_constraintBottom_toTopOf
layout_constraintBottom_toBottomOf
layout_constraintBaseline_toBaselineOf
layout_constraintStart_toEndOf
layout_constraintStart_toStartOf
layout_constraintEnd_toStartOf
layout_constraintEnd_toEndOf
```
```xml
<Button android:id="@+id/buttonB" ...
                 app:layout_constraintLeft_toLeftOf="parent" />  <!--buttonB的左边和parent的左边建立约束关系 -->
```
如下是一个例子：

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_constraint_with_view.webm" type="video/webm"/>
</html>

```xml
<ImageView
        android:id="@+id/imageView2"
        ...
        app:layout_constraintEnd_toStartOf="@+id/imageView"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/imageView"
        app:srcCompat="@mipmap/ic_launcher_round" />
```
解释一下：
`app:layout_constraintEnd_toStartOf="@+id/imageView"`是第一步操作，使imageView2的End于imageView的Start建立约束；
`app:layout_constraintStart_toStartOf="parent"`是第二步操作，使imageView2的Start和parent的Start建立约束；
`app:layout_constraintTop_toTopOf="@+id/imageView"`是第三步操作，使imageView2的Top和imageView的Top建立约束；
建立约束的过程仅仅是约定约束边界的过程，相当于对被约束的View限定了其可布局的空间，如下所示，ImageView2的Top不能超过ImageView的Top，End不能超过ImageView的Start，Start不能超过parent的Start：

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_constraint_limit.webm" type="video/webm"/>
</html>


## Guideline的使用
您可以添加可限制视图的垂直或水平guideline，该guideline对应用用户不可见。您可以根据相对于边缘的dp单位或百分比在布局中定位guideline。

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_guideline.webm" type="video/webm"/>
</html>


我们看一下对应的代码：
```xml
    <android.support.constraint.Guideline
        android:id="@+id/guideline4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_begin="192dp" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent="0.5" />
```
`android:orientation="vertical | horizontal"`来确定Guideline的方向；
`app:layout_constraintGuide_begin`和`app:layout_constraintGuide_end`来确定起始边缘和结束边缘的距离；
`app:layout_constraintGuide_percent`来确定guideline距离起始距离的百分比。

> guideline的宽和高只能是`wrap_content`，不要去指定其它的值，那样做也没什么意义。

## Barrier的使用
与Guideline类似，Barrier是您可以限制视图的无形线条。Barrier没有自己的位置，而是基于其中包含的视图的位置而移动。 当您想要将视图限制为一组视图而非一个特定的视图时，这非常有用。
具体操作是：
* 点击上方Guidelines 按钮，选择合适的Barrier
* 在左下角的Component Tree中，选中要约束的View并拖入Barrier中

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_barrier.webm" type="video/webm"/>
</html>

我们观察代码：
```xml
<android.support.constraint.Barrier
        android:id="@+id/barrier4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:barrierDirection="left"
        app:constraint_referenced_ids="imageView6,imageView4"
        app:layout_constraintStart_toStartOf="parent" />
```
可见`app:constraint_referenced_ids="imageView6,imageView4"`就是将imageView6和imageView4添加到Barrier中的属性了；
`app:barrierDirection="left"`指定了约束类型，一共有六种，left-right，start-end，top-bottom。对应了四个边缘。

>  注意，Android Studio可能会提示错误，这个错误不必理会，强迫着患者可以在代码中添加类似位置约束来消除错误提示，不过你需要明白，位置约束以及margin参数对Barrier是没有效果的。

Barrier内的View还可以同Barrier建立约束，在需要对齐屏障内的View的情况下十分有用；

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_constraint_with_barrier.webm" type="video/webm"/>
</html>


或者将guideline加入barrier，这样可以给barrier一个“最小值”：

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_barrier_with_guideline.webm" type="video/webm"/>
</html>

## 合理使用右侧Attributes控制栏
我们可以看到，Attributes控制栏有许多丰富的功能，我们在这里一一认识一下：

![image](https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/image_01.png)

标记1，ratio属性，用来控制宽高比的，只有在宽和高不全为`wrap_content`时才有效果，至于为什么，我们会在介绍`match_constraint`时做说明；
对应的属性是：
```xml
app:layout_constraintDimensionRatio="w,2:1"
```
`w`代表比例的第一位代表宽，此时的2比1是宽和高的比，如果把`w`改为`h`,就代表着高和宽之比是2比1。
标记2，删除约束条件；
标记3，高和宽的模式，一共有三种模式，分别是`wrap_content` ，`fixed`，也就是固定大小以及`match_constraint` 。最后一个属性单独拿一个小节来讲。

标记4，margin属性，用来指定和约束边界的margin值，效果大家可以感受一下：

标记5，bias属性，是用来控制约束百分比的，效果如下，双击可以快速将值设为50：

对应的属性如下：
```xml
app:layout_constraintHorizontal_bias="0.41"
app:layout_constraintVertical_bias="0.39"
```
如果这两个属性被省略，则取默认值0.5。

## match_constraint

在ConstraintLayout中，不允许使用`match_parent`来表示宽和高的模式了，而由`match_constraint`取代。其原理是，将宽或高的值设为0，然后利用其它属性来设置宽和高的模式。宽和高的模式有如下三种：percent，spread，wrap

![image](https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/image_02.png)

在文件`example_match_constraint.xml`中，imageView7将宽的模式设置为了`match_constraint`的`percent`模式，也就是宽度是parent宽度的百分比：

```xml
android:layout_width="0dp"
app:layout_constraintWidth_default="percent" <!--设置模式，有三种模式-->
app:layout_constraintWidth_percent="0.5" <!--值为0.5，那么宽应该是parent的宽度的50%-->
```
在文件`example_match_constraint.xml`中，imageView8将宽的模式设置为了`match_constraint`的`spread`模式，也就是说，宽度是尽量展开的，这是默认的属性：

```xml
android:layout_width="0dp"
app:layout_constraintWidth_default="spread"
app:layout_constraintWidth_min="wrap" <!--设置压缩的最小值，为wrap_content-->
```
此外还有`app:layout_constraintWidth_max`属性，用来确定宽度的最大值。
在文件`example_match_constraint.xml`中，imageView9将宽的模式设置为了`match_constraint`的`wrap`模式，这和`wrap_content`效果是一样的。
```xml
android:layout_width="0dp"
app:layout_constraintWidth_default="wrap"
```
而文件`example_match_constraint.xml`中的imageView10则展示了在`match_constraint`模式下ratio属性对布局的影响。很显然，此时的宽度将由其和高度的比例决定。

## 线性组中的链式约束
在ConstraintLayout中来实现LinearLayout是十分简单的，我们需要借助双向约束，也就是线性约束来实现。

<html>
<video controls="controls" width="500" height="309" style="clear:both;display:block;margin:auto" src="https://raw.githubusercontent.com/aiboom/aiboom_top/master/constraint_layout/ice_video_build_chains.webm" type="video/webm"/>
</html>
