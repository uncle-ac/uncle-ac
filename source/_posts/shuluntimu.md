---
title: 数论题目集(协会)
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/54.webp
date: 2020-06-02 16:58:19
authorLink:
tags: 数论
keywords:
description: 数论基础部分
---
>以下题目涉及知识有 欧拉函数、素数筛、算数基本定理（唯一分解定理）

>因为数论之前没咋学，欧拉函数还是这两天补的，又要考试，时间不够，所以大多数都是直接搜题解做的，本来信誓旦旦好好写一些题解巩固一下的，发现越写越累，索性直接搬来别人的优质题解算了🤔

<font color="red">一定记得素数筛时isp数组要用bool，bool只占用一个字节，int4个会爆内存，卡死我了，我说咋一直爆内存</font>

## Bi-shoe and Phi-shoe
**题意**：  
给定N个数，让你求欧拉函数值大于等于这N个数的的那个数的最小数值之和（这里1的欧拉函数值很特殊，设置为0，因为小于1且与1互质的数量为0）  
例如：
N==2  
1 2  
则答案为4 == 1+3  
**思路**  
要求的是欧拉函数值大于等于给定数的最小数，那么我们就要让这个数对应的欧拉函数值尽可能大一点，什么情况下一个数的欧拉函数值最大呢？很明显是素数时！一个素数的欧拉函数值就等于这个数减一，从这里我们就能推出来最小的那个对应欧拉函数值~大于等于~给定数的那个数最小就是这个给定数后面的那个素数，例如： 10对应的就是11 ，12对应13 ，14对应17，11，13，17就是所要求的最小的三个数，由此思路就明确了
**思路2**   
还可以用筛法求1~N的欧拉函数，然后打表每个欧拉函数值的最优解，再取和最小
### CODE-1
```
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
const int MAXN = 1e7+10;
const int MOD = 1e9;
int pr[MAXN/10], tail = 0; 
bool isp[MAXN];
void prime() {  //一个素数筛
    isp[1] = 1;
    for (int i = 2; i < MAXN; i++) {
        if (!isp[i]) pr[++tail] = i;
        for (int j = 1; i <= MAXN/pr[j]; j++) {
            isp[i * pr[j]] = 1;
            if (i % pr[j] == 0) break;
        }
    }
}
int main()
{
	ios;
	prime();
  int t,kase=0; cin>>t;
  while(t--){
    int n; cin>>n;
    ll sum=0;
    for(int i=1;i<=n;i++){
      int x; cin>>x;
      for(int j=x+1; ;j++){ //找到给定数字后面的那个素数累加到sum里面
        if(!isp[j]){
          sum+=j;
          break;
        }
      }
    }
    cout<<"Case "<<++kase<<": "<<sum<<" Xukha"<<endl;
  }
  return 0;
}
```
### CODE-2
```
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
using namespace std;
typedef long long ll;
const int MAXN = 1e6+100;
const int MOD = 1e9;
int pr[MAXN/10],tail;
int phi[MAXN],ans[MAXN];
bool isp[MAXN]; 
void euler()
{
	phi[1]=0;
	for(int i=2;i<MAXN;i++){
		if(!isp[i]){
			pr[++tail]=i;
			phi[i]=i-1;
		}
		for(int j=1;j<=tail&&i*pr[j]<MAXN;j++){
			isp[i*pr[j]]=1;
			if(i%pr[j]==0){
				phi[i*pr[j]]=phi[i]*pr[j];
				break;
			}
			else phi[i*pr[j]]=phi[i]*phi[pr[j]];
		}
	}
	int cur=0;
	for(int i=2;i<MAXN;i++){  //核心代码，这里一定要保证ans是递增的，因为要往后走找更大的欧拉函数值
		if(phi[i]>cur&&ans[phi[i]]==0){ //ans下标代表欧拉函数值，储存的是该欧拉函数值对应的数字，ans[]==0起到防止相同欧拉函数值的两个数后面那个数把前面的覆盖了
			ans[phi[i]]=i;
			cur=phi[i]; //记得更新cur 因为要求最小的那个数字
		}
	}
}
int main()
{
	ios; ll n;
	euler();
	int t,kase=0;
	cin>>t;
	while(t--){
		ll sum=0;
		int n; cin>>n;
		for(int i=1;i<=n;i++){
			int x; cin>>x;
			for(int j=x; ;j++){
				if(ans[j]>0){
					sum+=ans[j];
					break;
				}
			}	
		}
		cout<<"Case "<<++kase<<": "<<sum<<" Xukha"<<'\n';
	}
    return 0;
}
```
## Aladdin and the Flying Carpet
**题意**  
给出矩形面积S，和组成该矩形的边的最小值，问这种面积为S的矩形有几种  
比如样例12 2，矩形面积为12，组成这样矩形的最小边为2，共有2种这样的矩形（2， 6），（3， 4）(这些边都大于或等于2，其中（2,6）和（6,2）是同一种)   
**思路**   
这道题用到了唯一分解定理：N = p1^a1^  p2^a2^ * p3^a3^ * ... *pn^an^(其中p1、p2、... pn为N的因子，a1、a2、... 、an分别为因子的指数)   
N的因子个数公式：     M = (1 + a1)* (1 + a2)* (1 + a3)* ...*(1 + an);   
用唯一分解定理求出ab的因子个数，但题要求的是满足条件的因子对数，所以最终所求的因子个数需要除以2，然后再将不满足的减去   
该题要用到筛选素数来缩短时间（减少循环次数）来防止TLE  
**疑问**
两个因子可以一样啊，25 ==5 ，5，那num不是应该=(num+1)/2吗？这道题自动排除了1和数本身的情况？
### CODE
```
#include<queue>
#include<math.h>
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<iostream>
#include<algorithm>
using namespace std;
typedef long long ll;
const int MAXN = 1e6+5;
bool isp[MAXN];
int pr[MAXN/10],tail;
int prime(){
	isp[1]=1;
	for(int i=2;i<MAXN;i++){
		if(!isp[i]) pr[++tail]=i;
		for(int j=1;j<=tail&&i*pr[j]<MAXN;j++){
			isp[i*pr[j]]=1;
			if(i%pr[j]==0) break;
		}
	}
}
ll fun(ll n){ //找出n的因子数量
	ll ans=1;
	for(ll i=1;i<=tail&&pr[i]*pr[i]<=n;i++){
		if(n%pr[i]==0){
			ll e=0;
			while(n%pr[i]==0){
				e++;
				n/=pr[i];
			}
			ans*=(1+e);//算数基本定理的经典问题：求一个数的因子数量
		}
	}
	if(n>1) ans*=2;
	return ans;
}
int main()
{
	prime();
  int t,kase=0;
  scanf("%d",&t);
  while(t--){
    kase++;
    ll s,a;
    scanf("%lld%lld",&s,&a);
    if(a*a>s){
        printf("Case %d: 0\n", kase);
        continue;
    }
    ll num=fun(s);
    num/=2; //两个因子为一对，除2
    for(ll i=1;i<a;i++){
      if(s%i==0) num--;
    }
    printf("Case %d: %lld\n", kase, num);
	}
    return 0;
}
```
## Goldbach`s Conjecture
**题意**
给出几组测试数据，每组给出一个n，问n能被分成几对素数的和。   
**思路**
少有的水题，素数筛一下，遍历素数，然后每次查看sum-该素数是不是素数是的话答案加一，遍历过半就可以退出了
### CODE
```
#include <stdio.h>
#include <iostream>
using namespace std;
typedef long long ll;
const int MAXN=1e7+20;
bool isp[MAXN];
int pr[700000];
int t,k=0;
void prime()
{
    isp[1]=1;
    for(int i=2;i<=MAXN;i++){
        if(!isp[i]){
            pr[++k]=i;
            for(int j=i+i;j<=MAXN;j+=i){
                isp[j]=1;
            }
        }
    }
    return ;
}
int main()
{
	prime();
    int t,kase=0;
    scanf("%d",&t);
    while(t--){
    	int n,cnt=0;
    	scanf("%d",&n);
    	for(int i=1;i<=n/2;i++){
    		if(pr[i]>=n/2+1) break;
    		if(!isp[n-pr[i]]) cnt++;
		}
		printf("Case %d: ",++kase);
		cout<<cnt<<'\n';
	}
    return 0;
}
```
## Pairs Forming LCM
**题意**   
在1~n中有多少对数满足lcm(a,b)==n  
**思路**   
gcd(a,b)=p1 ^min(a1,b1)^ * p2^min(a2,b2)^ * .......... *pn^min(an,bn)^  
lcm(a,b)=p1 ^max(a1,b1)^ * p2^max(a2,b2)^ * .......... *pn^max(an,bn)^   
先对n素因子分解，n = p1^e1^ * p2^e2^ * .......... *pk^ek^，  
lcm(a,b)=p1 ^max(a1,b1)^ * p2^max(a2,b2^ * .......... *pk^max(ak,bk)^  

所以，当lcm(a,b)==n时，max(a1,b1)==e1,max(a2,b2)==e2,…max(ak,bk)==ek  
当ai == ei时，bi可取 [0, ei] 中的所有数  有 ei+1 种情况，bi==ei时同理。   
那么就有2(ei+1)种取法,但是当ai = bi = ei 时有重复，所以取法数为2(ei+1)-1=2*ei+1。   
除了 (n, n) 所有的情况都出现了两次  那么满足a<=b的有 (2*ei + 1)) / 2 + 1 个   
### CODE
```
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
const int MAXN = 1e7+10;
const int MOD = 1e9;
int pr[MAXN/10], tail = 0;
bool isp[MAXN];
void prime() {
    isp[1] = 1;
    for (int i = 2; i <= MAXN; i++) {
        if (!isp[i]) pr[++tail] = i;
        for (int j = 1; j<=tail &&i * pr[j] <= MAXN; j++) {
            isp[i * pr[j]] = 1;
            if (i % pr[j] == 0) break;
        }
    }
}
int main()
{
	prime();
    int t,kase=1;
    cin>>t;
    for( ;kase<=t;kase++){
    	ll n; cin>>n;
    	int ans=1;
    	for(int i=1;i<=tail&&pr[i]*pr[i]<=n;i++){
    		if(n%pr[i]==0){
    			int e=0;
    			while(n%pr[i]==0){
    				e++;
    				n/=pr[i];
				}
				ans*=(2*e+1);
			}
		}
		if(n>1) ans*=(2*1+1);
		printf("Case %d: %d\n",kase,(ans+1)/2);
	}
  return 0;
}
```
## Mysterious Bacteria
**题意**  
给你一个数x = b^p^,求p的最大值  
**思路**   
p = gcd(x1, x2, x3, ... , xs) xi是拆分后的指数  
比如： 24 = 2^3^*3^1^，p应该是gcd(3, 1) = 1,即24 = 24^1^  
      324 = 3^4^*2^2^,p应该是gcd(4, 2) = 2,即324 = 18^2^  
