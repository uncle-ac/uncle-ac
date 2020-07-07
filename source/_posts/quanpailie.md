---
title: 全排列问题
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 记录
comments: true
date: 2020-05-05 10:44:09
tags: 全排列
keywords:
description:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/40.webp
---
# STL next_permutation函数实现
[原文链接](https://www.cnblogs.com/luruiyuan/p/5914909.html)    
掌握了next_permutation函数的原理:smile:   
```
void inline swap(char *s1,char *s2){
    char t=*s1;
    *s1=*s2;
    *s2=t;
}
/**
*反转字符串函数,s,e分别执行字符串的开始和结尾，不能反转中文 
**/
void reverse(char *s,char* e){
    for(e--;s<e;s++,e--)swap(s,e);
}

bool next_permutation(char *start,char *end){
    char *cur = end-1, *pre=cur-1;
    while(cur>start && *pre>=*cur)cur--,pre--;
    if(cur<=start)return false;
    
    for(cur=end-1;*cur<=*pre;cur--);//找到逆序中大于*pre的元素的最小元素 
    swap(cur,pre);
    reverse(pre+1,end);//将尾部的逆序变成正序 
    return true;
}
```
`prev_permutation只要将上面的cur和pre作比较的地方换一下位置就行了`
```
bool prev_permutation(char *start,char *end){
    char *cur = end-1, *pre=cur-1;
    while(cur>start && *pre<=*cur)cur--,pre--;//这里符号有变化 
    if(cur<=start)return false;
    
    for(cur=end-1;*cur>=*pre;cur--);//这里符号有变化 
    swap(*cur,*pre);
    reverse(pre+1,end);
    return true;
}
```
## 例题
给出一个字符串S（可能有重复的字符），按照字典序从小到大，输出S包括的字符组成的所有排列。例如：S = "1312"，

输出为：

1123   
1132   
1213   
1231   
1312   
1321   
2113   
2131   
2311   
3112   
3121   
3211   
Sample Input   
1312
Sample Output   
1123   
1132   
1213   
1231   
1312   
1321   
2113   
2131   
2311   
3112   
3121   
3211   
# CODE
```
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
bool next_permutation(char *start,char *end){
    char *cur = end-1, *pre=cur-1;
    while(cur>start && *pre>=*cur)cur--,pre--;
    if(cur<=start)return false;
    
    for(cur=end-1;*cur<=*pre;cur--);//找到逆序中大于*pre的元素的最小元素 
    swap(*cur,*pre);
    reverse(pre+1,end);//将尾部的逆序变成正序 
    return true;
}

int main(){
    char s2[100];
    scanf("%s",s2);
    int n=strlen(s2);
    sort(s2,s2+n);
    do{
        puts(s2);
//        cnt++;
    }while(next_permutation(s2,s2+n));
//    printf("%d",cnt);
}

DFS

#include<iostream>
#include<algorithm>
#include<cstring>
 
using namespace std;
 
const int N = 10+5;
 
char ch[N];
char ans[N];
bool vis[N];
int n;
 
void dfs(int x){
	int i;
	if(x == n){ 
		printf("%s\n",ans);
	}else{
		for(int i = 0; i < n;i++){
			if(vis[i] == false){
				vis[i] = true;
				ans[x] = ch[i];
				dfs(x+1);
				vis[i]=false;
				//筛除重复数据 
				while(i < n-1 && ch[i+1]==ch[i]) i++;
			}
		}
	}
}
 
int main()
{
	scanf("%s",&ch);
	n = strlen(ch);
	sort(ch,ch+n);
	dfs(0);
	return 0;
}
```