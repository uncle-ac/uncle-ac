---
title: 校赛第七场汇总
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-10 18:12:06
tags: 比赛
keywords:
description: 一些含有重要思想的题
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/44.webp
---
## 合并数字
[题目链接](https://vjudge.net/contest/362412#problem/A)
### 分析  
这道题用栈模拟一下就行了
### CODE
```
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<vector>
#include<stack>
using namespace std;
int main()
{
	stack<int> s;
	int n,cnt=0;
	cin>>n;
	for(int i=1;i<=n;i++){
		int t;
		cin>>t;
		while(s.size()&&s.top()-t==1){
			cnt++; s.pop();
		}
		if(s.size()&&t-s.top()==1) cnt++;
		else s.push(t);
	}
	cout<<cnt<<endl;
}
```
## REd AND BLACK
[题目链接](https://vjudge.net/contest/362412#problem/C)
### 分析
一个简单的DFS
### CODE
```
#include<iostream>
#include<stdio.h>
#include<string>
#include<algorithm>
using namespace std;
char s[30][30];	
int m,n,sum;
int dfs(int x,int y){
	if(x+1>n||x+1<1||y+1>m||y+1<1) return 0;
	if(s[x][y]=='#') return 0;
	else{
		s[x][y]='#';
		return 1+dfs(x+1,y)+dfs(x-1,y)+dfs(x,y+1)+dfs(x,y-1);
	}
}
int main()
{

	while(scanf("%d %d",&m,&n)!=EOF){
		if(m==0&&n==0) break;
		sum=0;
		for(int i=0;i<n;i++){
			scanf("%s",s[i]);
		}
		for(int i=0;i<n;i++){
			for(int j=0;j<m;j++){
				if(s[i][j]=='@'){
					sum=dfs(i,j);
					goto skip;
				}
			}
		}
		skip:printf("%d\n",sum);
	}
}
```
## 棋盘问题
[题目链接](https://vjudge.net/contest/362412#problem/E)
### 分析
水题，DFS过了
### CODE
```
#include<stdio.h>
#include<string.h>
#include<algorithm>
using namespace std;
char map[9][9];
int vis[50],n,k,ans;
void dfs(int m,int x)
{
    if(x==k)
    {
//    	printf("%d\n",m);
        ans++;
        return ;
    }
    for(int i=m;i<=n;i++)
        for(int j=0;j<n;j++)
    {
        if(map[i][j]=='.'||vis[j]==1) continue;
        vis[j]=1;
        dfs(i+1,x+1);
        vis[j]=0;
    }
}
int main()
{
    while(~scanf("%d%d",&n,&k))
    {
        if(n==-1&&k==-1)
            break;
        for(int i=1;i<=n;i++)
            scanf("%s",&map[i]);
        memset(vis,0,sizeof(vis));
        ans=0;
        dfs(1,0);
        printf("%d\n",ans);
    }
    return 0;
}
```
## F - Super Jumping! Jumping! Jumping!(重要)
[题目链接](https://vjudge.net/contest/362412#problem/F)
## 分析
最大上升子序列，开一个数组没一个数组存储的值代表这之前最大的序列长度，用一个类似冒泡排序的比较来更新当前值就行了
## CODE
```
#include<stdio.h>
#include<string.h>
#include<algorithm>
using namespace std;
typedef long long ll;
const int N=1e6;
ll sum[N],arr[1010],n,m;
int main()
{
    while(scanf("%d",&n)!=EOF){
    	if(n==0) break;
    	memset(sum,0,sizeof sum);
    	for(int i=1;i<=n;i++) scanf("%d",&arr[i]);
	 	for(int i=1;i<=n;i++){
			for(int j=0;j<i;j++){
				if(arr[i]>arr[j]) 
				sum[i]=max(sum[i],sum[j]+arr[i]);
			}
		}   
		ll mx=0;
		for(int i=1;i<=n;i++){
			mx=max(mx,sum[i]);
		}	
		printf("%lld\n",mx);
	}

    return 0;
}
```
## H - WeirdSort(重要)
[题目链接](https://vjudge.net/contest/362412#problem/F)
## 分析
略
## CODE
```
#include<iostream>
#include<cstdio>
#include<cmath>
#include<cstring>
#include<algorithm>
#include<vector>
using namespace std;
const int MAXN=200;
int a[MAXN],p[MAXN],a1[MAXN];
void swap(int &a,int &b){
	int t=a;
	a=b;
	b=t;
}
int main()
{
	int t;
	cin>>t;
	while(t--){
		int m,n;
		cin>>m>>n;
		for(int i=1;i<=m;i++){
			cin>>a[i]; a1[i]=a[i];
		}
		for(int j=1;j<=n;j++) cin>>p[j];
		sort(p+1,p+1+n);
		int flag=0;
		while(1){
			int flag=0;
			int cnt=1;
			for(int i=cnt;i<=n;i++){
//				cout<<"i="<<i<<endl;
				if(a[p[i]]>a[p[i]+1]){
					swap(a[p[i]],a[p[i]+1]);
					flag=1;
				}
			}
			if(flag==0) break;
		}
		sort(a1+1,a1+1+m);
		for(int i=1;i<=m;i++){
			if(a1[i]!=a[i]){
				flag=1;break;
			}
		}
		if(flag==1) printf("NO\n");
		else printf("YES\n");
	}
}
```
## Integer Divisibility (重要余数方面)
[题目链接](https://vjudge.net/contest/362412#problem/I)
## 分析
重要的就是一句代码：`ans%=n;`就是这一句体现了同余原理，避免了大整数溢出
## CODE
```
#include<iostream>
#include<algorithm>

using namespace std;

int main(){
	int n,m,t;
	long long ans;
	cin>>t;
	int sum=0;
	while(t--){
	cin>>n>>m;
		ans=m;
		int s=0;
		while(ans%n!=0){
			ans=ans*10+m;
			//cout<<ans<<endl;
			ans%=n; //核心代码
			s++;
		}
		cout<<"Case "<<++sum<<": "<<++s<<endl;
	}
}
```
## Pie（实数区间上的二分）
[题目链接](https://vjudge.net/contest/362412#problem/J)
### 分析
这道题不看题解很难想到用二分去做，我第二次做这道题还是没想到，一只再找规律，其实这道题是找出一个区间，在区间内二分找出最大的满足条件的值，不过需要zhida注意这里是在实数区间上二分每次`不能l=mid+1或者r=mid-1`，以及二分条件必须是一个误差值:`r-l>误差值`，还有如何划分也是一个问题：每一个蛋糕体积除以mid就是该蛋糕最多分配给多少人，求一个和如果该和大于(f+1)就表示满足了条件，继续往右找👉，直到不满足条件为止，当不满足条件就需要减少mid的值往左找👈，减少每一个人分到的蛋糕数
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define pi acos(-1)
using namespace std;
const int MAXN=1e4+100;
double a[MAXN];
int main()
{
	int t; cin>>t;
	while(t--){
		memset(a,0,sizeof a);
		int n,f; cin>>n>>f;
		double sum=0;
		for(int i=1;i<=n;i++){
			int tt; cin>>tt;
			a[i]=pi*tt*tt;
			sum+=a[i];
		}
		double l,r,mid;
		l=0; r=sum/(f+1);
		while(1e-5<r-l){
			int sum2=0;
			mid=(l+r)/2;
			for(int i=1;i<=n;i++){
				sum2+=(int)(a[i]/mid);  //判断划分后该取前半部分还是后半部分
			}
			if(sum2>=(f+1)){
				l=mid;
			}else{
				r=mid;
			}
		}
		printf("%.4lf\n",mid);
	}

	return 0;
}
```
## M - Complete the Sequence（重要）
[题目链接](https://vjudge.net/contest/362412#problem/M)
### 分析
这道题其实规律很明显，很容易就能想到求差分数组（跟差分没有关系），前一个值减去当前值储存到新数组中，就是一个倒三角，直到倒三角的底部，然后往右复制c个数，再往上推就行了，就是实现代码比较数据关系比较费脑，图：  
<% fb_img https://cdn.jsdelivr.net/gh/uncleacc/img/sucai/2.png %>
### CODE
```
#include<stdio.h>
#include<iostream>
#include<string.h>
#include<algorithm>
using namespace std;
const int MAXN=110;
int a[MAXN][MAXN];
int main()
{
	int t; cin>>t;
	while(t--){
		int n,c;
		cin>>n>>c;
		for(int i=1;i<=n;i++){
			cin>>a[0][i];
		}
		int cnt=1;
		for(int i=1;i<n;i++){
			for(int j=1;j<=n-cnt;j++){
				a[i][j]=a[i-1][j+1]-a[i-1][j];
			}
			cnt++;
		}
		for(int i=1;i<=c;i++){
			a[n-1][i+1]=a[n-1][i];
		}
		for(int j=1;j<=c;j++){
			cnt=j+1;
			for(int i=n-1;i>0;i--){
				a[i-1][cnt+1]=a[i][cnt]+a[i-1][cnt];
				cnt++;
			}
		}
		for(int i=1;i<=c;i++){
			printf("%d%c",a[0][n+i],i==c?'\n':' ');
		}
	}
	return 0;
}
```
## N - Harmonic Number(重要)
[题目链接](https://vjudge.net/contest/362412#problem/N)
## 分析
这道题含有一个重要思想就是：`当我们开不了很大的数组储存许多数时，可以每一百个或五十个去储存`，这个思想很重要，当时就没想到，想到了就是水题一道了
## CODE
```
#include<stdio.h>
#include<string.h>
#include<algorithm>
using namespace std;
#define N 1123456
#define M 100000005
double a[N];
void init()
{
    double sum=0;
    int z=1;
    a[0]=0;
    for(int i=1;i<M;i++)
    {
        sum+=(1.0/i);
        if(i%100==0)
            a[z++]=sum;
    }
}
int main()
{
    int n,m;
    init();
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
    {
        scanf("%d",&m);
        int x=m/100;
        double ans=a[x];
        for(int i=x*100+1;i<=m;i++){
        	ans+=(1.0/i);
		}           
        printf("Case %d: %.10lf\n",i,ans);
    }
    return 0;
}
```