---
title: CSS语法笔记
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/60.webp'
date: 2020-06-28 18:36:47
authorLink:
categories: 技术
tags: CSS
keywords:
description: 学了CSS，做点笔记
---
>对Web有点兴趣，可能它是可视化的，给我带来的成就感更多吧🐷标签不记了，w3school上都有

## 元素
CSS元素分为`块、行、行内块`三种元素，块元素会独占一行、行元素会紧凑着排列、而行内块就是综合两者在行内排列着块。
<ul>
  行内元素特征：<br>
  <li>设置宽高无效</li>
  <li>对margin仅设置左右方向有效，上下无效；padding设置上下左右都有效，即会撑大空间,行内元素尺寸，由内含的内容决定，盒模型中 padding, border 与块级元素并无差异，都是标准的盒模型，但是 margin，却只有水平方向的值，垂直方向并没有起作用。行内元素的水平方向的padding-left,padding-right,margin-left,margin-right 都产生边距效果，但是竖直方向的padding-top,padding-bottom,margin-top,margin-bottom都不会产生边距效果。padding设置上下左右都有效，即会撑大空间但是<em>不会产生边距效果</em>。</li>
  <li>不会自动进行换行</li>
  <br><br>
  块状元素特征: <br>
  <li>能够识别宽高</li>
  <li>margin和padding的上下左右均对其有效</li>
  <li>可以自动换行</li>
  <li>多个块状元素标签写在一起，默认排列方式为从上至下</li>
  <br><br>
  行内块状元素特征: <br>
  <li>不自动换行</li>
  <li>能够识别宽高</li>
  <li>默认排列方式为从左到右</li>
</ul>

以上三种元素可以通过`display: ???`属性切换类型。
## 类选择器
通配符: “ * ”<br>
通配符选择器会改变页面里面所有东西，注意是所有！！！

class类通过在css文件里面通过.加类名来该变HTML标签样式，id是"#"开头
```
CSS:
.name {
  ???;
}
#name {
  ???;
}
<p class="name">???</p>
<p id="name">???</p>
```
上面会改变所有类名为name的标签，有时候只想该变某个标签内部的p标签，可通过复合标签：
```
CSS:
div .name {???}
<div>
<p class="name">???</p> (1)
<div><p class="name">???</p></div>
</div>
这样选中了div下所有name，如果只想选(1)，可以：
div>.name {???}
```
也可以结合元素：
```
p.name {???}
```
选择器现在会匹配 class 属性包含 important 的所有 p 元素，但是其他任何类型的元素都不匹配，不论是否有此 class 属性。
多类选择器:
```
<p class="important warning"></p> //词的顺序无关紧要
```
通过把两个类选择器链接在一起，仅可以选择同时包含这些类名的元素（类名的顺序不限）。

如果一个多类选择器包含类名列表中没有的一个类名，匹配就会失败。请看下面的规则：
```
.important.urgent {background:silver;}
```
这个选择器将只匹配 class 属性中包含词 important 和 urgent 的 p 元素。
如果想使得包含imgportant或者urgent的可以将两者用“ , ”隔开
```
.important, .urgent {background:silver;}
```
## 选择器权重
当使用复合选择器的时候是有权重的，比如`div .p{}`&lt`.p{}`，这就是权重的叠加造成的，每个权重都不一样，这点了解就行了，毕竟我也不会🐶
## 悬停选择器
这个很重要，也很简单，比如
```
.name{background-color: blue}
.name:hover{background-color: red}
<div class="name"></div>
```
效果就是当鼠标停放在div块上时会切换到hover上，背景色变成红色
## 继承性
字面意思，子类会继承父类的属性

<font size=5 color="red">持续更新中...<font>