~~注意：~~本题有一个坑，就是x可能为负数，如果x为负数的话，x = b^q, q必须使奇数，所以将x转化为正数求得的解如果是偶数的话必须将其一直除2转化为奇数
**疑问**   
为什么代码标记的地方的那个n不开ll会超时呢？ll转换成负数快？
### CODE
```
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
using namespace std;
typedef long long ll;
const int MAXN = 1e5+10;
const int MOD = 1e9;
int pr[MAXN/10], tail = 0;
bool isp[MAXN];
void prime() {
    isp[1] = 1;
    for (int i = 2; i < MAXN; i++) {
        if (!isp[i]) pr[++tail] = i;
        for (int j = 1; i <= MAXN/pr[j]; j++) {
            isp[i * pr[j]] = 1;
            if (i % pr[j] == 0) break;
        }
    }
}
int gcd(int a,int b){
	if(a%b==0) return b;
	else return gcd(b,a%b);
}
int main()
{
	ios;
	prime();
    int t,kase=0; cin>>t;
    while(t--){
    	ll n; cin>>n;  //这里n必须开成ll否则会超时
    	int flag=0,ans=0;
    	if(n<0){
    		n=-n;  //n如果不开成ll的话这里会卡住
    		flag=1;
		}
    	for(int i=1;i<=tail&&pr[i]*pr[i]<=n;i++){
    		if(n%pr[i]==0){
				int e=0;
    			while(n%pr[i]==0){
    				e++;
    				n/=pr[i];
				}
				if(ans==0) ans=e;
				else ans=gcd(ans,e);
			}
		}
    	if(n>1) ans=gcd(ans,1);
    	if(flag==1){
    		while(ans%2==0){
    			ans/=2;
			}
		}
    	cout<<"Case "<<++kase<<": "<<ans<<'\n';
	}
    return 0;
}
```
## Large Division
**题意**  
给你两个数a，b，让你求出来a是否能够被b整除。  
**思路**  
需要注意的是数字a太大了，所以要用数组来存储，同时还要注意数字b可能超出了int范围，要用long long int，考的其实就是除法的模拟   
### CODE
```
#include <stdio.h>
#include <iostream>
#include <string.h>
using namespace std;
typedef long long ll;
const int MAXN=1e6+10;
char a[MAXN];
int main()
{
    int t,kase=0;
    scanf("%d",&t);
    while(t--){
    	memset(a,0,sizeof a);
    	ll b;
    	scanf("%s",a);
    	scanf("%lld",&b);
    	if(b<0) b=-b;
    	ll x=0,len=strlen(a);
    	for(int i=0;i<len;i++){
    		if(a[i]=='-') continue;
    		x=(x*10+a[i]-'0')%b;
		}
		if(x==0) printf("Case %d: divisible\n",++kase);
    	else printf("Case %d: not divisible\n",++kase);
    	
	}
    return 0;
}
```
## Help Hanzo
**题意**  
求区间a,b内素数的数量   
**思路**  
由于b极大，所以打表会爆内存。但并不意味着放弃打表，我们可以先打一个小点的素数表出来，如果b在这个表内直接二分找一下a,b就可以了。否则利用到b-a<=100000这个性质，可以开一个这么大的桶下标表示为j-a来筛选a-b内的素数，这样就用到我们之前的小素数表来筛选了。
### CODE
```
#include<iostream>
#include<cstdio>
#include<set>
#include<cmath>
#include<cstring>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<stack>
#include<algorithm>
typedef long long ll;
using namespace std;
const int MAXN=1e6+10;
bool vis1[MAXN],vis2[MAXN];
int pr[MAXN],cnt;
int prime(){
	for(int i=2;i<MAXN;i++){
		if(vis1[i]==0){
			pr[cnt++]=i;
			vis1[i]=1;
		}
		for(int j=0;j<cnt&&pr[j]*i<MAXN;j++){
	  		vis1[i*pr[j]]=1;
	  		if(i%pr[j]==0) break;
		}
	}
}
int main()
{
	prime();
  	int t,kase=1;
  	cin>>t;
  	while(t--){
		ll a,b,ans=0;
		cin>>a>>b;
		if(b<=MAXN){
		    int loab=lower_bound(pr,pr+cnt,b)-pr; 
		    if(pr[loab]>b) loab-=1;
		    int loaa=lower_bound(pr,pr+cnt,a)-pr;
		    ans=loab-loaa+1;
		}
		else{
		    memset(vis2,0,sizeof(vis2));
		    for(int i=0;i<cnt;i++){
		        ll k=(a%pr[i]==0)?a/pr[i]:a/pr[i]+1;  
		        for(ll j=k*pr[i];j<=b;j+=pr[i]) vis2[j-a]=1;
		    }
		    for(ll i=a;i<=b;i++){
		        if(!vis2[i-a]) ans++;
		    }
		}
		cout<<"Case "<<kase++<<": "<<ans<<endl;
  	}
}
```
## GCD - Extreme (II)
**题意**  
求在1-n之间所有任意两个数的的最大公因数的和。    
**题解**   
因为让求的1-n区间里任两个数的最大公因数之和，所以假设gcd(n,m)=z，在这里，因为n和m的范围都是超级大，所以，不能枚举n，m，但是可以枚举m，z，或者n，z。
具体思路是：假设gcd(x,y)=1,那么当执行到x，y的时候，最后的和都要加1，那么相应的，执行到2x，2y时，最后的和都要加2，以此类推，执行到kx，ky的时候最后的和都要加k，那么这些一切的根源都归咎于gcd(x,y)=1，所以才有了上面那一句话，枚举n，z（n，m选其一，无所谓的），这里的z就是上面的1，2，。。k。枚举z的问题解决了，那么轮到n了，枚举n，假设一个值为num，那么num代表与n的最大公因数是z（1，2，3，，，，k）的个数，这里的z有好多值，但是任何的z（大于1）都可以有最根本的gcd（x，y）推出，所以算出只需要算出z=1时num的值就可以了，这个时候，就会想到欧拉函数值（小于n的数里与n互质的个数）。然后，这道题算是结束了。
### CODE
```
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
using namespace std;
typedef long long ll;
const int MAXN = 4e6+10;
const int MOD = 1e9;
int pr[MAXN/10],tail;
ll a[MAXN],phi[MAXN];
bool isp[MAXN]; 
void euler()
{
	phi[1]=1;
	for(int i=2;i<MAXN;i++){
		if(!isp[i]){
			pr[++tail]=i;
			phi[i]=i-1;
		}
		for(int j=1;j<=tail&&i*pr[j]<MAXN;j++){
			isp[i*pr[j]]=1;
			if(i%pr[j]==0){
				phi[i*pr[j]]=phi[i]*pr[j];
				break;
			}
			else phi[i*pr[j]]=phi[i]*phi[pr[j]];
		}
		for(int j=1;i*j<MAXN;j++){
			a[i*j]+=phi[i]*j;
		}
	}
}
int fun(int n){
	int ans=n;
	if(n==1) return 1;
	for(int i=2;i*i<=n;i++){
		if(n%i==0){
			ans=ans/i*(i-1);
			while(n%i==0) n/=i;
		}
	}
	if(n>1) ans=ans/n*(n-1);
	return ans;
}
int main()
{
	ios; ll n;
	euler();
	for(int i=1;i<MAXN;i++){
		a[i]+=a[i-1];
	}
	while(~scanf("%lld",&n)&&n){
		printf("%lld\n",a[n]);
	}
    return 0;
}
```
## Farey Sequence
**题意**   
给出式子F F中分子分母互质，且分子小于分母   
例：  
 
