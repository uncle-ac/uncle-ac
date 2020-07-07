---
title: 一道组合计数的高中数学题
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/56.webp
date: 2020-06-03 23:23:30
tags: 数论
keywords: 
description: 一道组合计数的经典题目
---
>一道组合计数的经典题目，箱子放球🐴
## 题目描述
特斯拉公司的六位密码被轻松破解后，引发了人们对电动车的安全性能的怀疑。李华听闻后，自己设计了一套密码：假设安全系统中有n个储存   区，每个储存区最多能存储存2个种类不同的信号（可以不储存任何信号）。有0和1这两种信号，其中0有a个，1有b个，单独一个0或1算一个信号。现要将这些信号储存在储存区中，0和1可以不用全部储存，一种不同的储存方案经过李华处理后就将是一串不同的密码。现在给出n，a，b，求可能的不同储存方案的个数。

## 输入格式
>第一行：共3个整数，n，a，b。  
a,b≤50，n+a≤50，n+b≤50
## 输出格式
>第一行：一个整数，表示方案个数。
## 输入输出样例
**输入**
>2 1 1
**输出**
>9
## 题目链接

[点击我](https://www.luogu.com.cn/problem/P2638)

## 题解

[点击我](https://www.luogu.com.cn/blog/x4Cx58x54/solution-p2638)

## 正解代码
```
#include <stdio.h>
#include <iostream>
#include <string>
#include <string.h>
#include <map>
#include <queue>
#include <stack>
#include <algorithm>
#include <vector>
#include <set>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define debug freopen("in.txt","r",stdin); freopen("out.txt","w",stdout)
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int MAXN = 1e5;
const int MOD = 1e9;
ull C[60][60];
void init(){
	C[0][0]=1;
	C[1][0]=1; C[1][1]=1;
	for(int i=2;i<=50;i++){
		C[i][0]=1;
		for(int j=1;j<=i;j++){
			C[i][j]=C[i-1][j-1]+C[i-1][j];
		}
	}
}
int main()
{
    ios;
    init();
	int n,a,b;
	cin>>n>>a>>b;
	int maxx=max(a,b)-1+n;
	ull ans=0;
	for(int i=0;i<=a;i++){
		for(int j=0;j<=b;j++){
			ans+=C[i-1+n][n-1]*C[j-1+n][n-1];
		}
	}
	cout<<ans<<endl;
    return 0;
}
```