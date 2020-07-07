---
title: 数状数组求逆序对&&二维树状数组
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
date: 2020-04-29 21:04:20
tags: 树状数组
keywords:
description:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/35.webp
---
参考原文：[](https://blog.csdn.net/GodJing007/article/details/81158288)
# 二维树状数组
我们先来讲讲怎么去表示。   
数组A[][]的树状数组定义为：

C[x][y] = ∑ a[i][j], 其中，
x-lowbit(x) + 1 <= i <= x,
y-lowbit(y) + 1 <= j <= y.

例：举个例子来看看C[][]的组成。  
设原始二维数组为：   
```
A[][]={{a11,a12,a13,a14,a15,a16,a17,a18,a19},   
{a21,a22,a23,a24,a25,a26,a27,a28,a29},   
{a31,a32,a33,a34,a35,a36,a37,a38,a39}，   
{a41,a42,a43,a44,a45,a46,a47,a48,a49}};
```   
那么它对应的二维树状数组C[][]呢？

记:
B[1]={a11,a11+a12,a13,a11+a12+a13+a14,a15,a15+a16,…} 这是第一行的一维树状数组   
B[2]={a21,a21+a22,a23,a21+a22+a23+a24,a25,a25+a26,…} 这是第二行的一维树状数组   
B[3]={a31,a31+a32,a33,a31+a32+a33+a34,a35,a35+a36,…} 这是第三行的一维树状数组   
B[4]={a41,a41+a42,a43,a41+a42+a43+a44,a45,a45+a46,…} 这是第四行的一维树状数组   
那么：
C[1][1]=a11,C[1][2]=a11+a12,C[1][3]=a13,C[1][4]=a11+a12+a13+a14,c[1][5]=a15,C[1][6]=a15+a16,…   
这是A[][]第一行的一维树状数组

C[2][1]=a11+a21,C[2][2]=a11+a12+a21+a22,C[2][3]=a13+a23,C[2][4]=a11+a12+a13+a14+a21+a22+a23+a24,   
C[2][5]=a15+a25,C[2][6]=a15+a16+a25+a26,…   
这是A[][]数组第一行与第二行相加后的树状数组   

C[3][1]=a31,C[3][2]=a31+a32,C[3][3]=a33,C[3][4]=a31+a32+a33+a34,C[3][5]=a35,C[3][6]=a35+a36,…   
这是A[][]第三行的一维树状数组   

C[4][1]=a11+a21+a31+a41,C[4][2]=a11+a12+a21+a22+a31+a32+a41+a42,C[4][3]=a13+a23+a33+a43,…   
这是A[][]数组第一行+第二行+第三行+第四行后的树状数组
> 就是每一行都是一个树状数组，以行为元素，整个列也是一个树状数组。
（这句话请记住，这个思想会贯穿始终）
既然如此，我相信代码也很快就出来了，接下来我就来给出代码，并进行简单的解释。
## 单点修改
```
void add(int x,int y,int p){  
    while(x<=n){
        for(int i=y;i<=m;i+=lowbit(i)){
            C[x][i]+=p;
        } 
        x+=lowbit(x);
    }  
}  
```
这个根据我刚刚说的两个树状数组（那句贯穿始终的话），就很容易理解了。
我们外围循环枚举每一行，内循环在行内进行一维树状数组的单点修改，从而实现二维树状数组的单点修改。
## 以原点为一个端点的子矩阵和
```
int sum(int x,int y){  
    int result = 0;  
    while(x>0){
        for(int i=y;i>0;i-=lowbit(i)){
            result+=C[x][i];
        }
        x-=lowbit(x);
    }  
    return result;  
}  
```
## 以任意两点为左上和右下两个端点的子矩阵和
```
int ask(int x1,int y1,int x2,int y2){
    return sum(x2,y2)+sum(x1-1,y1-1)-sum(x2-1,y1)-sum(x1,y2-1);
}
```
# 逆序对
这个借助例题来讲解   
[题目](https://vjudge.net/contest/368993#problem/B)   
求冒泡排序交换了多少次，分析一下，不难看出就是求逆序对的个数，首先明白逆序对的概念给定i,j满足i < j && a[ i ]>a[ j ]则a[ i ]和a[ j ]就是一对逆序对。   
求逆序对的做法，举个例子：   
给定序列   
9 6 4 8 7
遍历这个数组，每次遇到一个数，就把该数所在的树状数组的位置处的数加一：   
1 2 3 4 5 6 7 8 9   
0 0 0 0 0 0 0 0 0  
初始数组值为0，代表插入0个数，sum(n)代表小于等于该数的个数，那么大于该数的个数就是i-sum(n) 
1. 1 2 3 4 5 6 7 8 9   
   0 0 0 0 0 0 0 0 1   
  此时插入了一个数，sum(9)=1,1-1=0
2. 1 2 3 4 5 6 7 8 9   
   0 0 0 0 0 1 0 0 1   
   sum(6)=1 2-1=1
3. 1 2 3 4 5 6 7 8 9   
   0 0 0 1 0 1 0 0 1   
   sum(4)=1 3-1=2
4. 1 2 3 4 5 6 7 8 9   
   0 0 0 1 0 1 0 1 1   
   sum(8)=3 4-3=1   
5. 1 2 3 4 5 6 7 8 9   
   0 0 0 1 0 1 1 1 1  
   sum(7)=3 5-3=2 

上面所有的得数相加就得到了最后结果：   
0+1+2+1+2=6   
这就是思路，思路清楚了就可以做题了，可是这道题数据很大，明显开不了这么大得树状数组，一个简单得离散化就行了，先对树状数组按照值的大小进行排序，新开一个从序号1开始的连续的数组，一一映射到树状数组，最后对序号求逆序对就行了。

建立一个结构体包含val和id， val就是输入的数，id表示输入的顺序。然后按照val从小到大排序，如果val相等，那么就按照id排序。

如果没有逆序的话，肯定id是跟i（表示拍好后的顺序）一直一样的，如果有逆序数，那么有的i和id是不一样的。所以，利用树状数组的特性，我们可以简单的算出逆序数的个数。

如果还是不明白的话举个例子。（输入4个数）

输入：9 -1 18 5

输出 3.

输入之后对应的结构体就会变成这样   
val：9 -1 18 5    
id：  1  2  3  4    
排好序之后就变成了   
val ：  -1 5 9 18   
id：      2 4  1  3   
2 4 1 3 的逆序数 也是3   
之后再利用树状数组的特性就可以解决问题了
```
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<algorithm> 
using namespace std;
const int maxn=500001;
int c[maxn];
struct Node
{
	int v,index;
	bool operator < (const Node &b) const
	{
		return v<b.v; //从小到大排序 
	}
}node[maxn];
int n;
void add(int i)
{
	while(i<=n)
	{
		c[i]++;
		i+=i&(-i);	
	}
}
long long getsum(int i)
{
	long long res=0;
	while(i>0)
	{
		res+=c[i];
		i-=i&(-i);
	}
	return res;
}
 
int main()
{
	while(1){
		cin>>n;
		if(n==0) break;
		int a;
		memset(node,0,sizeof node);
		memset(c,0,sizeof c);
		for(int i=1;i<=n;i++)
		{
			scanf("%d",&a);
			node[i].index=i;
			node[i].v=a;
		}
		sort(node+1,node+1+n);
		long long ans=0;
		for(int i=1;i<=n;i++)
		{
			add(node[i].index);  //离散化结果—— 下标等效于数值
			ans+=i-getsum(node[i].index); //得到之前有多少个比你大的数（逆序对）
		}
		cout<<ans<<endl;
	}
	return 0;
}
```