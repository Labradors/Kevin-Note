# Js学习笔记

###  全局变量

- 在最外层声明的变量。
- 不使用var声明的变量。

### 事件三要素

`事件源.事件=function(){事件处理函数}`

### 注意事项

```html
javascript:;表示这是一个空连接。点击之后没任何反应。
类似的是#，但是一个#点击之后页面很长的情况下会会滚到顶部；而javascript:;没这样的问题；
当然###这样的效果就跟javascript:;一样了
eg:
<a href="###"></a>
<a href="javascript:;"></a>
js中input中输入的value为string类型，如果判断数字，需要转换
```

###  