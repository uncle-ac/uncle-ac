---
title: 细胞分裂
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-04-15 22:15:51
tags: 唯一分解定理
keywords:
description: 一道经典唯一分解定理题目
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/17.webp
---
# 题目
[细胞分裂](https://www.luogu.com.cn/problem/P1069)
# 知识
任何一个数都可以表示成为这个数的质因子的幂的乘积形式，例如24^1^=24=2^3^*3
# Code

    #include<iostream>
    #include<stdio.h>
    #include<algorithm>
    #include<string.h>
    #include<cmath>
    using namespace std;
    const int maxn=3e4+100;

    int n,m1,m2,minn=1<<15;
    bool flag=0,here;
    int z[maxn],z1[maxn],p[maxn];  //z和z1储存质因子的指数
    int tail=0,cell;

    int vis[maxn];
    void prime() //素筛
    {
      vis[1]=1;
      for(int i=2;i<=maxn;i++){
        if(!vis[i]) p[++tail]=i;
        for(int j=1;j<=maxn&&i<=maxn/p[j];j++){
          vis[i*p[j]]=1;
          if(i%p[j]==0) break;
        } 
      }
    }
    int main()
    {
      prime();
      cin>>n; cin>>m1>>m2;
      if(m1==1){
          puts("0"); exit(0);
      }
      for(int i=1;i<=tail&&m1!=1;i++){  //质因子分解
          while(m1%p[i]==0){
            z[i]+=m2;
            m1/=p[i];
          }
      }
      for(int i=1;i<=n;i++){
        cin>>cell; int j=1; int Max=-1; here=1;
        memset(z1,0,sizeof(z1));  //注意初始化
        for(int j=1;j<=tail&&cell!=1;j++){
            while(cell%p[j]==0){
              z1[j]++;
              cell/=p[j];
            }
        }
        for(j=1;j<=tail;j++)
        {
          if(z[j]!=0&&z1[j]==0){
            here=0; break;
          }
          if(z[j]!=0&&z1[j]!=0) Max=max(Max,(z[j]-1)/z1[j]+1); //小技巧向上取整
        }
        if(here) flag=1,minn=min(minn,Max);
      }
      if(flag) cout<<minn<<endl;
      else cout<<-1<<endl;
      return 0;
    }