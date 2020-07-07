---
title: 差分
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
date: 2020-04-14 16:06:07
tags: 算法
keywords:
description: 借用模板题讲解差分
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/2.webp
---

# 题目
题目：

输入一个长度为n的整数序列。
接下来输入m个操作，每个操作包含三个整数l, r, c，表示将序列中[l, r]之间的每个数加上c。
请你输出进行完所有操作后的序列。

输入格式

第一行包含两个整数n和m。
第二行包含n个整数，表示整数序列。
接下来m行，每行包含三个整数l，r，c，表示一个操作。

输出格式

共一行，包含n个整数，表示最终序列。

## 思路
对一个区间内的数加C，如果暴力加，会浪费很多时间，我们可以开一个新数组用于差分操作，数组下标就代表数轴上的每一个数，每次给定一个区间，把以区间左端点未下标的数组值加上C，而以（区间右端点+1）为下标的数组值减去C，进行m次操作后，再求一次前缀和并加上原来数组的值就是进行区间操作后的数组，参考下图：
![](差分/0.jpg)
## Code
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
using namespace std;
int a[100],b[100];  //例题，开的很小，你可以开大
int main()
{
//	freopen("test.txt","r",stdin); 
	int n,m;
	cin>>n>>m;
	for(int i=1;i<=n;i++){
		cin>>a[i];
//		b[i]+=a[i];
//		b[i+1]-=a[i];  //也可以初始化b数组就加上a数组的值，如果这样做了，下面就不能加a数组的值了
	}
	for(int i=1;i<=m;i++){
		int l,r,c;
		cin>>l>>r>>c;
		b[l]+=c;
		b[r+1]-=c;
	}
	for(int i=1;i<=n;i++) b[i]+=b[i-1]; //前缀和
	for(int i=1;i<=n;i++) cout<<b[i]+a[i]<<" ";  //一定要记得加上原数组的值
}
```
>复杂度： O(N)，小于1e8的数据量都可以过

很简单的算法吧（owo）