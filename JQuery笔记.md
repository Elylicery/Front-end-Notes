# 1. JQuery入门

## 1.1 JQuery简介

**jQuery是什么？**

* JQuery(write less,do more)
* JQuery是一个**快速，小巧且功能丰富的Javascript库**。它使HTML文档的遍历和操作，事件处理，动画和Ajax等操作变得更加简单，并提供了一个易于使用的API，可以跨多种浏览器工作
* jQuery集多功能性和可扩展性于一身，改变了数百万人编写JavaScript的方式

**jQuery的作用**

* HTML元素选取
* HTML元素操作
* HTML事件函数
* HTML DOM遍历和修改
* CSS操作
* Js特效和动画
* AJAX
* Utilities

**JQuery的目的**

* 简化代码，使程序更高效
* 用来替代原生js的

**学前准备**

* html
* css
* js及其DOM操作

**jQuery的优势**

* 轻量级
* 强大的选择器
* 出色的DOM操作及其封装
* 可靠得事件处理机制
* 完整的Ajax
* 不污染顶级变量
* 出色的浏览器兼容性
* 隐式迭代
* 行为层和结构层的分离
* 丰富得插件支持

## 1.2 JQuery的使用

**JQuery的引入与 $**

**引入：**

* 在页面`<head>`中引入
* 使用npm，yarn。。等引入

**JQuery符号**

jQuery把所有功能全部封装在一个全局变量jQuery中，`$`是变量jQuery的别名

**JQuery的书写格式**

```js
$ //jQuery(selector,context)
```

# 2. JQuery选择器

详细看教辅把

## 2.1 选择器简介

## 2.2 基本选择器

* #id
* element
* .class
* *



## 2.3 多项选择器

* $("selector1,selector2,seletcorN")
* 将每一个选择器匹配到的元素合并后一起返回

## 2.4 层级选择器

* $('ancestor descentdant')
* $('parent> child')
* $('prev+next')
* $('prev ~ siblings')

## 2.5 属性选择器

* [attribute]
* [attribute=value]
* [attribute!=value]
* [attribute^=value] 属性值以value**开头**的
* [attribute$=value] value作为**结尾**的所有属性元素
* [attribute*=value] 属性值**包含**value即可
* `[selector1][selector2][slectorN]`

## 2.6 过滤器

* first-child
* last-child
* .....

## 2.7 表单相关选择器

* :input
* :text
* :radio/:checkbox/:image...

## 2.8 查找与过滤

* find(xpr|Object|element)
* children([expr])
* parent([expr])
* next([expr]),prev([expr])
* eq(index|-index)
* siblings([expr])
* filter(expr|ibject|element|fn)
* not(expr|element|fn)
* first() last() slice(start,[end])



# 3. JQuery事件

## 3.1 介绍

html加载完才能正确获取元素

```js
window.onload(funtion(){
//...
}))

//JQuery用的是ready
```



## 3.2 鼠标事件（最常用）

* click，dbclick
* mouseenter，mouseleave，mousemove
* hover([over],out)
* mousedown,mouseup
* scroll([data],fn)

## 3.3 键盘事件

* keydown
* keyup
* keypress

## 3.4 其他事件

* ready(fn)
* focus
* resize
* change
* select()
* submit

## 3.5 事件参数

* event
* 所有事件都会掺入event对象作为参数，可以从event对象上获取到更多的信息

## 3.6 事件绑定与取消

* on(events,[selector],[data],fn)
* off(events,[selector],[fn])
* one 一次性的事件处理函数

# 4. JQuery扩展

## 4.1 动画

* 动画DOM及其CSS操作
* 自定义动画
* 动画函数
* 计时器





