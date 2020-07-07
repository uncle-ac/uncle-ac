---
title: 二维差分
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
date: 2020-04-27 15:26:14
tags: 二维差分
keywords:
description: 浅谈二维差分
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/31.webp
---
知道了一维差分二位差分就很好理解了，直奔主题吧
## 二维前缀和
先来温习一下二维前缀和

差分的思想是和前缀和有关的，一维的前缀和我们都懂求，那么二维的呢？
{% fb_img https://img-blog.csdnimg.cn/20200703225128170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}
因为是从左到右，从上到下的遍历，当要求红色部分，(0,0)到（i,j）处的前缀和时，我们黄色部分和蓝色部分已经是已知的了，而它们重叠的部分就是绿色部分，所以把黄色和蓝色部分的结果加起来，再减去绿色部分，最后加上(i,j)处的值就是（i，j）位置的前缀和了。　所以，二维前缀和就是sum[i][j]=a[i][j]+sum[i-1][j]+sum[i][j-1]-sum[i-1][j-1]

而我们要求左上角是(x1,y1),右下角是(x2,y2)的矩形区间内的值处理出前缀和后也可以O(1)时间内求出来。

{% fb_img https://img-blog.csdnimg.cn/20200703225136477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

我们要求紫色部分的值，我们已知的是黄色部分的值，但它多了两个蓝色部分的值，而两个蓝色部分有重叠了个绿色部分

所以要求的区间内的值就是sum[x2][y2]-sum[x2][y1-1]-sum[x1-1][y2]+sum[x1-1][x1-1]

## 二维差分
温习完二维前缀和来看看二维差分

如图
{% fb_img https://img-blog.csdnimg.cn/2020070322514598.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70%}
在我们要的区间开始位置（x1,y1）处+a，根据前缀和的性质，那么它影响的就是整个黄色部分，多影响了两个蓝色部分，所以在两个蓝色部分-a消除+a的影响，而两个蓝色部分重叠的绿色部分多受了个-a的影响，所以绿色部分+a消除影响。

所以就是
1. dif[x1][y1]+=a
2. dif[x1][y2+1]-=a
3. dif[x2+1][y1]-=a
4. dif[x2+1][y2+1]+=a

`简单不（owo）`