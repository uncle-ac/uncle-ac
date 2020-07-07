---
title: 基础算法2(快速幂，二分)
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-24 09:21:54
tags: 基础算法练习
keywords:
description: 基础算法2题解
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/50.webp
---
>发现了一些快速幂上的小问题，可以说很细节的问题了，导致我第一题巨水的一道题wrong了5次！！当时都懵了，感觉代码一点毛病都没有🐷（菜是原罪）

<font color="red">
把这次我在快速幂模板上踩的坑说一下，看下面两段代码
</font>

```
代码一
ll qpow(ll a,ll b){
	if(b==0) return 1;
	ll ans=qpow(a,b>>1)%MOD;
	ans*=ans%MOD;
	if(b&1) ans*=a%MOD;
	return ans%MOD;
}
代码二
ll qpow(ll a,ll b){
	if(b==0) return 1;
	ll ans=qpow(a,b>>1)%MOD;
	ans=ans*ans%MOD;
	if(b&1) ans=ans*a%MOD;
	return ans%MOD;
}
```
<font color="red">
看着这两段代码没啥区别，就是把ans=ans*ans改成了ans*=ans，如果没有取模的话这俩没有任何区别，但是一旦取模就是AC和wrong的天壤之别，为什么？因为爆ll！首先看ans*=ans%MOD这一段首先计算ans%MOD，这是小于MOD的一个数，但是也有可能就比MOD小1，就把ans当作是MOD-1，然后再乘以ans，哇，(MOD-1)*(MOD-1)这是一个比MOD大几十倍的数，本来目的是最后算出一个小于MOD的数的！结果呵呵，这就是我的习惯了，我习惯把a=a*a写成a*=a，最后debug时真的直接就忽略了这个🙃
</font>

## Pseudoprime numbers
**题意:**  

如果p是非质数，算a^p^%p是否==a，是的话输出yes否则输出no，否则输出no

