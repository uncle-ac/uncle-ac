---
title: 解决Leancloud流控问题
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/61.webp'
date: 2020-07-02 20:25:25
authorLink:
categories:
tags: 流控
keywords: 
description: 解决Leancloud流控问题
---
<blockquote>
文章背景
<p>因为明天要考科目一了，本来是打算明天下午写这篇文章的，可是Acm训练要开始了，所以决定提前写了吧，明天考完直接投入复习算法的学习中🐷哎，魔鬼月要开始了！</p>
</blockquote>

## 前言
之前写过一篇给Leancloud添加自定义邮件回复的文章[Click me](https://www.fezhu.top/2020/05/15/Valineyoujian/)，令我自责的是教程有一些问题，因为我也是看别人教程去做的，没想到她的那个教程错了，导致我也跟着错了。。`ADMIN_URL`这个值不是填博客地址，这个跟邮件回复没有半点关系，不加这个参数也行，这个参数是用来实行自唤醒任务用的，具体看文章吧，在这里跟我教错的网友说一声抱歉

## 正文
Leancloud最近实行了流控: 自唤醒任务是无法唤醒已经休眠的机器的，所以要想任何时候都能收到邮件就需要早上手动唤醒一次机器，接下来交给自唤醒任务就行了，不过每天都手动唤醒也是挺烦的，所以就有大佬站出来了，[原作者](https://www.antmoe.com/tags/LeanCloud%E6%B5%81%E6%8E%A7/)，这位大佬直接解决了这个问题，在短时间内众多网友纷纷效仿，Leancloud流控问题彻底解决

首先你要确保你的Leancloud是正常的，如果你的Leancloud是国内版本的，我劝你换成国际版本的，因为国内版本绑定Web域名是需要备案的，而备案有需要服务器，你总不可能一直续费服务器吧，而国际版本是不需要备案直接就能绑定的，而且Leancloud的数据是可以导入导出的，把久的数据导入到国际版本中，花不了多少时间，非常香🤗

### 绑定Web域名
[参考原文](https://www.93bok.com/Valine%E8%AF%84%E8%AE%BA%E7%9A%84Web%E7%AE%A1%E7%90%86%E7%95%8C%E9%9D%A2/)

点击`云引擎`->`设置`，找到Web主机域名，这里没有限制，你可以随便填写，一般都是自己博客的字母，比如我的就是fezhu，`注意不用加后缀和前面的www`！！！

{% fb_img https://img-blog.csdnimg.cn/20200702205200250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

<blockquote>
改域名作用
<p>当机器休眠时，访问此地址能够唤醒机器</p>
</blockquote>

然后点击上面的添加新变量，前面填ADMIN_URL，后面填Web主机域名，这个Web主机域名有什么用呢？你可以访问这个域名，这个网址就是你的评论后台地址

{% fb_img https://img-blog.csdnimg.cn/20200702205849784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

打开页面就是这个样子，接下来添加账户名和密码：

{% fb_img https://s1.ax1x.com/2018/11/22/FP0mlQ.png %}
{% fb_img https://s1.ax1x.com/2018/11/22/FP0uOs.png %}

OK,现在我们使用`email`字段的邮箱去登陆即可

{% fb_img https://s1.ax1x.com/2018/11/22/FP0lT0.png %}

在这个后台你还能查看删除评论

### 设置定时任务
定时任务使用cron表达式设定的<br>
[参考文章](https://www.cnblogs.com/yanghj010/p/10875151.html)<br>
首先应该明白UTC时间和北京时间区别：
>协调世界时,又称世界统一时间,世界标准时间,国际协调时间,简称UTC。
UTC时间比北京慢8个小时，UTC时间=北京时间-8小时，国际版本用的是UTC时间，所以定时任务要减去8小时，我是设定了三个定时任务，分别填写cron表达式如下：

`0 25/0 0-15 * * ?`&&`0 45/0 0-15 * * ?`&&`0 5/0 0-15 * * ?`

这表示从北京时间早上八点开始一直到晚上11点，每小时的5分，25分，45分都执行一次自唤醒，之所以不设成整点是因为，从外部唤醒会有一定延迟

### 从外部访问后台

<font color="red" size=4>接下来就是重点内容了</font>

1. 鼠标放在右上角，选择 setting
	{% fb_img https://cdn.jsdelivr.net/gh/blogimg/picbed@latest/2020/05/14/a81f88e80fd7105d7cc3e1844970e8bd.png %}
2. 点击 Developer settings。
	{% fb_img https://cdn.jsdelivr.net/gh/blogimg/picbed@latest/2020/05/14/66df62fbc0d51403fcdc8223c4b6ce52.png %}
3. 选择` Personal access tokens`，添加一个新的`TOKEN`。<br>
	这个 `TOKEN` 主要使用来启动 `actions` 和上传结果用的。<br>
	设置名字为` GITHUB_TOKEN` , 然后勾选 `repo , admin:repo_hook , workflow` 等选项，最后点击 `Generate token` 即可。
	{% fb_img https://cdn.jsdelivr.net/gh/blogimg/picbed@latest/2020/05/15/23ee2808dce8dab17e06107d1ddaf5d6.png %}
	<b>名字请务必使用GITHUB_TOKEN。</b>
4. 接下来 FORK 项目。
	[点击我](https://github.com/blogimg/WakeLeanCloud)
	如果觉得好用可以点个赞哦！
5. 成功 FORK 后，进入项目的设置。添加你的 leancloud 的后台地址（也就是评论管理的后台地址）
	{% fb_img https://cdn.jsdelivr.net/gh/blogimg/picbed@latest/2020/05/14/d15b1fffb681f0dd3b9264ea878bf055.png %}
	选择 Secrets，添加你的评论后台地址，一定是 Leancloud 的后台地址（环境变量 ADMIN_URL），而不是你的博客地址。
	
	{% fb_img https://cdn.jsdelivr.net/gh/blogimg/picbed@latest/2020/05/14/6c2cd1845116e3d4e4147157d334be19.png %}
	
	{% fb_img https://cdn.jsdelivr.net/gh/blogimg/picbed@latest/2020/05/14/caed66e8408e5a0c91fe446951180f8d.png %}
    
    <div class="note danger">
    注意
    <p>SITE 的网址应填你的评论管理后台地址，而不是博客地址。</p>
    </div>
    
    其中 Name 的名字必须为 SITE，Value 可以是多个后台地址（注意请求头也要写），用英文逗号分隔。不要中中文逗号，不要用中文逗号，不要用中文逗号
6. 接下来对自己的项目点个 star 就能启动了，启动后请切换到 actions，看看是否运行成功。
	* 成功
		那么你就可以关掉了，默认是每天 8:00-24:00 时每 20 分钟运行一次。(GitHub 时间稍有延迟，大概时 2-5 分钟。)
		
		{% fb_img https://cdn.jsdelivr.net/gh/blogimg/HexoStaticFile2@latest/2020/05/25/4736d5905066440617f4a63ecfc70cc1.png %}
	
	* * 失败
		如果你的 GitHub 从来没有用过 actions，那么需要先 “了解”。方法很简单，点击绿色的按钮即可。
		
		{% fb_img https://cdn.jsdelivr.net/gh/blogimg/HexoStaticFile2@latest/2020/05/25/91a79177e21e2a49d28be09cc676ad7a.png %}

>自己点自己的项目是手动执行一次 actions。是为了测试才设计这个功能的哦！
并不是不点星这个 actions 就不会运行。

<font color="red" size=4>以上是原作者文章原话，非常详细</font><br>

这里我推荐大家把cron表达式改成`0 0 * * *`，这表示每天八点从外部访问一次你的后台地址，之后就可以交给你的定时任务了，如果过按照默认的设置，每小时都会提交好几次，一天下来都几百个了，不建议用默认设置

>>修改cron方法：修改workflows文件夹中的autoWakeup.yml文件，找到里面的cron表达式改成上述即可

一小时过去了......
<font color="red" size=5>帮助到您就点个赞吧，富豪也可以赞赏我哦🙃</font>

