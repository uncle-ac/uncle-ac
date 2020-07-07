---
title: Valine添加自定义邮件提醒
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
date: 2020-05-15 17:57:47
tags: Valine
keywords:
description: 给你的Valine评论系统添加邮件提醒功能
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/30.webp
---
## 前言
>我配置Valine时是尝试了两次的，第一次弄了一上午看了许多教程，因为当时不懂原理就跟着瞎琢磨，最后因为没有重启的原因没弄成，后来又搞了一次，重新开始最后偶然发现配置环境变量时有提醒说配置完环境变量后下一次部署后生效，我幡然醒悟，又去部署了一次果然成了，而且其他的教程都有点不对Valine的操作，比如原文档中说让在云引擎设置中部署代码，但是设置中没有部署页面，应该在云引擎部署页面中部署源码，可能是Valine更新页面了吧，在这里我记录一下（这个其实前几天就准备发的今天才开始），希望小白网友们可以少掉一些坑🙏

<font color="red" size=4> 现在Leancould对非标准版本的进行了限流，内部自唤醒已经行不通了，所以本教程最后的自唤醒请自行忽略，若想解决该问题请<a href="https://www.antmoe.com/posts/ff6aef7b/" target="_blank">点击我进行查看</a></font>

如果你要使用第三方邮件提醒(你也不得不使用第三方因为Valine在1.4以后的已经不支持邮件提醒了)，最好是使用国际版，因为国内的需要备案，如果你不想徒增不必要的麻烦我劝你最好用国际版本，如果你已经用了国内的，趁着用的时间还不算太长趁早换成国际版本吧(来自过来人的提醒)
## 教程
首先确定你的Valine的配置都是正常的，具体参考[Valine文档](https://valine.js.org/)，我看其他的教程有的说Valine说Valine是自带邮件提醒功能的，网友们不要被迷惑了   
<font color="red" size=5>!!! 自带的邮件提醒功能将在v1.4.0发布时下线，请使用自带邮件提醒的用户注意更改为第三方邮件提醒   
</font>
Valine文档里面有这一句话，说Valine自带邮件提醒的都不知道是多久前的文章了！   

* <font size=4> 本文是基于博主zhaojun1998开发的Valine-Admin，感谢开发者🙏</font>   

### 部署源码
<font color="green" size=4> 特别注意，如今Leancloud已经不支持node.js 6.*的部署，所以理论上网上部署的zhaojun1998的教程已经过期了，但是之前部署过的是不会影响的</font>

进入[Leancould](https://www.leancloud.cn/)对应的Valine应用中  
点击`云引擎->部署`  
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/5.png %}   
填上代码`https://github.com/BillChen2K/Valine-Admin`到代码库中并部署到master，最后在`日志`中看到部署成功就行了  
### 环境配置项
进入`云引擎->设置`配置环境变量   
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/6.png %}   
{% folding, 参数介绍 %}  

`SITE_NAME` : 网站名称  
`SITE_URL` : 网站地址, 最后不要加 /   
`SMTP_USER` : SMTP 服务用户名，一般为邮箱地址（例如QQ 账号.qq.com）  
`SMTP_PASS` : SMTP 密码，一般为授权码，而不是邮箱的登陆密码，请自行查询对应邮件服务商的获取方式  
`SMTP_SERVICE` : 邮件服务提供商，支持 QQ、163、126、Gmail、"Yahoo"、...... ，全部支持请参考[ Nodemailer Supported services](https://nodemailer.com/smtp/well-known/#supported-services) :  --- 如这里没有你使用的邮件提供商，请查看[自定义邮件服务器](https://github.com/zhaojun1998/Valine-Admin/blob/master/%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE.md#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A8)   
`SENDER_NAME` : 寄件人名称
`SENDER_EMAIL` : 收件的邮箱地址   
`SMTP_HOST` : 提供邮件服务的支持方（例如QQ，就是smtp.qq.com）  
`TEMPLATE_NAME` : 收到的邮件主题，不填则是默认，建议选rainbow效果在最后查看  

{% endfolding %}

需要说明如果你用的是QQ，需要开启SMTP服务  
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/7.png %}  
SMTP密码就是上面弹出的验证码 
### 设置邮件模板
进入`设置->邮件模板`填入以下代码，其中改掉相应的用户名为你自己的  
```
<p>Hi, {{username}}</p>
<p>你在 {{appname}} 的评论收到了新的回复，请点击查看：</p>
<p>
<a href="https://cungudafa.gitee.io" style="display: inline-block; padding: 10px 20px; border-radius: 4px; background-color: #3090e4; color: #fff; text-decoration: none;">马上查看</a></p>
```
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/8.png %}   
OK，到这里基本就结束了，最后在在`云引擎->部署`中点击重启，<font color="red">我就是在这里栽跟头的😒</font>  
看一下效果：  
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/pinglun.webp %}  
### 休眠
需要说明的是：
<font color="green" size=4>

免费版的 LeanCloud 容器，是有强制性休眠策略的，不能 24 小时运行：
* 每天必须休眠 6 个小时
* 30 分钟内没有外部请求，则休眠。
* 休眠后如果有新的外部请求实例则马上启动（但激活时此次发送邮件会失败）  
</font>

如果不想付费的话，最佳使用方案就设置定时器，每天 7 - 23 点每 20 分钟访问一次，这样可以保持每天的绝大多数时间邮件服务是正常的。
1. 点击云引擎 - 定时任务，新增定时器，按照图片上填写：  
```
0 0/20 7-23 * * ?
```
{% fb_img https://img-blog.csdnimg.cn/2020022516052988.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2N1bmd1ZGFmYQ==,size_16,color_FFFFFF,t_70 %}  
2. 在云引擎-设置-自定义环境变量中添加
```
ADMIN_URL:你的域名  
```
{% fb_img https://img-blog.csdnimg.cn/20200225160702506.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2N1bmd1ZGFmYQ==,size_16,color_FFFFFF,t_70 %}  
3. 添加后要记得去部署中点击重新启用
4. 启用成功后，每 20 分钟在`云引擎的 - 应用日志`中可以看到提示：
{% fb_img https://img-blog.csdnimg.cn/20200225161126521.png %}  

<font color="red" size=5 style="center">
<center>Ending~撒花，不要白嫖了😗，点个赞👍留下您的评论吧</center>

## 参考文档
[]()
[开发者文档](https://github.com/zhaojun1998/Valine-Admin)
[大佬程序媛](https://cungudafa.gitee.io/post/cb8f.html#2-valine-admin%E6%A8%A1%E6%9D%BF)
