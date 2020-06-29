---
title: Valine添加自定义表情
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 记录
comments: true
date: 2020-05-01 18:43:17
tags: Hexo
keywords:
description: 为你的评论系统添加自定义表情
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/36.webp
---
## 前言
Valine是一个很好的评论系统，今天我发布这篇文章主要是为了让小白少掉一些坑，先来看看成果:laughing:：   
<img src="https://cdn.jsdelivr.net/gh/GamerNoTitle/ValineCDN/HONKAI3-Stan/70677f9c51bd601e60881da47d77a4e6189ad895.gif" />   
![](Valine添加自定义表情/0.png)   
`还挺不错的吧`   

## 教程
[官方文档](https://valine.js.org/changelog.html)   
如果您不嫌弃鄙人的当然也可以直接用:kissing_smiling_eyes:   
效果可以在我的博客评论区点击表情预览    
链接：https://pan.baidu.com/s/1XbmZXCfBZddANNzVzPhIxw    
提取码：r0ol   
{% span ultra logo red, 不要白嫖了留下您的评论点个赞吧 %}
首先确定你的valine版本是在1.4.5之上的，因为在这之后valine才支持自定义表情，我的主题是Sakura，自带的版本是1.3.5，低版本的，所以就要去[官方](https://github.com/xCss/Valine/releases)下载，可以挑自己喜欢的版本   
![](Valine添加自定义表情/1.png)    
点击上面的Source code(zip)下载压缩包，下载后找到里面的`Valine.min.js`将其移到`myblog-sakura\themes\Sakura\source\js`目录下，然后在`blog\myblog-sakura\themes\Sakura\layout\_partial\footer`中找到valine.min.js的引用，将其改为本地引用，可以搜索valine，将这一句引用改成：
```
  <script src='/js/valine.min.js'></script>
```
你也可以`在commit.ejs`中应用，都一样，只不过Sakura默认是在`footer.ejs`中，之后更改下载好的js文件内容：搜索smile找到表情部分:
![](Valine添加自定义表情/2.png)
这是我更改后的样子，默认的是压缩性的，找到这一部分后，就可以在这里面添加表情了，注意这里默认的接口是`https://img.t.sinajs.cn/t4/appstyle/expression/ext/normal/`，首先要更改成自己的接口，可参考文章：[点击我](https://bili33.top/2020/04/19/Valine-Magic/)，这是一个现成的表情仓库，直接引用即可，不过如果引用了这个，就不能添加自己想要的了，如果想添加自己想要的，就要收集所有表情到GitHub仓库里面，使用JsDeliver引用，如果有不会JsDeliver的可以百度，很多教程，不过这个过程可能有点麻烦，~~我是个很懒的人~~ :laughing:，改完以后，使用
```
hexo clean && hexo g && hexo s
```
命令预览一下，没问题部署到远程仓库就ok了<img src="https://cdn.jsdelivr.net/gh/drew233/cdn/kawayi.webp"  />

另外新版的valine是支持qq头像的，在评论名称处打上你的QQ号发表评论直接默认是你的qq头像，很NICE！

ending~ 撒花。

## 参考文档
[Valine官方文档](https://valine.js.org/)

[其他详细教程](https://cungudafa.gitee.io/post/8202.html#%EF%BC%883%EF%BC%89-%E6%A0%B9%E6%8D%AE%E9%82%AE%E7%AE%B1%E6%8B%89%E5%8F%96qq%E5%A4%B4%E5%83%8F)

[Uncle_drew -《valine添加自定义表情》](https://cndrew.cn/2020/04/09/valinebq/)

[Valine详细介绍](https://blog.csdn.net/cungudafa/article/details/104281764)