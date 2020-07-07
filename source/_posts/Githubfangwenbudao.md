---
title: Github突然访问不到解决方案
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
date: 2020-06-05 09:16:42
tags: Github
keywords:
description: 差点气疯我的一天
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/58.webp
---
>小林下午老老实实的写着博客，当完成后网上提交时突然发现连接不上Github，当时还没有意识到问题严重性，因为以前也经常遇到这类问题，网络不好的原因，多试几次就行了，好的，又试了N次，都说找不到仓库，好家伙！这下我傻了，在浏览器上打开Github打不开！！！我懵了，博客部署不上我的博客不久毁于一旦了？不能！！

<font color="black" size=5>
我不敢保证此教程能完全解决您的问题，因为网上许多教程解决了一些人的问题对我却不适用，我只是分享出我的解决方案
</font>

## 解决部署问题
首先明白本地和Github取得联系是通过ssh的这把钥匙链接的，既然连接不上就说明这把钥匙有问题了，打开ssh所在文件夹，打开config文件(如果没有新建一个)，在里面添加如下内容：
```
Host github.com
User 此处为你的github账号绑定的邮箱
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```
添加以后，再次部署没问题，问题解决👍
## 解决浏览器访问不到以及ping不通Github
这个我真的是尝试了好久！！网上有很多教程，别看那么多，其实他们百分之八十都是同样的内容，让你往hosts文件后面加两句话，但是它们却都没有告诉你这俩句话不是对任何人都适用的，可能只是对他一个人有用！首先我们要明白我们为什么访问不到Github，我们上网时网络有一个DNS服务的东西，它会把你访问的那个域名也就是www.****.com的东西转换成IP地址，然后这个IP地址指向所对应的服务器，然后我们就能成功上网，现在访问不到Github就是因为DNS失效了，他不能帮你转换成IP地址或是它转换错了！那么我们就要自己来转换，如何转换呢？电脑里面都有一个叫hosts的文件，这个文件是干嘛的？就是用来解决DNS失效的问题的，hosts文件默认是一堆说明，#号后面是注释的意思，我们就要在这里面填上对应的IP地址和其域名，当你访问对应的域名时计算机会自动帮你转换成前面的IP，这也就是为什么那么多教程让你改hosts文件加这两行的原因
## 如何改hosts
登录网址[点击我](https://fastly.net.ipaddress.com/github.global.ssl.fastly.net)，在里面找到对应的IP
{% fb_img https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTgwMjI0MTk1MzA0MTAx %}   
hosts文件的更改需要权限，这个如果不会自行百度  
然后在hosts文件中加上你查到的IP，在后面加上github.com   
之后登录网址[点击我](https://ip.cha127.com/github.global.ssl.fastly.net.html)   
在里面找到对应IP，然后在hosts文件后面添加上对应IP并在后面加上~github.global.ssl.fastly.net~  
**记得域名与IP之间隔一个空格**   
之后再ping github成功了
## 解决浏览器访问不到github
ping通后可是浏览器还是访问不到github，只不过错误变了，原来是网页访问不到，现在是~~您的连接并不安全~~如何解决这个问题？我在网上找到的[原文](https://blog.csdn.net/sinat_35811978/article/details/80289219)  
删除上面让你添加的github.com的那一行，然后再访问就行了.......但是又ping不通了，这个我也不知道为啥，俩者好像没有关系😳，不过能访问就行

<font color="red" size=6>Ending~ 撒花</font>
