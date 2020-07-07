---
title: 水果
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-16 22:43:16
tags: STL
keywords: 
description: 一道map的经典排序题
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/45.webp
---
# 题目
<font color="red" size=4>

F - 水果

夏天来了~好开心啊,呵呵,好多好多水果~
Joe经营着一个不大的水果店.他认为生存之道就是经营最受顾客欢迎的水果.现在他想要一份水果销售情况的明细表,这样Joe就可以很容易掌握所有水果的销售情况了.

Input

第一行正整数N(0<N<=10)表示有N组测试数据.
每组测试数据的第一行是一个整数M(0<M<=100),表示工有M次成功的交易.其后有M行数据,每行表示一次交易,由水果名称(小写字母组成,长度不超过80),水果产地(小写字母组成,长度不超过80)和交易的水果数目(正整数,不超过100)组成.

Output

对于每一组测试数据,请你输出一份排版格式正确(请分析样本输出)的水果销售情况明细表.  这份明细表包括所有水果的产地,名称和销售数目的信息.水果先按产地分类,产地按字母顺序排列;同一产地的水果按照名称排序,名称按字母顺序排序.   
两组测试数据之间有一个空行.最后一组测试数据之后没有空行.

Sample Input

1  
5  
apple shandong 3  
pineapple guangdong 1  
sugarcane guangdong 1  
pineapple guangdong 3  
pineapple guangdong 1  

Sample Output

guangdong  
   |----pineapple(5)  
   |----sugarcane(1)  
shandong  
   |----apple(3)  
</font>

# 思路

map<（1）,（2）>，1，2不只可以是基础数据类型，还是可以自定义类型，经常用自己定义的结构体，在结构体里面还可以再定义一个map，类似循环嵌套一样，map里面是有序排列的，这样的话就是最外面的map排列优先级最高，里面也是这样，这样就可以实现排列的优先级问题，而且map有一个特点就是它的键值是唯一的，天然去重，这道题正好可能会出现同名的水果，用一般数组写的话里面的数据关系很复杂，而用map比较清晰
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<map>
using namespace std;
typedef long long ll;
const int maxn=2e5+100;
struct node{
	map<string,int> mp;  //名字（优先级低），数量
}f[110];
int main()
{
	map<string,node> mm; //地点（优先级高），第二个是一个自定义结构体，实际上是一个小map
	map<string,int>::iterator mpit;
	map<string,node>::iterator mmit;
	int t; cin>>t;
	while(t--){
		mm.clear();
		int n; cin>>n;
		for(int i=0;i<n;i++){
			string name,place;
			int cnt;
			cin>>name>>place>>cnt;
			mm[place].mp[name]+=cnt;  //这里map实现了去重操作省去了很多麻烦，直接累加就行了
		}
		for(mmit=mm.begin();mmit!=mm.end();mmit++){
			cout<<mmit->first<<endl;
			for(mpit=mmit->second.mp.begin();mpit!=mmit->second.mp.end();mpit++){
				cout<<"   |----"<<mpit->first<<"("<<mpit->second<<")"<<endl;
			}
		}
		if(t!=0) cout<<endl;
	}
	return 0;
}
```