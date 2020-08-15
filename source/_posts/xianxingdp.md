---
title: 线性dp
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/71.webp'
date: 2020-08-03 11:19:11
authorLink:
categories: 题目
tags: dp
keywords: 
description: 这里记录以后碰到的典型dp问题
---

## Common Subsequence

题目原型这里不贴了，只说一下题目意思

给你两个字符串找到里面最长的公共子串长度，当然要你找出这个子串套用这个转移方程也是能做的，这里只贴一下样例：

>```tex
>abcfbc         abfcab
>programming    contest 
>abcd           mnp
>```
>
>```tex
>4
>2
>0
>```

状态转移方程：

如果s1[i]==s2[j] 则 c[i] [j]=c[i-1] [j-1]+1

如果s1[i]!=s2[j] 则 c[i] [j]=max(c[i-1] [j],c[i] [j-1])

{% fb_img http://img.51nod.com/upload/000FBEAF/08D25D565D85EFF40000000000000002.jpeg %}

所以这道题用二维数组存一下，就解决了

### CODE

```c
#include<iostream>
#include<cstring>
#include<cstdio>
using namespace std;
char s1[1005],s2[1005];
int dp[1005][1005];
int main(){
    while(~scanf("%s%s",s1+1,s2+1)){
        int len1=strlen(s1+1);
        int len2=strlen(s2+1);
        for(int i=0;i<=len2;i++) dp[0][i]=0;
        for(int i=0;i<=len1;i++) dp[i][0]=0;
        for(int i=1;i<=len1;i++){
        	for(int j=1;j<=len2;j++){
                if(s1[i]==s2[j])
                    dp[i][j]=dp[i-1][j-1]+1;
                else
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
		}
        printf("%d\n",dp[len1][len2]);
    }
    return 0;
}
```

## Jumping Cows

题意：n个数，从前往后挑，当前挑的是奇数个就往前走，挑的是偶数个就往后推，问？最远到达哪里？

> **样例**
>
> ```
> 8 //8个数
> 7
> 2
> 1
> 8
> 4
> 3
> 5
> 6
> ```
>
> ```
> 17 //7-1+8-3+6
> ```

这道题可以贪心可以dp，贪心比较好想而且容易理解

<font colot=red size=3>贪心</font>

{% fb_img https://images0.cnblogs.com/blog/494784/201308/06211533-2df9d37b2b454daabde15f6bfce1cb89.png %}

如图，从1开始到4之前一直上升，4是波峰，然后到2时是波谷，到了5时又是波峰，所以最大值=4-2+5=7；

还要注意的是如果到达了数列的末尾，如果此时需要加数，那当然是加上结尾一个数，因为它是结尾附近最大的数；如果是需要减数，则不需要减了，这样可以保持当前值最大。

<font colot=red size=3>DP</font>

 dp[i]\[0] 表示 当拿到第i个药时，之前已经拿了奇数个药的最大弹跳力。

 dp[i]\[1] 表示 当拿到第i个药时，之前已经拿了偶数个药的最大弹跳力。

### CODE

```c
#include<iostream>
#include<stdio.h>
#include<string.h>
using namespace std;
typedef long long ll;
const int MAXN=150010;
ll val[MAXN];
int main()
{
	int n;
	cin>>n;
	for(int i=1;i<=n;i++) cin>>val[i];
	ll maxn=-1,minn=1e9,ans=0,flag=0;
	for(int i=1;i<n;i++){
//		cout<<i<<'\n';
		if(val[i+1]<val[i]){
			maxn=val[i]; //若当前是峰值，则开始进入波谷，并且记录下来峰值
			int j;
			for(j=i+1;j<n;j++){ //进入波谷
				if(val[j+1]>val[j]){ //波谷结束
					minn=val[j]; //记录波谷值
					ans+=maxn-minn; //加入答案
					i=j; //更新i从下一个位置开始遍历
					break;
				}
			}
			if(j==n){ //如果结束的时候是以递减形式结束，则不需要减值了
				ans+=maxn;
				flag=1; //标记
				break;
			}
		}
	}
	if(!flag) ans+=val[n]; //如果不是以递减形式结束需要加上最后一个值
	cout<<ans<<'\n';
	return 0;
}
/*
6
5 2 3 4 2 3
*/
```

方程就是:

dp[i]\[0] = max(dp[i-1]\[0], dp[i-1]\[1]- v[i]);

dp[i]\[1] = max(dp[i-1]\[1], dp[i-1]\[0]+v[i]);

### CODE

```c
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
int dp[151000][2];
int maxd(int a,int b)
{
    return a<b?b:a;
}
int main()
{
    int n,i,t;
    while (scanf("%d",&n) != EOF)
    {
        dp[0][0]=0;
        dp[0][1]=0;
        for (i=1; i<=n; i++)
        {
            scanf("%d",&t);
            dp[i][0]=maxd(dp[i-1][0],dp[i-1][1]-t);
            dp[i][1]=maxd(dp[i-1][1],dp[i-1][0]+t);
        }
        printf("%d\n",maxd(dp[n][0],dp[n][1]));
    }
}
```

## 饭卡

电子科大本部食堂的饭卡有一种很诡异的设计，即在购买之前判断余额。如果购买一个商品之前，卡上的剩余金额大于或等于5元，就一定可以购买成功（即使购买后卡上余额为负），否则无法购买（即使金额足够）。所以大家都希望尽量使卡上的余额最少。
某天，食堂中有n种菜出售，每种菜可购买一次。已知每种菜的价格以及卡上的余额，问最少可使卡上的余额为多少。

既然卡上有5元就可以购买任何菜品，那不如我们就用原钱数减去5始终最少保留5元去买最贵的，剩下的用一下01背包找到最大值

### CODE

```c
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
const int MAXN = 1e3+100;
const int MOD = 1e9;
int dp[MAXN],w[MAXN],c[MAXN]; 
int main()
{
    ios;
    int n,m;
    while(cin>>n&&n){
    	memset(dp,0,sizeof dp);
    	for(int i=1;i<=n;i++) cin>>w[i];
    	cin>>m;
    	sort(w+1,w+1+n);
    	for(int i=1;i<n;i++){
    		for(int j=m-5;j>=w[i];j--){
    			dp[j]=max(dp[j],dp[j-w[i]]+w[i]);
			}
		}
		if(m<5) cout<<m<<'\n';
		else cout<<m-dp[m-5]-w[n]<<'\n';
	}
	return 0;
}
 
```

## Robberies

题意：一个小偷要抢银行，给定一个被抓的概率，以及准备抢的银行数量，然后是这些银行的金额和抢劫被抓风险，问在不被抓到的情况下最多抢到多少钱？

和一般01背包一样，只不过这里的抢劫风险(背包容量是小数)，这样一来就不能做了，开始我在想咋把小数转化成整数，想了好久发现是不可行的，所以这道题要转换思路，我们要求的是在一定风险内偷盗的最大金额，现在我们转换为一定金额内的最大逃跑几率，最后从大到小遍历金额当一种金额可行时就输出并break

### CODE

```c
#include <bits/stdc++.h>
using namespace std;
const int MAXN=1e3+10;
double c[MAXN],dp[1000000],m;
int w[MAXN];
int main()
{
    int t,n; cin>>t;
    while(t--){
        memset(dp,0,sizeof(dp));
        int sum=0;
        cin>>m>>n;
        m=1-m; //逃跑几率
        for(int i=1;i<=n;i++){
            cin>>w[i]>>c[i];
            c[i]=1-c[i]; //逃跑几率
            sum+=w[i]; //因为要求出每一种金额的最大逃跑几率，所以应该求出所有金额的总量
        }
        dp[0]=1; //注意初始化，当偷0金额时不用逃跑的，所以逃跑几率是1
        for(int i=1;i<=n;i++){
        	for(int j=sum;j>=c[i];j--){
            	dp[j]=max(dp[j],dp[j-w[i]]*c[i]);
			}
		}
        for(int i=sum;i>=0;i--){ //从最大金额开始遍历，找到第一个可以逃跑的
            if(dp[i]>=m){ 逃跑几率大于最低逃跑几率
                cout<<i<<endl;
                break;
            }
        }
    }
    return 0;
}
```

##  钱币兑换问题

> 在一个国家仅有1分，2分，3分硬币，将钱N兑换成硬币有很多种兑法。请你编程序计算出共有多少种兑法。
>
> 每行只有一个正整数N，N小于32768。
>
> 对应每个输入，输出兑换方法数
>
> ```
> 2934
> 12553
> ```
>
> ```
> 718831
> 13137761
> ```

仔细分析，N相当于容量，而硬币面值相当于重量和价值，不过要求的不是最大价值，求的是组合数，dp[i]\[j]表示前i种硬币组成j块钱的最大组合数，那么dp[i]\[j]==dp[i-1]\[j]+dp[i]\[j-i]，前一个意义：不用第i种硬币组合成为j元的组合数，后面指的是至少用了i种硬币组成j-i元的组合数，那你问dp[i-1]\[j]为啥不表示至少用第i-1种硬币呢？说实话我也不太清楚，感觉跟从小往大推有关系，当没有更新到时表示至少用了第i种硬币，更新过了就变成了所有组合数。。但是不知道咋证明

```c
#include<bits/stdc++.h>
using namespace std;
const int MAXN=510;
const int INF=0x3f3f3f3f;
int c[MAXN],w[MAXN],dp[32868];
int main()
{
	memset(dp,0,sizeof dp);
	dp[0]=1;
	for(int i=1;i<=3;i++){
		for(int j=i;j<=32768;j++){
			dp[j]=dp[j]+dp[j-i];
		}
	}
	int n;
	while(scanf("%d",&n)!=EOF){
		printf("%d\n",dp[n]);
	}
	return 0;
}
```

## CD

[题目链接](https://vjudge.net/contest/387630#problem/G)

这一道题需要你记录选中了哪些数！！！挺有意思的啊，题意是给你一个数n，还有m个数，要求这m个数里面挑出一些它们的和最接近n，并输出这个和还有选中的数，注意和不能大于n。

如果只要求和，那简单01背包就搞定了，但是现在要求选中的数，那就需要标记一下，每次更新时都在二维数组标记这个位置，表示这个位置更新了，最后输出时从后往前遍历，找到标记的点，具体看代码

### CODE

```c
#include<bits/stdc++.h>
using namespace std;
const int MAXN=40000;
const int INF=0x3f3f3f3f;
int c[MAXN],dp[40000],vis[30][MAXN];
int main()
{
	int n,m;
	while(cin>>n>>m){
		memset(vis,0,sizeof vis);
		memset(dp,0,sizeof dp);
		for(int i=1;i<=m;i++) cin>>c[i];
		for(int i=1;i<=m;i++){
			for(int j=n;j>=c[i];j--){ //01背包
				if(dp[j]<=dp[j-c[i]]+c[i]){
					dp[j]=dp[j-c[i]]+c[i];
					vis[i][j]=1; //更新了就标记
				}
			}
		}
		int j=n,tail=0;
		for(int i=m;i>=1;i--){
			if(vis[i][j]){
				cout<<c[i]<<' ';
				j-=c[i]; //注意这里要更新
			}
		}
		cout<<"sum:"<<dp[n]<<'\n';
	}
	return 0;
}
```