~~很水的题，我在坑里跳了5次，，一直以为是cin，cout又在捣鬼，换了几次一直爆零~~
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
ll MOD;
ll qpow(ll a,ll b){
	if(b==0) return 1;
	ll ans=qpow(a,b>>1)%MOD;
	ans=ans*ans%MOD;
	if(b&1) ans=ans*a%MOD;
	return ans%MOD;
}
int jug(ll p){
	for(int i=2;i*i<=p;i++){
		if(p%i==0) return 0;
	}
	return 1;
}
int main()
{
	ll p,a;
	while(scanf("%lld %lld",&p,&a)!=EOF ){
		if(a==p&&p==0) break;
		MOD=p;
		if(jug(p)) printf("no\n");
		else{
			if(qpow(a,p)==a) printf("yes\n");
			else printf("no\n");
		}
	}
	return 0;
}
```
## Raising Modulo Numbers
**题意**
T组，M，H小组

每小组给出a，b的值，求a1^b1^+ a2^b2^+ …+an^bn^之和mod M的值
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
ll MOD;
ll qpow(ll a, ll n) {
    ll res=1;
    while(n){
        if(n&1) res=res*a%MOD;
        a=a*a%MOD;
        n>>=1;
    }
    return res%MOD ;
}
int main()
{
	ll t,m,h; scanf("%lld",&t);
	while(t--){
		scanf("%lld%lld",&m,&h);
		ll sum=0;
		MOD=m;
		for(int i=1;i<=h;i++){
			ll a1,a2; scanf("%lld %lld",&a1,&a2);
			sum+=qpow(a1,a2);
		}
		printf("%lld\n",sum%MOD);
	}
	return 0;
}
```
## Key Set
**题意**
给你一个具有n个元素的集合S{1,2,…,n}，问集合S的非空子集中元素和为偶数的非空子集有多少个取模1000000007。  
**解**  
找规律，公式2^(n-1)^-1
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
ll MOD=1000000007;
ll qpow(ll a, ll n) {
    ll res=1;
    while(n){
        if(n&1) res=res*a%MOD;
        a=a*a%MOD;
        n>>=1;
    }
    return res ;
}
int jug(ll p){
	for(int i=2;i*i<=p;i++){
		if(p%i==0) return 0;
	}
	return 1;
}
int main()
{
	ll a,t; scanf("%lld",&t);
	while(t--){
		scanf("%lld",&a);
		a-=1;
		printf("%lld\n",qpow(2,a)-1);
	}
	return 0;
}
```
## Distribution money
**题意**
随意排队领薪金，如果一个人领的超过其他人的总和那么这个人将受到惩罚输出这个人的工号，如果没人领的薪金超过其他人的总和输出-1  
**解**
总共薪金是n，某个人可能领x，那么其他人的总和就是n-x，如果x>n-x即x>n/2，那么这个人领的薪金就超过其他人输出这个人的工号  
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
const int MAXN=1e4+100;
ll MOD;
ll qpow(ll a, ll n) {
    ll res=1;
    while(n){
        if(n&1) res=res*a%MOD;
        a=a*a%MOD;
        n>>=1;
    }
    return res%MOD ;
}
int b[MAXN];
int main()
{
	int n,maxx=-1e9,id;
	while(~scanf("%d",&n)){
		memset(b,0,sizeof b);
		id=-1;
		for(int i=1;i<=n;i++){
			int t;
			scanf("%d",&t);
			b[t]++;
		} 
		for(int i=1;i<=MAXN-10;i++){
			if(b[i]>n/2){
				id=i;
				break;
			}
		}
		printf("%d\n",id);
	}
	return 0;
}
```
## Rightmost Digit
**题意**
是输出一个数的几次方最右面的数字  
**解**
快速幂对10取余
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
ll MOD=10;
ll qpow(ll a, ll n) {
    ll res=1;
    while(n){
        if(n&1) res=res*a%MOD;
        a=a*a%MOD;
        n>>=1;
    }
    return res ;
}
int jug(ll p){
	for(int i=2;i*i<=p;i++){
		if(p%i==0) return 0;
	}
	return 1;
}
int main()
{
	ll a,t; scanf("%lld",&t);
	while(t--){
		scanf("%lld",&a);
		printf("%lld\n",qpow(a,a));
	}
	return 0;
}
```
## 人见人爱A^B
**题意**  
不说了  
**解**  
略  
### CODE
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
ll MOD=1000;
ll qpow(ll a, ll n) {
    ll res=1;
    while(n){
        if(n&1) res=res*a%MOD;
        a=a*a%MOD;
        n>>=1;
    }
    return res ;
}
int jug(ll p){
	for(int i=2;i*i<=p;i++){
		if(p%i==0) return 0;
	}
	return 1;
}
int main()
{
	ll p,a;
	while(scanf("%lld %lld",&a,&p)!=EOF ){
		if(a==p&&p==0) break;
		printf("%lld\n",qpow(a,p));
	}
	return 0;
}
```
## Trailing Zeroes (III)
**题意**  
给你一个数n，问你最小的哪个数的阶层包含n个零  
**解**  
这道题是最难的一道了！！~~我也不会，上网查了才知道~~  

首先我们要知道一个数的阶层中0的个数由谁来提供？一定是由2乘5得来的0，哎不对呀！4乘5不是也能得到0吗？对！但是4不也是由2乘2得来的吗？所以n的阶层中0的个数就是由1~n的因子中2和5的数量决定的，再仔细想因子2的数量一定是少于5的，随便想都是这，所以10的个数就由因子5的数量来决定  
比如：37的阶层  
5 10 15 20 25(2) 30 35  
上面一系列数都含有因子5，而25含有俩个因子5，所以总数是8个  

另外随着n的增大阶层中0的个数也不断增多，那这就是`有序`的，一看到有序，马上想起二分试错，二分n，如果n的阶层中0的数量不够，就去右边找，够了，要找最小，往左边找，思想就出来了，不过二分最难的不是思想，而是边界问题，整数上的二分最后一定是会到这样一种情况的left+1==right，因为每次都是right-1或者left+1，所以只要考虑好这种情况该怎么处理就行了  

再来看我们怎么算1~n中含有因子5的数量(n的阶层中0的数量)  
一个数N,从1~N中包含因子M的数量为N/M    
但是怎么去求1~N中因子M的数量呢？  
这句话和上面的不一样  
例如 N=36 M=5  
那么1~N中包含因子5的数有：  
5 10 15 20 25 30 35 （7）个  
但是1~N中每一个数的因子M的数量和：  
5(1) 10(1) 15(1) 20(1) 25(2) 30(1) 35(1)  
这里面的25可以拆成5*5，是有两个因子5的  
那如何求问题2呢？  
可以这样去想，N/M表示包含因子M的数量  
那N/=m之后就表示把从1~N里面的数包含因子M的数都除以了M  
那么上面的数列就变成1~(N/5)：  
1(0) 2(0) 3(0) 4(0) 5(1) 6(0) 7(5)  
其实把N/M可以当作是把1~N中含有因子M的数都筛掉一遍  
那么求1~N中含有因子M的数的的因子M的数量就是：  

