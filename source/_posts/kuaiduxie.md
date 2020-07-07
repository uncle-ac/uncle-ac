---
layout: post
title: 快读快写
date: 2020-04-06 10:20:05
tags: [模板]
categories: 算法
author: uncleacc
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/7.webp
---

## 应用
一些题目需要读取的数据量十分庞大，很可能读取数据次数高达几十万次，而这时用cin或者scanf时间上就有了一些差距（scanf比cin要快），当输入数据更加庞大，scanf时间上也有些乏力，毕竟有些题目就是考你会不会快读快写，C语言输入输出字符时是要比数字输出的快的，我们可以利用这一点来把数字转化成字符来输出

下面的inline是内联函数的意思，小伙伴可以看 https://blog.csdn.net/hyqsong/article/details/51857833 了解一下
## 快读代码

    inline int read(){
        int x=1,y=0;
        char ch=getchar();
        while(ch<'0'||ch>'9'){
            if(ch=='-') x=-1; ch=getchar(); //这里的循环是为了避免输入数字之前的空格造成影响
        }
        while(ch>='0'&&ch<='9'){
            y=y*10+ch-'0';
            ch=getchar();
        }
        return x*y;
    }
需要注意的是每次循环最后都要加一句getchar()
## 快写代码

    inline void write(int n){
        if(n<0) putchar('-'),n*=-1;
        if(n>9) write(n/10);
        putchar(n%10+'0');
    }
代码比较好理解，注意puchar()里面的n%10
>制作不易，您的赞助是我最大动力，点击下面的赏，快来赞助我吧

![](https://dss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=868635476,2104676212&fm=111&gp=0.jpg)