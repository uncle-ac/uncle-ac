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

## 文章转载
原文链接：[Click me](https://www.antmoe.com/posts/fd1c8f75/index.html)
本人也对其做了少些修改

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
## 选择器分类

- 基本选择器:  共有 5 个基本选择器，是 CSS 选择器的最为基本的用法。
- 层级选择器:  共有 4 个层级选择器。
- 组合选择器：具有交集和并集两种用法，是将之前基本选择器和层级选择器进行组合。
- 伪类选择器：允许未包含在 HTML 页面中的状态信息选定位 HTML 元素。
- 伪元素选择器：定位所有未被包含 HTML 的实体。

## 基本选择器

1.  类型选择器（元素选择器）
       ```
       div {
            font: 12px;
           }
       ```
2.  类（Class）选择器
	类选择器前面是“ . ”，类名对应HTML中的class类 
       ```
       .demo {
          color: lightcoral;
          font-size: 24px;
        }
       ```
3.	ID 选择器
	id选择器前面是“ # ”，`一个 html 文件中 id 只允许出现一次`
        ```
        #Demo {
          color: lightcoral;
          font-size: 24px;
        }
        ```
4.	通配符“ * ”，为HTML中所有元素添加样式
        ```
        * {
          color: lightcoral;
        }
        ```
5.	属性选择器
	
	* [attr] 属性选择器：通过 HTML 元素的 attr 属性名来定位具体 HTML 元素，把所有title属性的元素全部改变。
        ```
        [title] {
            color="red";
        }
        ```
	* [标签][属性]：将所有特定标签加油特定属性的元素添加样式
        ```
        a[href] {color:red;} //对所有加有href的a标签添加样式
        ```
	* [标签][属性1][属性2][...]：将同时加有属性1、属性2...的特定标签添加样式
        ```
        a[href][title]  {color:red;}
        ```
    * 根据具体属性值选择
    除了选择拥有某些属性的元素，还可以进一步缩小选择范围，只选择有特定属性值的元素。
        ```
        a[href="http://www.w3school.com.cn/about_us.asp"] {color: red;}
        ```
    * 根据多个具体属性选择
        ```
        a[href="http://www.w3school.com.cn/"][title="W3School"] {color: red;}
        ```
    * 选择 titile 属性包含单词 "flower" 的元素，并设置其样式：
        ```
        [title~=flower]
        { 
            background-color:yellow;
        }
        <p title="flower nb">添加成功</p>
        <p title="nb">添加失败</p>
        ```
    * 选择 lang 属性值以 "en" 开头的元素，并设置其样式：
        ```
        [lang|=en]
        { 
        background-color:yellow;
        }
        ```
    * 设置 class 属性值以 "test" 开头的所有 div 元素的背景色：
        ```
        div[class^="test"]
        {
        background:#ffff00;
        }
        ```
	* 设置 class 属性值以 "test" 结尾的所有 div 元素的背景色：
        ```
        div[class$="test"]
        {
            background:#ffff00;
        }
        ```
	* 设置 class 属性值包含 "test" 的所有 div 元素的背景色：
        ```
        div[class*="test"]
        {
            background:#ffff00;
        }
        ```
## 优先级
>优先级就是分配给指定的 CSS 声明的一个权重，它由匹配的选择器中的每一种选择器类型的数值决定。

例如：a.name权重等于a标签权重加上name类名的权重和，其权重大于.name，所以两者同时存在时前者样式生效

<font color="red" size=3>权重相同，<b>后来者居上</b></font>

<font size=4>!important提高到最高优先级</font>
```
    div {
      color: blue !important;
    }
    .demo {
      color: red;
    }
    <div class="demo">这是一个测试内容.</div> //最终的颜色为 blue
```
## 层级选择器
### 层级选择器
```
  <div id="ancestor1">
    <div id="parent1">
      <div id="child11"></div>
      <div id="child12"></div> 

    </div>
    <div id="parent2">
      <div id="child2"></div>
    </div>
  </div>
  <div id="ancestor2"></div>
```
{% fb_img https://tva1.sinaimg.cn/large/832afe33ly1gainbqrpuuj21jj0qi763.jpg %}
* 兄弟元素：ancestor1 元素和 ancestor2 元素、parent1 元素和 parent2 元素，以及 child11 元素和 child12 元素。
* 父级与子级元素：
	* 如果 `` 元素是父级元素的话，那 ancestor1 元素和 ancestor2 元素就是子级元素。
	* 如果 ancestor1 元素是父级元素的话，那 parent1 元素和 parent2 元素就是子级元素。
	* 如果 parent1 元素是父级元素的话，那 child11 元素和 child12 元素就是子级元素。
* 祖先与后代元素：
	* 如果 `` 元素是祖先元素的话，那其包含的所有元素都是后代元素。
	* 如果 ancestor1 元素是祖先元素的话，那其包含的所有元素都是后代元素。
	* 如果 parent1 元素是祖先元素的话，那其包含的所有元素都是后代元素。
### 层级选择器种类
* 后代选择器
简单来说，该元素的所有后代元素。
```
div span {
      background-color: lightcoral;
    }
<div>
    <span>Span 1.
      <span>Span 2.</span>
    </span>
  </div>
<span>Span 3.</span>

这样只会影响到div里的span标签，而div外的标签则不会受到影响
```
* 子级选择器
定位该元素的所有子级元素。并不会影响孙子级元素
```
<style>
span{
	color: teal;
}
div>span{
	color: violet;
}
</style>
```
* 相邻兄弟选择器
定位与该目标元素拥有同一个父级元素的**下一个**指定元素 <font color="red">不包括当前元素，只包括后边的元素</font>
```
    <style>
    #a+li{
        color: red;
    }
    </style>
    <ul>
        <li id="a">1sadsada</li> <!-- 不会变色 -->
        <li>555555</li><!-- 变色 -->
    </ul>
```
* 普通兄弟选择器
简单来说就是 p~codep 元素之后的元素

定位与该目标元素拥有同一个父级元素的之后任意指定元素
```
  <style>
    span {
      background-color: lightgreen;
    }

    p~code {
      background-color: lightcoral;
    }
  </style>
  <span>This is not red.</span>
  <p>Here is a paragraph.</p>
  <code>Here is some code.</code> 会变色
  <span>And here is a span.</span>
```
## 组合选择器
### 组合（并集）选择器
`h1, h2, h3, h4, h5, h6 { color:blue; }` 同时定义多个标签的属性。
### 组合（交集）选择器
```
p.cls {
    color: blueviolet;
}
表示把所有class名为cls的p标签都设置成blueviolet颜色
```
## 伪类选择器
```
/* 所有用户指针悬停的按钮 */
button:hover {
  color: blue;
}
```
### 否定伪类选择器
>:not(selector) {
    属性 : 属性值;
}
```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>否定伪类选择器</title>
  <style>
    .fancy {
      text-shadow: 2px 2px 3px gold;
    }

    p:not(.fancy) { /*匹配class名不是fancy的p标签*/
      color: green;
    }

    body :not(p) { /*匹配body中不是p标签的标签*/
      text-decoration: underline;
    }
  </style>
</head>

<body>
  <p>我是一个段落。</p>
  <p class="fancy">我好看极了！</p>
  <div>我不是一个段落。</div>
</body>

</html>
```
## 伪元素选择器
```
/* CSS3 语法 */
选择器::伪元素 {
  属性 : 属性值;
}
/* CSS2 过时语法 (仅用来支持 IE8) */
选择器:伪元素 {
  属性 : 属性值;
}
```
<b>伪元素选择器</b>的语法格式为 `::伪元素`，一定不要忘记 ::。伪元素选择器只能和基本选择器配合使用，并且一个选择器只能使用一个伪元素选择器，如果要为一个选择器增加多个伪元素选择器需要分别编写。
### before 和 after
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        a::after{
            content: "→";
        }
        a::before{
            content: "♥";
        }
    </style>
</head>
<body>
    <div>
        <a href="https://antmoe.com">这是一个测试内容</a>
    </div>
</body>
</html>
```
{% fb_img https://tva1.sinaimg.cn/large/832afe33ly1gaio8klqf7j20k50iadgw.jpg %}
### first-letter
<b>::first-letter</b> 伪元素的作用是为匹配元素的文本内容的第一个字母设置样式内容。 如下示例代码展示了 **::first-letter 伪元素 ** 的用法：
```
p::first-letter {
  font-size: 130%;
}
```
### first-line 伪元素
<b>::first-line</b> 伪元素的作用是为匹配 HTML 元素的文本内容的第一行设置样式内容。 如下示例代码展示了 **::first-line 伪元素 ** 的用法：
```
.line::first-line{
            background-color: tomato;
        }
```
### ::selection 伪元素
<b>::selection</b> 伪元素的作用是匹配用户在 HTML 页面选中的文本内容（比如使用鼠标或其他选择设备选中的部分）设置高亮效果。如下示例代码展示了 **::selection 伪元素 ** 的用法：
```
p::selection {
    color: gold;
    background-color: red;
}
```
{% fb_img https://tva1.sinaimg.cn/large/832afe33ly1gaiohh13mbg20ok0950su.gif %}
<blockquote>
<p>注意：&nbsp只有一小部分 CSS 属性可以用于::selection 伪元素：</p>
<ul>
<li>color 属性</li>
<li>background-color 属性</li>
<li>cursor 属性</li>
<li>caret-color 属性</li>
<li>outline 属性</li>
<li>text-decoration 属性</li>
<li>text-emphasis-color 属性</li>
<li>text-shadow 属性</li>
</ul>
</blockquote>
<font color="red" size=5>最后感谢原创！！！支持原创</font>