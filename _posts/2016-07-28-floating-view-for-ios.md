---
layout: post
title: "Floating View for iOS"
date: 2016-07-28 17:49:19 +0800
description: "Floating View"
category: iOS
tags: []
---
### 1 Hidden View ###

<pre>
view.alpha = 0.f;
</pre>

<pre>
view.backgroundColor = [UIColor clearColor];
</pre>

<pre>
view.hidden = YES;
</pre>

#### 1.1 alpha ####

液晶显示器是由一个个的像素点组成的，每个像素点都可以显示一个由RGBA颜色空间组成的一种色值。其中的A就表示透明度alpha，UIView中alpha是一个浮点值，取值范围0~1.0,表示从完全透明到完全不透明。
当把alpha的值设置成0以后：

- 当前的UIView和subview都会被隐藏，而不管subview的alpha值为多少。
- 当前UIView会从响应者链中移除，而响应者链中的下一个会成为第一响应者

alpha的默认值是1.0。

另外，更改alpha值时，默认是有动画效果的，这是因为图层在Cocoa中是由Core Animation中CALayer表示的，该动画效果是CALayer的隐含动画。

#### 1.2 hidden ####

该属性为BOOL值，用来表示UIView是否隐藏，默认值是NO。

当值设为YES时:

- 当前的UIView和subview都会被隐藏，而不管subview的hidden值为多少。
- 当前UIView会从响应者链中移除，而响应者链中的下一个会成为第一响应者

总之，同alpha为0时的显示效果相同。

#### 1.3 opaque ####
<pre>
R = S + D * ( 1 – Sa )
</pre>

其中，R表示混合结果的颜色，S是源颜色(位于上层的红色图层一)，D是目标颜色(位于下层的绿色图层二)，Sa是源颜色的alpha值，即透明度。公式中所有的S和D颜色都假定已经预先乘以了他们的透明度。

知道图层混合的基本原理以后，再回到正题说说opaque属性的作用。当UIView的opaque属性被设为YES以后，按照上面的公式，也就是Sa的值为1，这个时候公式就变成了：
<pre>
R = S
</pre>
即不管D为什么，结果都一样。因此GPU将不会做任何的计算合成，不需要考虑它下方的任何东西(因为都被它遮挡住了)，而是简单从这个层拷贝。这节省了GPU相当大的工作量。由此看来，opaque属性的真实用处是给绘图系统提供一个性能优化开关！

按照前面的逻辑，当opaque属性被设为YES时，GPU就不会再利用图层颜色合成公式去合成真正的色值。因此，如果opaque被设置成YES，而对应UIView的alpha属性不为1.0的时候，就会有不可预料的情况发生，这一点苹果在官方文档中有明确的说明：

An opaque view is expected to fill its bounds with entirely opaque content—that is, the content should have an alpha value of 1.0. If the view is opaque and either does not fill its bounds or contains wholly or partially transparent content,the results are unpredictable. You should always set the value of this property to NO if the view is fully or partially transparent.


### 2 Transparent view vs. hidden view ###

#### 2.1 Rise the question

Here comes the question,

>  **how to create a transparent view without any contents?** 

Wow, the answer looks so easy to get! Almost every iOS developer has created one or even more ‘transparent view’s. Let’s take a look at those solutions. We have 2 solutions.

##### Solutions #1
Create a view with alpha=0.f. take a look at the code below
<pre>
UIView *transparentView1 = [[UIView alloc] initWithFrame:CGRectMake(0.f, 0.f, 320.f, 460.f)];
transparentView1.alpha = 0.f;
[self.view addSubview:transparentView1];
[transparentView1 release];
</pre>
It’s simple and straight forward. Hum? Let’s take a look at another solution.

##### Solution #2
Create a view with backgroundColor=[UIColor clearColor].
<pre>
UIView *transparentView2 = [[UIView alloc] initWithFrame:CGRectMake(0.f, 0.f, 320.f, 460.f)];
transparentView2.backgroundColor = [UIColor clearColor];
[self.view addSubview:transparentView2];
[transparentView2 release];
</pre>
This is also very simple.

So, what’s the difference between these 2 solutions?

#### 2.2 The difference on surface
Before we talking about the difference, we need to review some basic yet key properties provided by iOS

- hidden: set this property to YES will cause the view invisible. default is NO;
- alpha: this property affect all the contents of the view, including drawing and subviews. animatable;
- backgroundColor: this only affect the background color, not affect the drawing and subviews of the view. animatable;
- opaque: set this property to YES, will gain some performance boost on UI rendering. But need to full fill the rectangle of the view. If opaque is set to YES and the view has full or partial transparent content, the behavior is not define.

> Visit apple’s document for UIView for detail information.

With these basic knowledge, we can easily tell the difference between these 2 solutions.

- Solution #1 uses alpha property to make the view transparent, in another word, it’s invisible because the view is hidden.
- Solution #2 sets backgroundColor to transparent color to make it invisible.
- Solution #2 only set the background to transparent, if you draw anything on the view, or add some subviews to the view, they are still visible, and solution #1 will make everything in the view invisible, including the custom drawings and all the subviews.
- If the view is a clean view without any custom drawings and subviews, the result of these 2 solutions look the same.
They look the same? But actually not the same? Yes, they just look the same, but not exactly the same, so what’s the real difference?

#### 2.3 Look the same, but…
There is always a BUT.

These 2 solutions look the same, but act differently on receiving touch event.

- Solution #1: alpha=0.f;, according to Apple’s document:
To hide a view visually, you can either set its hidden property to YES or change its alpha property to 0.0. A hidden view does not receive touch events from the system. However, hidden views do participate in autoresizing and other layout operations associated with the view hierarchy. Thus, hiding a view is often a convenient alternative to removing views from your view hierarchy, especially if you plan to show the views again at some point soon.

See Apple’s document

- Solution #2: when background color is set to clear color, the alpha is still 1.0, so this view still can receive touch event.

#### 2.4 Finally, we have the answer
Before we get the correct answer, we need to use the correct terminologies to clarify the answer:

Hidden View: when hidden=YES or alpha=0.f
Transparent background View: when backgroundColor is set to clear color
##### The answer
- a hidden view will not accept touch event, while a transparent background view will.
- a hidden view may look like a transparent background view if add no subviews and draw nothing on the view.
