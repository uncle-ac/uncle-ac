---
title: 如何跳过某网盘限速
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/76.webp'
date: 2020-08-17 16:40:45
authorLink:
categories: 技术
tags: 网盘限速
keywords: 
description: 优雅跳过某某网盘限速，高速下载文件
---

> 你有多久没体验过10M/1s的速度了？10kb/s的速度是否恶心到了你？自从某盘限速以来无数人埋怨叫嚣，今天向网友们分享一款脚本文件可以直接找到下载原链接，避过限速，亲测有效🐷

## 前言

网盘限速这块做的真的牛，之前在网上看到过很多说是直链提取网盘链接的脚本或者插件，尝试过好多个了，到最后得到的链接无一例外都是403，今天无意中又发现了一个，抱着试一试反正不知道干些啥的心态试了试，没想到竟然成功了！贴出原文档链接：[[网盘直链下载助手](https://www.baiduyun.wiki/zh-cn/assistant.html)，具体请查看原文档，十分详细，本文只是说一下大致使用方法

## 正言

首先请保证你使用的浏览器是Goole，火狐也可以，不过本文就这Google介绍

如果Goole插件您还没有用过，请先查看[本文](https://www.fezhu.top/2020/06/22/yixiehaoyong/)

第一步：去插件商城里面下载暴力猴或者油猴，推荐暴力猴，~~因为我没油猴没用过~~

第二步：[点击我](https://www.baiduyun.wiki/download.html)，找到`源码地址`，点击，之后点击`查看脚本`，跳转到暴力猴安装脚本页面点击安装

第三步：自行搜索IDM，安装IDM

第四步：集成到Goole浏览器中，一般会自动集成，若没有，则需要手动点开扩展管理，点开开发者模式，将IDM文件夹中的`IDMGCExt.crx`，文件移动到Goole中，集成完毕

{% fb_img http://ww1.sinaimg.cn/large/007Y60soly1ghtwy8oe2gj30ee05s3yp.jpg %}

如图所示即成功

第五步：点开任意一个网盘分享页面，找到图上内容

{% fb_img http://ww1.sinaimg.cn/large/007Y60soly1ghtx2alto9j30mo08gjrg.jpg %}

点击`直接下载`或者`显示链接`，当IDM集成到Goole时直接下载就可以唤醒IDM下载器直接进行高速下载，若你没有集成到浏览器，也可以显示链接复制链接，在IDM新建任务进行下载，推荐第一种，因为显示链接有可能是无效链接

## 补充

网盘万能助手可以配合网盘直链助手下载，网盘万能助手是一款插件，它配合Xdown可以获取Aria下载地址进行下载，有兴趣可以自行琢磨

<font color="red" size=8>不要白嫖了留下您的评论吧</font>

