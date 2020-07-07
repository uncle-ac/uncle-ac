---
title: 皇后问题
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-04-14 23:05:13
tags: dfs
keywords:
description: 经典dfs
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/5.webp
---
## 题目
题目描述

一个如下的 6×6 的跳棋棋盘，有六个棋子被放置在棋盘上，使得每行、每列有且只有一个，每条对角线（包括两条主对角线的所有平行线）上至多有一个棋子。

![](https://cdn.luogu.com.cn/upload/pic/60.png)

上面的布局可以用序列 2 4 6 1 3 5 来描述，第 i 个数字表示在第 i 行的相应位置有一个棋子，如下：

行号 1 2 3 4 5 6

列号 2 4 6 1 3 5

这只是棋子放置的一个解。请编一个程序找出所有棋子放置的解。
并把它们以上面的序列方法输出，解按字典顺序排列。
请输出前 333 个解。最后一行是解的总个数。

输入格式

一行一个正整数 n，表示棋盘是 n×n 大小的。

输出格式

前三行为前三个解，每个解的两个数字之间用一个空格隔开。第四行只有一个数字，表示解的总数。
输入输出样例

输入 #1

6

输出 #1

2 4 6 1 3 5

3 6 2 5 1 4

4 1 5 2 6 3

4

说明/提示

【数据范围】
对于 100%100\%100% 的数据，6≤n≤136 \le n \le 136≤n≤13。

题目翻译来自NOCOW。

USACO Training Section 1.5

## Code
```
#include<iostream>
#include<stdio.h>
#include<algorithm>
using namespace std;
int col[20],l[100],r[100],a[100],n,cnt;
void queen(int i){
	if(i>n){
		if(cnt<3) for(int j=1;j<=n;j++) printf("%d%c",a[j],j==n?'\n':' ');
		cnt++; return;
	}
	for(int j=1;j<=n;j++){
		if(!col[j]&&!l[n-i+j]&&!r[i+j-1]){
			a[i]=j;
			col[j]=1; l[n-i+j]=1; r[i+j-1]=1;
			queen(i+1);
			col[j]=0; l[n-i+j]=0; r[i+j-1]=0;
		}
	}
	return ;
}
int main()
{
	cin>>n;
	queen(1);
	cout<<cnt<<endl;
}
```