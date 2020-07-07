---
title: Simple String Queries【树状数组】
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-03 21:33:08
tags: 树状数组
keywords:
description: 一个灵活了一点的树状数组
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/39.webp
---
# 题目
>Simple String Queries   

Problem Statement   

You are given a string S of length N

consisting of lowercase English letters.

Process Q queries of the following two types:

* Type 1
: change the iq-th character of S to cq. (Do nothing if the iq-th character is already cq
.)
* Type 2
: answer the number of different characters occurring in the substring of S between the lq-th and rq-th characters (inclusive).   

Constraints   
* N, Q, iq, lq, and rq are integers.
* S is a string consisting of lowercase English letters.
* cq is a lowercase English letter.
* 1≤N≤500000
* 1≤Q≤20000
* |S|=N
* 1≤iq≤N
* 1≤lq≤rq≤N
* There is at least one query of type 2 in each testcase.

Input   

Input is given from Standard Input in the following format:
N   
S   
Q    
Query1⋮    
QueryQ   
Here, Queryi in the 4-th through (Q+3)-th lines is one of the following:

1 iq cq

2 lq rq

Output

For each query of type 2, print a line containing the answer.

Sample Input 1

7   
abcdbbd   
6   
2 3 6   
1 5 z   
2 1 1   
1 4 a   
1 7 d   
2 1 7   

Sample Output 1

3
1
5

In the first query, cdbb contains three kinds of letters: b , c , and d, so we print 3    
.   
In the second query, S is modified to abcdzbd.    
In the third query, a contains one kind of letter: a, so we print 1   
.    
In the fourth query, S is modified to abcazbd.    
In the fifth query, S does not change and is still abcazbd.    
In the sixth query, abcazbd contains five kinds of letters: a, b, c, d, and z, so we print 5    
.    
# 思路
刚做这道题就看出来要用树状数组，但是我只做过模板题，想了好久才做了出来，用一个二维的树状数组来储存当前位置之前每一个字母的数量，一维表示26个字母，二维表示数量
# Code
```
#include<stdio.h>
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<string>
#define ios ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
using namespace std;
typedef long long ll;
const int maxn=5e5+100;
char ch[maxn];
int n,arr[27][maxn];
int lowbit(int n){
	return n&-n;
}
void update(int p,int t,int a[]){
	for(int i=p;i<=n;i+=lowbit(i)){
		a[i]+=t;
	}
}
ll getsum(int p,int a[]){
	ll ans=0;
	for(int i=p;i>0;i-=lowbit(i)){
		ans+=a[i];
	}
	return ans;
}
int main()
{
	int m;
	cin>>n;
	scanf("%s",ch+1);
	for(int i=1;i<=n;i++){
		update(i,1,arr[ch[i]-'a']); //更新当前位置存储的当前位置之前的某一个字母数量
	}
	cin>>m;
	for(int i=1;i<=m;i++){
		int t; cin>>t;
		if(t==1){
			char c;
			int p;
			cin>>p>>c;
			update(p,-1,arr[ch[p]-'a']);  //将当前位置存储的字母数量减一
			ch[p]=c; //记得更新ch数组
			update(p,1,arr[c-'a']);
		}
		if(t==2){
			ll sum=0,x,y;
			cin>>x>>y;
			for(int i=0;i<26;i++){
				sum+=getsum(y,arr[i])-getsum(x-1,arr[i])>0;  //26个字母在这个区间的数量若大于0，则表示存在这个字母，sum加一
			}
			cout<<sum<<endl;
		}
	}
    return 0;
 } 
```