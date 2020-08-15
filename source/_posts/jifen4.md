---
title: jixun4
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/68.webp'
date: 2020-08-10 22:53:10
authorLink:
categories: 题目
tags: 集训
keywords:
description: 积分赛4
---

[题目面板](https://pan.baidu.com/s/1aWuvSuQW7LylPAX3pNOn4Q) 提取码：k2fu

##  **A.** 胡图图的数学难题

这道题很有意思让你求斐波那契数列的平方和，[一篇易懂的题解](https://blog.csdn.net/lanchunhui/article/details/51840616?utm_source=blogxgwz5) 得到公式以后直接一个矩阵快速幂就解决了

```c
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long LL;
const LL MOD = 1e9 + 7;
struct node{
	LL mat[2][2];
	node(){
		memset(mat , 0 , sizeof mat);
	}
	void Set(){
		for(int i = 0 ; i < 2 ; ++ i)
			mat[i][i] = 1;
	}
	node operator * (node b){
		node res;
		for(int i = 0 ; i < 2 ; ++ i){
			for(int j = 0 ; j < 2 ; ++ j){
				for (int k = 0 ; k < 2 ; ++ k){
					res.mat[i][j] = (res.mat[i][j] + mat[i][k] * b.mat[k][j]) % MOD;
				}
			}
		}
		return res;
	}
};
node fastpow(node a , LL b){
	node res;
	res.Set();
	while(b){
		if(b&1) res=res*a;
		a=a*a;
		b>>=1;
	}
	return res;
}
int main ()
{
	LL n;
	cin >> n;
	if(n == 1) cout << 1 << endl;
	else{
		node a;
		a.mat [0][0] = a.mat [0][1] = a.mat [1][0] = 1;
		a = fastpow(a , n);
		cout << a.mat [0][0] * a.mat [1][0] % MOD << endl;
	}
	return 0;
}
```

##  **E.** 胡图图的难题

1) 让最小的带最大的过去，最小的回来 fi = fii1 + ai + *a*1

2) 让最小的带第二小的过去，最小的回来，最大的两个过去，第二小的回来 *fi* = *fii* 2 + *ai* + a*1 + *a*1 + *a*2 *∗* 2

综上所述，fi = *min*(*fii* 1 + *ai* + *a*1*, fi*2 + *ai* + *a*1 + *a*2 *∗* 2)

```c
#include <bits/stdc ++.h>
using namespace std;
const int mx =100000+47;
long long n,a[mx],f[mx];
int main () {
    scanf("%lld",&n);
    for(int i=1;i<=n;i++) scanf("%lld",&a[i]);
    sort(a+1,a+n+1);
    f[1]=a[1];
    f[2]=a[2];
    f[3]=a[3]+a[2]+a[1];
    for(int i=4;i<=n;i++)
    f[i]= min(f[i -2]+a[i]+a[1]+(a[2]<<1),f[i -1]+a[i]+a[1]);
    printf("%lld\n",f[n]);
}
```

## F 胡图图找队友

### CODE1

维护一个数组存储每一个点和根节点的关系，是否属于一个集合，因此每次只要通过根节点这个媒介联系两个结点的关系

[详细题解](https://blog.csdn.net/freezhanacmore/article/details/8774033)

```c
#include<cstdio>
#include<iostream>
#include<algorithm>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define debug freopen("in.txt","r",stdin); freopen("out.txt","w",stdout)
using namespace std;
typedef long long ll;
const int MAXN=1e3+100;
int fa[MAXN],r[MAXN];
void init(int n){
	for(int i=0;i<=n;i++){
		fa[i]=i;
		r[i]=0;
	}
}
int find(int x){
	if(x==fa[x]) return x;
	int tmp=fa[x]; //备份父亲节点
	fa[x]=find(fa[x]); //递归特性先更新最里面的东西，因此在下面一行更新之前，父亲结点已经发生改变，同理递归深层也是如此，所以如今父亲节点记录的直接是和祖宗的关系
	r[x]=(r[x]+r[tmp])%2; //因为压缩路径本质就是直接找到当前结点和根节点的关系，所以r数组记录的也要是和根节点的关系
	return fa[x]; //fa[x]现在已经是根节点了，返回根节点
}
void Union(int a,int b,int c){
	int x=find(a), y=find(b);
	if(x!=y){
		fa[y]=x;
		if(c) r[y]=(r[a]+r[b]+1)%2;
		else r[y]=(r[a]+r[b])%2;
	}
}
int main()
{
	// ios;
	// debug;
	int t,n,m;
	scanf("%d%d%d",&n,&m,&t);
	init(n);
	char c;
	int x,y;
	while(m--){
        scanf("%c%d%d",&c,&x,&y);
		if(c=='B') Union(x,y,0);
		else Union(x,y,1);
	}
	while(t--){
		int a,b;
		scanf("%d%d",&a,&b);
		if(find(a)==find(b)){
			if(r[a]==r[b]) printf("Yes\n");
			else printf("No\n");
		}
		else printf("Not sure\n");
	}

	return 0;
}
```

### CODE2

第二种思路，前n个作为第一个集合，后n个作为第二个集合，具体查看[题解](https://blog.csdn.net/u011008379/article/details/50999448?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)

```c
#include<cstdio>
#include<iostream>
#include<algorithm>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define debug freopen("in.txt","r",stdin); freopen("out.txt","w",stdout)
using namespace std;
typedef long long ll;
const int MAXN=1e3+100;
int fa[MAXN*2];
void init(int n){
	for(int i=0;i<=n;i++) fa[i]=i;
}
int find(int x){
	if(x==fa[x]) return x;
	return fa[x]=find(fa[x]);
}
void Union(int a,int b){
	int x=find(a), y=find(b);
	if(x!=y) fa[y]=x;
}
int main()
{
	int t,n,m;
	scanf("%d%d%d",&n,&m,&t);
	init(2*n);
	char c;
	int x,y;
	while(m--){
        scanf(" %c%d%d",&c,&x,&y);
		if(c=='B'){
			Union(x,y);
			Union(x+n,y+n);
		}
		else{
			Union(x+n,y);
			Union(x,y+n);
		}
	}
	while(t--){
		int a,b;
		scanf("%d%d",&a,&b);
		if(find(a)==find(b)||find(a+n)==find(b+n)) printf("Yes\n");
		else if(find(a+n)==find(b)||find(a)==find(b+n)) printf("No\n");
		else printf("Not sure\n");
	}
	return 0;
}
```

##  **I.** 胡图图吃桃子

完全背包问题，算是比较模板，不过你得想到完全背包%%%

[详细点击我](https://www.luogu.com.cn/problem/solution/P1679)

```c
#include<cstdio>
#include<cstring>
#include<cmath>
#define min(x,y) (x<y?x:y)
const int MAXN=200010;
int s[MAXN],f[MAXN];
int m;

int main()
{
    scanf("%d",&m);
    for(int i=1;i<=m;i++)
        f[i]=1e8;
    int n=ceil(sqrt(sqrt(m))+1);
    for(int i=1;i<=n;i++)
        s[i]=i*i*i*i;
    for(int i=1;i<=n;i++)
        for(int j=s[i];j<=m;j++)
            f[j]=min(f[j],f[j-s[i]]+1);
    printf("%d\n",f[m]);
}
```

