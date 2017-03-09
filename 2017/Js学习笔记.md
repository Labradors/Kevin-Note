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
```

### 函数声明

### 变量提升

- 函数内部赋值了变量，但是是在调用之后声明的，又有全局变量与之同名。那么打印的值为undefined。


### 函数参数

### 一些知识点

```javascript
$(id).className;更换类名
$(id).innerHtml;更换内容
// 自动获取焦点 
$(id).focus();
// 鼠标移动到输入框全选
$(id).onmouseover=function(){
	this.select();
}
docment.getElementsByTagName();
```