代码：
```  
while(n){  
	ans+=(n/5);  
	n/=5;
}
```
### CODE
```

```
## Pie
**题意**  
N种蛋糕，每个半径给出，要分给F+1个人，要求每个人分的体积一样（形状可以不一样），而且每人只能分得一种蛋糕（即一个人得到的蛋糕只能来自一块），求每人最大可以分到的体积。  
**解**  
一道实数上的二分题，有点像进击的奶牛那一道，都是猜一个答案去试错  
double二分搜索

最小值是： 0 （大家都不吃）  
最大值是：派的总体积 ÷ 总人数  
每次取中间值 记作：mid，计算如果每个派切出 mid体积 能切多少块   
如果能切够 F+1块，则将最小值更新为中间值  
如果切不出 F+1块，则将最大值更新为中间值  
直到两次中间值的差值 小于 10^(-4) 时就是结果了（精度）  

需要注意实数上的二分缩小区间时是r=mid或者l=mid
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
				sum2+=(int)(a[i]/mid);
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
## Can you solve this equation?
**题意**  
给出一个等式，和y的值，x为0~100之间的值，求出x(精确到4位小数)  
**解**  
经典二分题从0到一百二分答案试错就行了,注意精度要合适，大了会超时，小了误差大，具体看代码  
### CODE
```
#include<stdio.h>
#include<iostream>
#include<math.h> 
using namespace std;
double Y;
double f(double x){
	return 8*pow(x,4)+7*pow(x,3)+2*pow(x,2)+3*x+6-Y;
}
int main(){
	int t;
	cin>>t;
	while(t--){
		cin>>Y;
		if(f(0)*f(100)>0) puts("No solution!");
		else{
			double l=0,r=100,mid;
			while(r-l>1e-8){
				mid=(l+r)/2;
				if(f(mid)<0) l=mid;
				else r=mid;
			}
			printf("%.4lf\n",mid);
		}
	}
}
```
## Subsequence
**题意**  
找到和大于等于给定数的最短子序列的长度  
**解**  
这道题不知道咋回事脑子抽了，~~我直接排序了~~，但是没注意到是子序列，但是wrong了还奇怪呢。。我真想抽自己一巴掌  
我用的是尺取，毕竟时尺取的经典例题  
二分的话预处理前缀和，然后二分后面的区间，大了往左看看能不能找到更小的，小了说明不够往右找 
### CODE(二分)
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
using namespace std;
typedef long long ll;
const int maxn=1e5+100;
int a[maxn];
int main()
{
	int t;
	cin>>t;
	while(t--){
		memset(a,0,sizeof a); 
		int n,s,len=1e9;
		cin>>n>>s;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			a[i]+=a[i-1];
		}
		if(a[n]<s){
			cout<<"0"<<'\n';
			continue;
		}
		for(int i=0;i<n;i++){
			int temp=a[i];
			int l=i+1,r=n,mid,len2=1e9;
			while(l<=r){
				mid=(l+r)>>1;
				if(a[mid]-temp<s) l=mid+1;
				else{
					len2=mid-i;
					r=mid-1;
				}
			}
			len=min(len,len2);
		}
		cout<<len<<'\n';
	}
	return 0;
}
```
### CODE(尺取)
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#include<cmath>
using namespace std;
typedef long long ll;
const int MAXN=1e5+100;
int a[MAXN];
int main()
{
	int t,n,s;
	scanf("%d",&t);
	while(t--){
		scanf("%d%d",&n,&s);
		for(int i=1;i<=n;i++) scanf("%d",&a[i]);
		int l=1,r=0,ans=1e9,sum=0;
		while(l<=n){
			while(r+1<=n&&sum<s) sum+=a[++r];
			if(sum>=s){
				ans=min(ans,r-l+1);
			}
			sum-=a[l];
			l++;
		}
		if(ans==1e9) printf("0\n");
		else printf("%d\n",ans);
	}	
	return 0;
}
```
<font color="pink" size=4>
总结：题目还是很友好的，毕竟是基础 233~
</font>