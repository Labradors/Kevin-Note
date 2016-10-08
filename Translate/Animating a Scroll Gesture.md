# Animating a Scroll Gesture
> [Animating a Scroll Gesture](https://developer.android.com/training/gestures/scroll.html)

在android中，`ScrollView`是滚动的典型实现。在任何平台布局中，内容都有可能超出本身容器的边界。Android框架来管理一个可嵌套的`ScrollView`。在特殊的场景下，实现一个自定义的滚动器是必要的。
用滚动器来显示一个可滚动的效果，用来响应一个触摸手势。你可以用滚动器去收集数据。你需要创造一个滚动动画来响应你的触摸事件。`Scroller`和`OverScroller`它们是相似的。但是`OverScroller`指向了用户平易或者惯性所到达的内容边缘区域。`InteractiveChart`案例用`EdgeEffect`来显示一个用户到达内容边缘之后屏幕发光的效果。<br>
> Note:注意，对于滑动的效果，我们推荐你使用`OverScroller`,`OverScroller`对于较老的设备使用了较好的向下兼容性。当然，一般情况下，你的view知识需要一个滑动的功能，那只需要使用`scrollers`即可。如果你只需要一个嵌套的滚动布局，`ScrollView `和`HorizontalScrollView `即可满足你的所有需求。

一个滚动器被用来实现滚动完成之后的动画。用作标准平台的物理滚动。这个滚动器事实上并没有做任何事。滚动器只是用来跟踪滚动结束后的偏移量。但是他不自动应用这些位置到你的应用中，他的责任是平滑的应用这些动画到一个新的坐标。

## Understand Scrolling Terminology

滚动在Android拥有不同的含义，这个依赖于上下文。
