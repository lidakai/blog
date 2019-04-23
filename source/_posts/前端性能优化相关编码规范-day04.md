---
title:  前端性能优化相关编码规范
date: 2018-06-22 14:14:20
tags: 
    - 前端性能
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1556021941506&di=73676e7ba2dd679207b0dd74b905b3e2&imgtype=0&src=http%3A%2F%2Fwww.33lc.com%2Farticle%2FUploadPic%2F2012-10%2F2012101516255476188.jpg
categories: 
    - 大杂烩
---

### 目录导航

- [前端性能优化相关编码规范](#前端性能优化相关编码规范)
- [一. css编码规范](#一-css编码规范)
- [1.避免使用css表达式](#避免使用css表达式)
- [2.选择使用而不是@import](#选择使用而不是import)
- [3.避免使用Filter](#避免使用filter)
- [4.css选择符](#css选择符)
- [css选择符类型](#css选择符类型)
- [高效的使用css选择符](#高效的使用css选择符)
- [编写高效的css选择符](#编写高效的css选择符)

- [二. Javascript编码规范](#二-javascript编码规范)
- [1. 一些导致js性能缓慢的例子](#一些导致js性能缓慢的例子)
- [2. js陷阱](#js陷阱)
- [3. 加载性能一些可以提高的点：](#加载性能一些可以提高的点)
- [4. 鲜为人知的DOM](#鲜为人知的dom)
- [dom性能缓慢可以归结为一下3个原因：](#dom性能缓慢可以归结为一下个原因)
- [解决方案](#解决方案)

- [5. 面向对象的Javascript](#面向对象的javascript)
- [6. client-server对话](#client-server对话)
- [7. 动画](#动画)
- [8. 事件](#事件)
- [9. 样式](#样式)

# 前端性能优化相关编码规范

> 说明：本文主要介绍在使用HTML、CSS、JS的过程中，需要注意的编码细节和规范

---

## 一. css编码规范

### 1.避免使用css表达式

### 2.选择使用而不是@import

在IE中，使用@import相当于将放在页面底部。

### 3.避免使用Filter

使用filter的问题是它会阻塞渲染并且当浏览在加载图片时，会冻结浏览器。它也会提升内存消耗，它是作用于一个元素而不是一个图片，所以问题会更严重。

最好的解决方案是完全避免使用AlphaImageLoader，用PNG8来代替。

### 4.css选择符

css选择符的编写方式决定了浏览器必须执行的匹配次数，而某些类型的css选择符将会导致浏览器尝试更多匹配，因此开销比简单选择符更高。

#### **css选择符类型**

ID选择符：这种类型的选择符简单且高效，用于匹配页面唯一的元素。#id {}

类选择符.className {}

类型选择符tagName {}

相邻兄弟选择符H1 + #toc {}

子选择符#toc > li {}

后代选择符#toc A {}

通配选择符 * {}

属性选择符 [href=”#index”] {}

伪类和伪元素 A:hover {}

#### **高效的使用css选择符**

*最右边优先：事实上，css选择符是从右到左进行匹配的。*

##### 编写高效的css选择符

1. 避免使用通配规则：仅使用ID、类选择符
2. 不要限定ID选择符：ID选择符左边不用加任何其他选择符
3. 不要限定类选择符：不要用具体的标签来限定类选择符，而是根据实际情况对类名进行拓展。
4. 让规则越具体越好
5. 尽量避免使用后代选择符
6. 尽量避免使用标签做子选择符
7. 依靠继承
8. 仔细检查子选择符的用途

---

## 二. Javascript编码规范

### 1. 一些导致js性能缓慢的例子

- 
DOM访问

执行DOM交互的代码比i一般的js代码要慢。DOM交互是不可避免的，但是尽量的减少。例如，动态的使用innerHTML插入HTML语句比创建DOM节点更快。

- 
eval

无论什么时候，避免使用eval方法，因为执行这一方法会造成很大的开销。

- 
with

尽量不要使用with

- 
for-in 循环

---

### 2. js陷阱

1. 
避免使用eval和Function constructor

使用eval或者function constructor会加大开销因为每一次脚本引擎调用他们是必须将源码转换成可执行代码；

另外，使用eval时，字符串会在执行时被打断

![enter image description here](http://www.xuanfengge.com/netcool/images/performace1.png)

2. 
避免使用with

3. 
不要在对性能影响很大的地方使用try-catch-finally

![enter image description here](http://www.xuanfengge.com/netcool/images/performace2.png)

4. 
避免使用全局变量

![enter image description here](http://www.xuanfengge.com/netcool/images/performace3.png)

5. 
避免在对性能影响较大的地方使用for-in

for in循环需要脚本引擎构建一个枚举属性列表，而且每次都要从头重复检查

![enter image description here](http://www.xuanfengge.com/netcool/images/performace4.png)

6. 
使用字符串累加模式

![enter image description here](http://www.xuanfengge.com/netcool/images/performace5.png)

7. 
原生的操作比函数更快

![enter image description here](http://www.xuanfengge.com/netcool/images/performace6.png)

8. 
将函数而不是字符串传到setTimeout()和setInterval()中

![enter image description here](http://www.xuanfengge.com/netcool/images/performace7.png)

9. 
避免对象里没必要的DOM引用

![enter image description here](http://www.xuanfengge.com/netcool/images/performace8.png)

10. 
最大化对象解析速度，最小化作用域链

![enter image description here](http://www.xuanfengge.com/netcool/images/performace9.png)

11. 
尽量让脚本声明的变量短一点，不要太长，特别是循环里面的变量

![enter image description here](http://www.xuanfengge.com/netcool/images/performace10.png)

12. 
将自身引用存储在作用域外的变量中

当一个funciton被执行时，一个执行的上下文（context）会被创建，被激活的对象将所有自身变量push到上下文（context）的作用域链之前。

离作用域链越远，解析的越慢，这也意味着作用域本地的变量时解析的最快的。
将自身引用用作用域之外的变量存储，读写都会变的更快，这在全局变量和一些深度查找的资源解析中特别明显。

当然，作用域内定义的变量比使用对象自身访问的速度更快。

![enter image description here](http://www.xuanfengge.com/netcool/images/performace11.png)

假如你需要在一个大循环中访问一个dom，这样子会更快：

![enter image description here](http://www.xuanfengge.com/netcool/images/performace12.png)

![enter image description here](http://www.xuanfengge.com/netcool/images/performace13.png)

![enter image description here](http://www.xuanfengge.com/netcool/images/performace14.png)

---

### 3. 加载性能一些可以提高的点：

1. 
更快的加载和展现页面可以让js加载没有阻塞

![enter image description here](http://www.xuanfengge.com/netcool/images/performace15.png)

2. 
添加Experes或者Cache-Control HTTPheader

3. 
Gzip javascript和css资源

4. 
利用YUI或者JSMin压缩代码

5. 
尽量减少资源的数目和大小

6. 
让脚本无阻赛并行下载

7. 
合并异步加载的脚本

8. 
将行内脚本放到样式表之上？有待考证，不科学

9. 
少用iframe

---

### 4. 鲜为人知的DOM

#### dom性能缓慢可以归结为一下3个原因：

- 大规模的DOM操作 
- 脚本触发太多的重构和重绘
- 定位节点在DOM中的路径慢

#### 解决方案

1. 
尽可能减小DOM的大小

2. 
使用文档的组件模板来进行复用

动态的在dom中插入或者更新元素是代价很大的。一个有效的方法来解决这个问题是利用HTML模板来插入一些对话框或者UI组件。

3. 
最小化重绘和重构的次数

重绘发生在一些元素可见或者隐藏的时候，但是没有改变document的布局

重构发生在DOM的操作方式影响到布局。

重构的代价比重绘大的多得多。

重构表格比重构块状元素代价大

绝对定位的元素不会对document的布局产生影响

DOM的修改会触发重绘

**参考资料**：

- [Repaint and reflow at Opera Developer Network](http://dev.opera.com/articles/view/efficient-javascript/?page=3#reflow)
- [Notes on HTML Reflow](http://www-archive.mozilla.org/newlayout/doc/reflow.html) - more detailed information on the reflow process (archived)
- [Reflows & Repaints: CSS Performance making your JavaScript slow](http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow/)
- [Go With The Reflow](http://www.slideshare.net/lsimon/go-with-the-reflow)
- [Rendering: repaint, reflow/relayout, restyle](http://www.phpied.com/rendering-repaint-reflowrelayout-restyle/)

![enter image description here](http://www.xuanfengge.com/netcool/images/performace16.png)

4. 
利用cloneNode()

![enter image description here](http://www.xuanfengge.com/netcool/images/performace17.png)

5. 
利用HTML模板和innerHTML

6. 
设定元素不可见再进行改变（质疑，appendChild不会重发重绘，js执行完成以后才会）

![enter image description here](http://www.xuanfengge.com/netcool/images/performace18.png)

7. 
尽量少的使用改变元素尺寸或位置的操作

![enter image description here](http://www.xuanfengge.com/netcool/images/performace19.png)

8. 
使用className来完成多个预定义样式的的改变

![enter image description here](http://www.xuanfengge.com/netcool/images/performace20.png)

9. 
利用设定属性来动态完成多个样式的设定

![enter image description here](http://www.xuanfengge.com/netcool/images/performace21.png)

10. 
css class name vs. style属性

![enter image description here](http://www.xuanfengge.com/netcool/images/performace22.png)

11. 
不要遍历大量的节点，避免在遍历时改变dom结构

12. 
将DOM元素缓存在变量中使用

![enter image description here](http://www.xuanfengge.com/netcool/images/performace23.png)

13. 
在dom元素使用完以后移除引用

![enter image description here](http://www.xuanfengge.com/netcool/images/performace24.png)

---

### 5. 面向对象的Javascript

1. 考虑使用继承机制

---

### 6. client-server对话

1. 
对XMLHttpRequest设定超时时间

2. 
考虑使用约定的数据来做大数据的处理，比如选择xml或者json

---

### 7. 动画

1. 
选择性的使用动画

2. 
使用scrollTo()方法来实现滚动动画

3. 
将动画元素的position设置为absolute或者fix

4. 
在同一时间使用一个timer来服务多个动画元素

![enter image description here](http://www.xuanfengge.com/netcool/images/performace25.png)

5. 
让动画的速度更平滑

---

### 8. 事件

1. 
利用事件冒泡

![enter image description here](http://www.xuanfengge.com/netcool/images/performace26.png)

2. 
不要对一些经常触发的event使用代理

![enter image description here](http://www.xuanfengge.com/netcool/images/performace27.png)

3. 
Javascript调用栈使用setTimeout不会溢出

原因是setTimeout是伪异步的，把函数交给setTimeout处理后，原来的函数不会等待，会继续执行，函数会结束，资源也就可以释放。

而不用setTimeout的时候，函数必须等待调用的函数返回后才能继续执行，但调用的函数又必须等待下一级函数，这样所有函数都不能结束，资源也就释放不出。

换句话说，就是死锁。

---

### 9. 样式

1. 
优化css

2. 
优化图片

---
