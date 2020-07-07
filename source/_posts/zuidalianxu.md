---
title: 最大连续和
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-04-12 13:09:14
tags: 
keywords:
description:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/19.webp
---

## 题目描述：
Maxsum

Given a sequence a[1],a[2],a[3]......a[n], your job is to calculate the max sum of a sub-sequence. For example, given (6,-1,5,4,-7), the max sum in this sequence is 6 + (-1) + 5 + 4 = 14. 

Input

The first line of the input contains an integer T(1<=T<=20) which means the number of test cases. Then T lines follow, each line starts with a number N(1<=N<=100000), then N integers followed(all the integers are between -1000 and 1000). 

Output

For each test case, you should output two lines. The first line is "Case #:", # means the number of the test case. The second line contains three integers, the Max Sum in the sequence, the start position of the sub-sequence, the end position of the sub-sequence. If there are more than one result, output the first one. Output a blank line between two cases. 

Sample Input

2

5

6 -1 5 4 -7

7

0 6 -1 1 -6 7 -5

Sample Output

Case 1:

14 1 4

Case 2:
7 1 6
## 思想
用summax变量存储从开始到当前的最大连续和，用k这个变量表示求累加和的开始位置，从第一个数开始遍历，求累加和，当此时累加和大于summax时，就要更新s_id和end_id，并更新summax，如果当前累加和<0了，则需要重新开始算，把当前位置赋值给k，并更新sum为0
## 代码：

    #include<stdio.h>
    #include<iostream>
    using namespace std;
    int arr[100010];
    int main(){
      int t,cnt=0;
      cin>>t;
      while(t--){
        int n,s_id=0,e_id=0,sum=0,summax=-10000,k=1;
        scanf("%d",&n);
        for(int i=1;i<=n;i++){
          scanf("%d",&arr[i]);
        }
        for(int i=1;i<=n;i++){
          sum+=arr[i];
          if(sum>summax){
            summax=sum;
            s_id=k;
            e_id=i;
          }
          if(sum<0){
            sum=0;
            k=i+1;
          }
        }
        printf("Case %d:\n%d %d %d\n",++cnt,summax,s_id,e_id);
        if(t!=0) printf("\n");
      }	
    }

## 第二种思想
时间复杂度为O（n）的方法：

对于每一个数，将这个数与前几个数的和和下一个数进行比较，如果前者大的话，就取前面的那个，否则就从下一个数开始。这样保证每次加的数的和都是最大的（如果不是最大的，就从下一个数开始重新计算了），所以，特别神奇吧。

## 代码：
    #include <iostream>
    #include <stdio.h>
    #include <string.h>
    #include <string>
    #include <stdlib.h>
    #include <vector>
    #include <algorithm>
    #include <math.h>
    using namespace std;
    const int MAXN=50050;
    int n,ans=-2147483647,dp[MAXN],a[MAXN]； 
    int main()
    {
        scanf("%d",&n);
        for (int i=1;i<=n;i++) 
        {
            scanf("%d",&a[i]);
            dp[i]=max(dp[i-1]+a[i],a[i]); //关键
            ans=max(ans,dp[i]);
        }
        printf("%d\n",ans);
        return 0;
    } 