F2 = {1/2}   
F3 = {1/3, 1/2, 2/3}   
F4 = {1/4, 1/3, 1/2, 2/3, 3/4}   
F5 = {1/5, 1/4, 1/3, 2/5, 1/2, 3/5, 2/3, 3/4, 4/5}   
求解 fn的元素个数   
**题解**   
本题就是求解欧拉函数值的前n项和，模板题，筛一下就行了  
### CODE
```
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
using namespace std;
typedef long long ll;
const int MAXN = 1e6+100;
const int MOD = 1e9;
int pr[MAXN/10],tail;
ll phi[MAXN];
bool isp[MAXN]; 
void euler()
{
	phi[1]=0;
	for(int i=2;i<MAXN;i++){
		if(!isp[i]){
			pr[++tail]=i;
			phi[i]=i-1;
		}
		for(int j=1;j<=tail&&i*pr[j]<MAXN;j++){
			isp[i*pr[j]]=1;
			if(i%pr[j]==0){
				phi[i*pr[j]]=phi[i]*pr[j];
				break;
			}
			else phi[i*pr[j]]=phi[i]*phi[pr[j]];
		}
	}
}
int main()
{
	ios; ll n;
	euler();
	for(int i=1;i<MAXN;i++){
		phi[i]+=phi[i-1];
	}
	while(cin>>n){
		if(n==0) break;
		cout<<phi[n]<<'\n';
	}
    return 0;
}
```
## Maximum GCD
**题意**  
给定一串数，求两两gcd最大值   
**思路**   
这道题考的其实是读入，两个数之间空格可以有多个！！！  
### CODE
```
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
const int MAXN = 1e5;
const int MOD = 1e9;
int pr[MAXN], isp[MAXN], tail = 0;
void prime() {
    isp[1] = 1;
    for (int i = 2; i <= 17000; i++) {
        if (!isp[i]) pr[++tail] = i;
        for (int j = 1; i * pr[j] <= 17000; j++) {
            isp[i * pr[j]] = 1;
            if (i % pr[j] == 0) break;
        }
    }
}
int a[110];
char c[100000];
int main()
{
    int n,tail=0;
    scanf("%d\n",&n);
    while(n--){
    	gets(c); tail=0;
    	int len=strlen(c);
        for(int i=0; i<len; i++){
            int sum=0;
            while(i<len&&c[i]!=' '){
                sum=sum*10+c[i]-'0';
                i++;
            }
            a[++tail]=sum;
        }
        int maxx=-1e9;
		for(int i=1;i<tail;i++){
			for(int j=i+1;j<=tail;j++){
				maxx=max(maxx,__gcd(a[i],a[j]));
			}
		}
		cout<<maxx<<endl;
	}
    return 0;
}
```
## Primes
**题意**  
给定一个数判断是不是素数   
**思路**   
这个就太水了，坑我的就是题目最多有250组数据，我说咋一直超时。。
### CODE
```
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
const int MAXN = 1e5;
const int MOD = 1e9;
int pr[MAXN], isp[MAXN], tail = 0;
void prime() {
    isp[1] = 1;
    for (int i = 2; i <= 17000; i++) {
        if (!isp[i]) pr[++tail] = i;
        for (int j = 1; i * pr[j] <= 17000; j++) {
            isp[i * pr[j]] = 1;
            if (i % pr[j] == 0) break;
        }
    }
}
int main()
{
	prime(); 
    int n, kase = 0;
    for(int k=1;k<=250;k++){
    	scanf("%d",&n);
        if (n <= 0) break;
        if (n <= 2) {
            printf("%d: %s\n", ++kase, "no");
            continue;
        }
        printf("%d: ",++kase);
        if (isp[n]) printf("%s\n", "no");
        else printf("%s\n", "yes");
    }
    return 0;
}
```
