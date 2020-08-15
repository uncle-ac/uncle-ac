---
title: 积分赛3
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/70.webp'
date: 2020-08-02 20:42:26
authorLink:
categories: 题目
tags: 集训
keywords:
description: 自闭的积分赛
---

> 学习了最短路后，感觉自己理解真的很一般，只会一些模板题目，稍微变一下就死了，而且发现我的思维很死，只会写套路题，做那些稍微灵活一点的题目感觉就很吃力，希望以后能有好转

## B 张仙女的愧疚

>张仙女最近痴迷于游戏，可是他每周都得为学弟们准备一场积分赛。但，不是谁都能成为
>
>时间管理大师。在游戏的诱惑下，他不小心出了几道难题，导致学弟们的心态可能爆炸，终于
>
>他感到了深深的愧疚。决定这次一定友好一点。
>
>他想到了一个有趣且简单的问题，准备交给学弟们解决。
>
>给你一个数组*N*，数组的下标从1开始，数组中的每个数的值为*A**i*，你需要在这个数组中
>
>找到两个数，使他们的数值和与他们的下标的差的绝对值相等，两个数为一组，张仙女想考考
>
>你，在这个数组中你最多能找到可以满足题意的几组数。
>
>**输入**
>
>单组输入
>
>第一行为数组的大小*N*(2 ⩽ *N* ⩽ 200000)
>
>接下来一行*N*个以空格区分开的数表示数组中的值*A**i*1 *≤* *A**i* *≤* 109
>
>(1 *≤* *i* *≤* *N*)
>
>**输出数据**
>
>一个数字，表示答案
>
>**样例输入**
>
>6
>
>2 3 3 1 3 1
>
>**样例输出**
>
>3

### 分析

题意很简单求数组里面有多少对数满足数值和等于下标差，列出等式a[i]+a[j]==j-i，这道题就是一个简单的思维题，说简单，我还是没想出来，真的菜死了，这个式子可以变一下形把含i的弄到一边，这样a[i]+i==j-a[j]，这样一来我们只要遍历一遍看看数值等于a[i]+i的有多少个数，用map存下来，再遍历一遍，看看map下标等于j-a[j]的有多少个就行了

```c
#include<bits/stdc++.h>
using namespace std;
const int N= 200010;
int arr[N];
map<int,int> mp;
int main()
{
	int n;
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>arr[i];
		mp[i+arr[i]]++;
	}
	long long cnt=0;
	for(int i=1;i<=n;i++){
		if(mp[i-arr[i]]) cnt+=mp[i-arr[i]];
	}
	cout<<cnt<<'\n';
	return 0;
 } 
```

## C 何仙女的回文串

>在一个遥远的国度里，有一个女儿国，里面住着很多很多的小仙女。某一天，一位英俊的
>
>剑士前来寻求对象。国王告诉他，只要能回答出来一个问题，他就能娶走其中的何仙女；
>
>现在有一个字符串S，S为所有仙女名字的首字母集合，国王命令他找出最长的字符串**T**， 
>
>T满足以下条件: 
>
>• T的长度不超过S的长度
>
>• T是回文串
>
>• 存在两个字符串A,B（可能为空），使得 T=A+B ('+'表示连接两个字符串)，其中A是S的
>
>前缀字符串，B是S的后缀字符串
>
>剑士听后心头一颤，不知所措，暗地里告诉了你字符串S，希望没有对象的你能尽快告诉
>
>他满足条件的字符串T是多少；
>
>**输入数据**
>
>第一行是一个整数*T*(1 ⩽ *t* ⩽ 1000)，表示样例的个数。
>
>以后每个样例一行，是一个字符串S。 
>
>(数据确保所有字符串长度不超过5000)
>
>**输出数据**
>
>对于每个测试用例，输出满足上述条件的最长字符串T。如果存在多个可能的解决方案，输
>
>出其中任何一个。
>
>**样例输入**
>
>5
>
>a
>
>abcdfdcecba
>
>abbaxyzyx
>
>codeedoc
>
>acbba
>
>**样例输出**
>
>a
>
>abcdfdcba
>
>xyzyx
>
>codeedoc
>
>abba

### 分析

其实这道题也不是很难的，我没做这道题很大的原因其实就是这道题没多少人AC，心理就有些抵触。。我把大量精力都放到了F，其实F是一道很难的题目，但是AC的人多我就去做了。。这道题既然要找最长回文串，还要求前后是源字符串的前缀和后缀，我们就可以从前后找，来一个双指针，找到第一个不同的字符然后找中间最长的回文串，把三段拼接在一起就行了

```c
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int mod=998244353;
string s,ans;
bool check(int l,int r){  
	while(l<r){
		if(s[l]!=s[r]) return 0;
		l++,r--;
	}
	return 1;
}
int main()
{
	int t; cin>>t;
	while(t--){
		ans.clear();
		cin>>s;
		int l=0,r=s.size()-1;
		while(l<r&&s[l]==s[r]) l++,r--;
		int i,j;
		for(i=l;i<=r;i++) if(check(i,r)) break;
		for(j=r;j>=l;j--) if(check(l,j)) break;
		int ll,rr;
		if(r-i>j-l) rr=r,ll=i;
		else rr=j,ll=l;
		for(int i=0;i<l;i++) ans+=s[i];
		for(int i=ll;i<=rr;i++) ans+=s[i];
		for(int i=r+1;i<s.size();i++) ans+=s[i];
		cout<<ans<<'\n';
	}
}
```

## E 拯救李仙女

>大魔王因为抓来的公主被勇士救走后很是不甘心，于是他将魔爪伸向了李仙女，这次为了
>
>不再让强大的勇士阻止自己，他选择化身美男子让小仙女爱上自己，年轻的小仙女果不其然上
>
>当了，她将不顾危险的来到大魔王的城堡中。
>
>象征着爱与和平的尹法师想要阻止大魔王的阴谋，显然直接打败大魔王是不能拯救小仙女
>
>的，于是大法师派遣*n*个人在沿途中劝阻小仙女，每个人都守在一个路口上，初始时小仙女对
>
>大魔王的爱慕值为*w*，当她遇到第*i*个人，第*i*个人的影响力是*a**i*，将会使小仙女的爱慕值减少*a**i*，
>
>小仙女很聪明，知道大法师不会让自己轻易见到心爱的人，并且获得了大法师派遣的人的位置
>
>以及他们的影响力，她将选择受到最少阻拦的那条路，（即减少的爱慕值最少，如果有多条，那
>
>么这几条路都有被选择的可能）。
>
>如果小仙女在到达大魔王的城堡门口时，依旧爱着大魔王，大法师将出现在城堡门口。当
>
>然大法师是没有丝毫作用的，不过如果此时的小仙女对大魔王的爱慕值是12的倍数，那么大法
>
>师将获得神奇的力量，接下来，他将使用魔法进行召唤术（可进行无数次召唤），每次都会将
>
>一个小仙女沿途不可能见过的人召唤到这里阻拦她（大法师是不知道小仙女具体走的哪条路，
>
>且召换来的人依旧有与原来一样的影响力)，如果最后可以拦住请输出大法师最少的召唤次数。
>
>当然如果爱慕力不是12的倍数大法师也不会获得神奇的力量，小仙女就会陷入大魔王的魔爪
>
>中。
>
>不可能见过即指小仙女一定不会遇到的人。
>
>**输入数据**
>
>第一行有两个整数*n m w*(1 ⩽ *n* ⩽ 100000*,* 1 ⩽ *m* ⩽ 200000*,* 1 ⩽ *w* ⩽ 1*e*9)，分别代表路口
>
>数和道路数以及小仙女的初始爱慕值。
>
>第二行有*n*个整数*a*1 *· · ·* *a**n*(1 ⩽ *a**i* ⩽ *w*)，*a**i*是第*i*个人的影响力。接下来的*m*行每行有两个
>
>整数*i, j*(1 ⩽ *i, j* ⩽ *n*)，代表着第*i*号路口与第*j*号路口有一条路。（所给图不保证连通，当然不
>
>连通大法师也就不必那么麻烦了）
>
>初始时小仙女在1号路口处，大魔王的城堡在n号路口处，只要经过这个路口路口上的人就
>
>会影响到小仙女，小仙女的爱慕力小于等于0的时候，小仙女就会放弃大魔王。
>
>**输出数据**
>
>如果最终可以阻止小仙女见到大魔王输出大法师使用的最少召唤次数，如果不可以输
>
>出*ࠪ*1。
>
>**样例输入1**
>
>4 4 10
>
>1 3 5 7
>
>1 2
>
>1 3
>
>2 4
>
>3 4
>
>**样例输出1**
>
>0
>
>**样例输入2**
>
>5 5 34
>
>1 9 7 16 5
>
>1 2
>
>2 3
>
>3 5
>
>1 4
>
>4 5
>
>**样例输出2**
>
>-1

### 分析

题意是公主爱上了大魔王，魔法师要劝她，安排了n个人在n个路口劝阻公主，公主有一个爱慕值，她会挑一个最短路去找大魔王，最短路指的是减少爱慕值最少的一条路，如果公主走到了大魔王前，大法师就会出现，如果此时爱慕值是十二的倍数，那么大法师就会召集所有非最短路径的人一起来劝阻公主，最短路径可能有多条

所以这道题就是首先从起点跑一遍迪杰斯特拉算法，找到最短路，判断是否爱慕值是十二的倍数，是的话就再标记所有最短路，难的就是标记最短路，具体看代码注释吧

### CODE

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll N=100100;
const ll M=200100;
ll n,m,w;
ll arr[M];
ll val[N];
bool vis2[N];
struct node{ //链式前向星
	ll pos; //这条边到达的结点
	ll cost; //这条边的权值
	ll next; //这条边的相邻边
	bool operator < (const node &o) const{ //重载运算符
		return cost>o.cost;
	}
}edge[M]; //链式前向星的边数组
ll head[M]; //链式前向星的head数组，表示的是以u结点开始的最后一条边的编号
ll size; //边的编号
void add(ll u,ll v,ll w){
	edge[size].cost=w; //这条边的权值
	edge[size].pos=v; //这条边到达的结点
	edge[size].next=head[u]; //这条边的邻边就是上次以这个节点开始的最后一条边的编号
	head[u]=size++;//更新以这条边开始的最后一条边的编号，同时编号++
}
priority_queue<node> pq; //跑堆优化的迪杰斯特拉用的优先队列
ll dis[N]; //存的是每一个点到起点的最短距离
bool vis[N]; //记录是否确定了某一个点到起点的最短距离
void init(ll n){ //初始化函数
	size=0; //边的序号更新为0
	memset(head,-1,sizeof head); //head初始化为-1，也可以初始化为0，那样的话size就要从1开始
	while(!pq.empty()) pq.pop(); //队列清空
	memset(vis,0,sizeof vis); //vis重置
	memset(dis,0x3f,sizeof dis); //memset以字节赋值longlong也可以使用这种方法
}
void dij(ll s){ //mlog(n)的复杂度
	dis[s]=arr[s]; //起点的距离应该初始化为1结点的劝阻值
	pq.push({s,arr[s]}); //放进去起点
	while(!pq.empty()){
		node x=pq.top(); pq.pop();
		ll u=x.pos; //当前所有点离起点最近的
		if(vis[u]) continue;
		vis[u]=1; //这个点的最短路径就确定了，标记
		for(ll e=head[u];e!=-1;e=edge[e].next){ //找到以这个结点开始的最后一条边的编号，从这条边开始遍历邻边
			ll v=edge[e].pos; //找到u结点所有邻接点
			if(vis[v]) continue; //如果这个点的最短路径确定了continue
			if(dis[v]>dis[u]+edge[e].cost){ //否则更新其到起点的最短距离
				dis[v]=dis[u]+edge[e].cost;
				pq.push({v,dis[v]}); //放进队列，因为每次都是用所有结点离起点最近的点去更新每一个点的最短距离的
			}
		}
	}
}
void dfs(ll pos,ll d){
	if(pos==1) return ; //到了起点退出
	if(vis2[pos]) return ; //如果往回搜了退出
	vis2[pos]=1; //标记父亲结点，防止往后走
	for(ll e=head[pos];e!=-1;e=edge[e].next){ //遍历当前点的邻接点
		ll v=edge[e].pos; //邻接点
		if(d==dis[v]){ //如果(当前点)减去(邻接点到当前点的那条边的权值)后等于(邻接点到起点的最短距离)，则说明当前点是由这个邻接点过来的，则这个邻接点一定是从起点到终点的最短路径中的一条
			vis[v]=1; //标记该点
			dfs(v,dis[v]-arr[v]); //从该点开始找
		}
	}
	vis2[pos]=0; //其实这句在这里没有用，是恢复现场的，一条路撞到南墙了回来后每一个点都恢复原来的状态，加上这一句后其实不用再手动初始化vis2了
}
int main()
{
	scanf("%lld%lld%lld",&n,&m,&w); //n个结点，m条边，爱慕值
	init(n); //初始化
	for(ll i=1;i<=n;i++){ //输入每一个结点的人劝阻值
		scanf("%lld",&arr[i]);
	}
	while(m--){
		ll u,v;
		scanf("%lld%d",&u,&v);
		add(u,v,arr[v]); //这里用一条边的终点的劝阻值作为这条边的权值
		add(v,u,arr[u]); //无向边，但是权值不一样，但是不会有其他影响
	}
	dij(1); //1起点，n终点
	ll cost=dis[n]; //记录到n点的最少花费
	
	if(cost>=w) printf("0\n"); //如果公主到不了n点直接不用召唤了，输出0
	else{
		if((w-cost)%12!=0) printf("-1\n");// 如果这时的爱慕值不是十二的倍数，则公主与大魔王相遇
		else{
			memset(vis2,0,sizeof vis2); //dfs时不走回头路，这里其实不用初始化，因为dfs回朔已经全部恢复成0了，这里是一组数据，多组数据时就不用memset了，小优化
			memset(vis,0,sizeof vis); //vis标记最短路
			memset(val,0,sizeof val); //存非最短路的劝阻值
			int tail=0; //非最短路的结点数量
			vis[n]=1; //必须手动标记起点(dfs的起点)，因为直接就从起点的邻接点开始了
			dfs(n,cost-arr[n]);//这里应该倒着去找，从n开始往前遍历
			for(int i=1;i<=n;i++){ //遍历每一个结点，找到不是最短路径的点
				if(!vis[i]) val[++tail]=arr[i];
			}
			sort(val+1,val+1+tail,greater<ll>()); //从大到小排序，因为要输出最少召唤次数
			ll cnt=0;
			for(int i=1;i<=tail;i++){ //每次召唤最大的，使得召唤次数最少
				if(w<=cost) break;
				w-=val[i];
				cnt++;
			}
			if(cost>=w) printf("%lld\n",cnt);
			else printf("-1\n");
		}
	}
	return 0;
 } 
```

## F 黄仙女的生日

>今天是黄仙女的生日，她妈妈给她买了一块很大很大的蛋糕，在吃蛋糕的时候需要把蛋糕
>
>切开，黄仙女会法术，她每用一次法术就可以在蛋糕内任意选一点，发出两道激光（可以看作
>
>同一点发出两道射线）把蛋糕切开。这两条激光不能重合，不在一条直线上。她想知道如果
>
>用n次法术的话，最多可以把蛋糕分成多少块。（规定只能在蛋糕内选一点）	
>
>**输入数据**
>
>第一行是一个整数*T*(*T* ⩽ 1000)，表示测试实例的个数,然后是T行，每行有一个数字*n*(0 ⩽ 
>
>*n* ⩽ 104)表示用n次法术
>
>**输出数据**
>
>T行数字，最多可以分多少块
>
>**样例输入**
>
>3
>
>0
>
>1
>
>2
>
>**样例输出**
>
>1
>
>2
>
>7 

### 分析

这道题明明这么难，竟然做出来的人这么多！！！可能是我真的太菜了，我现在这道题都没理解为什么这么画块最多，这道题不讲了

### CODE

```c
#include<bits/stdc++.h>
using namespace std;
const int N=10001;
int dp[10001]={1,2};
int main()
{
	for(int i=2;i<10001;i++){
		dp[i]=dp[i-1]+i*4-3;
	}
	int t;
	cin>>t;
	while(t--){
		int n;
		cin>>n;
		cout<<dp[n]<<'\n';
	}
	return 0;
 } 
```

## G 小仙女的树

>小仙女在天上飞的时候，突然听到地面上有两个人争吵，Ayush和Ashish他们邀请小仙女
>
>给他们做裁判，他们有一个无根树，每个人每一次都可以取一个叶子节点，树上有一个特殊节
>
>点，问最后谁能取走特殊的节点。
>
>**输入数据**
>
>第一行输入表示有*T*(*T* ⩽ 10)组数据，第二行输入*n, x*(1 ⩽ *n, k* ⩽ 103),*n*表示有*n*个节点，
>
>*x*为特殊的节点,*x*小于等于*n*。下面的*nn* 1行每一行表表示两个节点无向互相连通，第一次
>
>是Ayush开始走。
>
>**输出数据**
>
>输出两人谁能取到特殊节点
>
>**样例输入**
>
>1
>
>3 1
>
>2 1
>
>3 1
>
>**样例输出**
>
>Ashish

### 分析

少有的签到题目，可我还是很粗心，画图我想当然得看到x结点裸漏在外面就认为能取走了，其实它的度数是大于1的！！！

### CODE

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll MOD= 998244353;
ll degree[1100];
int main()
{
	int t;
	cin>>t;
	while(t--){
		int n,x;
		cin>>n>>x;
		n--;
		while(n--){
			int a,b;
			cin>>a>>b;
			degree[a]++;
			degree[b]++;
		}
		if(degree[x]==1||degree[x]==0){
			cout<<"Ayush"<<'\n';
			continue;
		}
		else{
			if(n==3) cout<<"Ashish"<<'\n';
			else if((n-3)&1) cout<<"Ayush"<<'\n';
			else cout<<"Ashish"<<'\n';
		}
	}
	return 0;
 } 
```

## H 仙女们的高数又双叒叕挂了

> 话说有棵树，挂了很多人，其中也不乏众多仙女。其中有道题目是这样的
>
> 从-1到1对(1-x^2^)^n^积分
>
> 给定n，求得上式的值
>
> **输入数据**
>
> 输入包含多组测试，以文件结尾结束。每组测试包含一个整数n 
>
> 1 *<*= *n <*= 1*e*6
>
> 样例总数不超过1*e*5
>
> **输出数据**
>
> 结果若是整数，则直接输出
>
> 否则输出分数的最简形式，结果mod 998244353
>
> **样例输入**
>
> 1
>
> 2
>
> 3
>
> 455
>
> **样例输出**
>
> 4/3
>
> 16/15
>
> 32/35
>
> 622707799/502563857

### 分析

这道题我真的不信有人第一次就能手推出数学公式！！！不去网上查我是真的不信的！！！当然大神除外，不讲了，纯数学推导

### CODE

```c
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll MOD= 998244353;
const ll maxn=1e6+7;
ll fac[maxn*2+100];
ll ksm(ll a,ll b){
	ll ans=1;
	while(b){
		if(b&1) ans=ans*a%MOD;
		a=a*a%MOD;
		b>>=1;
	}
	return ans;
}
ll solve(ll n){
	ll ans=1;
	for(int i=1;i<=n;i++){
		ans=ans*i%MOD;
	}
	return ans;
}
void init(){
	fac[0]=1;
	for(ll i=1;i<=maxn*2;i++) fac[i]=fac[i-1]*i%MOD;
	return ;
}
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	init();
	int n;
	while(cin>>n){
		ll x=ksm(fac[n],2)*ksm(2,2*n+1)%MOD;
		ll y=fac[2*n+1]%MOD;
		ll tmp=__gcd(x,y);
		cout<<x/tmp<<'/'<<y/tmp<<'\n';
	}
	return 0;
 } 
```

## I  小仙女的善心

求两个矩形的覆盖面积

### CODE

```c
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll MOD= 998244353;
int main()
{
	ll a,b,c,d,e,f,g,h;
	cin>>a>>b>>c>>d>>e>>f>>g>>h;
	ll s1=(c-a)*(d-b);
	ll s2=(g-e)*(h-f);
	if(g<=a||e>=c||h<=b||f>=d){
		cout<<s1+s2<<'\n';
		return 0;
	}
	ll x1,y1,x2,y2;
	x1=max(a,e);
	y1=max(b,f);
	x2=min(c,g);
	y2=min(d,h);
	ll mid=(x2-x1)*(y2-y1);
	cout<<s2+s1-mid<<'\n';
	return 0;
 } 
```

## J 林仙女的选美大赛

>在马卡拉卡山脉的最深处，住着一群小仙女。最近，林仙女举办了一场选美大赛。有n名
>
>小仙女参加了比赛。比赛现场有m名裁判仙女，每个裁判仙女会表达一个自己的观点。比如裁
>
>判A说：“1=2”表示裁判A认为1号仙女和2号仙女一样美。选美比赛结束了，但是裁判仙女们
>
>却犯了愁。因为他们不知道如何整理这些观点，无法给小仙女们排名。屏幕前的你能帮助林仙
>
>女解决这个难题吗？如果根据所给信息可以给小仙女们排名请输出“OK”（可以存在并列排名）
>
>,如果所给信息有冲突（裁判A认为1>2, 裁判B认为1<2）,请输出“CONFLICT”，如果所给信息
>
>不全，无法确定排名请输出“UNCERTAIN”，如果信息中同时包含冲突且信息不完全，就输出
>
>“CONFLICT”。
>
>**输入数据**
>
>本题目包含多组测试，请处理到文件结束。每组测试第一行包含两个整数N,M(0<=N<=10000,0<=M<=20000),
>
>分别表示要参加选美比赛的小仙女数以及裁判仙女数。接下来有M行，分别是每个裁判的观
>
>点(1<=每个小仙女的编号<=n)
>
>**输出数据**
>
>对于每组测试，在一行里按题目要求输出
>
>**样例输入**
>
>3 2
>
>1 > 2
>
>2 = 3
>
>3 3
>
>1 > 2
>
>2 = 3
>
>3 > 1
>
>3 1
>
>1 > 2
>
>**样例输出**
>
>OK
>
>CONFLICT
>
>UNCERTAIN

### 分析

这是一道原题，虽然那道题当时删了，但还好我做了，这道题是用并查集去缩点，把所有=关系的点缩成一个，让它们的祖先代表他们，然后拓扑排序，看看是否有环

### CODE

```c
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=10100;
const int M=20100;
const ll MOD= 998244353;
struct node{
	int to;
	ll cost;
	int next;
}edge[M];
struct b{
	int u,v;
	char c;
}arr[M];
bool vis[N];
ll dis[N];
int head[M],fa[N],in[N];
int size=0,n,m,flag1,flag2,cnt;
queue<int> q;
void add(int u,int v,int w){
	edge[size].to=v;
	edge[size].cost=w;
	edge[size].next=head[u];
	head[u]=size++;
}
void init(int n){
	size=0; flag1=0; flag2=0; cnt=n;
	while(!q.empty()) q.pop();
	for(int i=0;i<=n;i++) head[i]=-1;
	for(int i=0;i<=n;i++) fa[i]=i;
	memset(in,0,sizeof in);
}
int find(int x){
	if(x==fa[x]) return x;
	return fa[x]=find(fa[x]);
}
void merge(int a,int b){
	int x=find(a);
	int y=find(b);
	if(x!=y) fa[y]=x;
}
void tuo(){
	for(int i=1;i<=n;i++){
		if(fa[i]==i&&!in[i]){ //祖先是自己并且入度为零
			q.push(i);
		}
	}
	while(!q.empty()){
		int u=q.front(); q.pop();
		if(q.size()) flag2=1; //如果当前入度为0的点不止一个，则说明有两个点无法比较，Uncertain，这里还有一个很骚的操作，可以直接反向建图，直接在开始时候判断是否入度为0的点大于1个
		cnt--; //计数看看能拓扑排序的点有多少个
		for(int e=head[u];e!=-1;e=edge[e].next){
			int v=edge[e].to;
			in[v]--;
			if(!in[v]) q.push(v);
		}
	}
}
int main()
{
	while(cin>>n>>m){
		init(n);
		for(int i=1;i<=m;i++){
			cin>>arr[i].u>>arr[i].c>>arr[i].v;
			if(arr[i].c=='='){ //把等于关系的两个点并到一个集合，即缩点
				merge(arr[i].u,arr[i].v);
				cnt--; //cnt记录缩点后有几个点
			}
		}
		for(int i=1;i<=m;i++){
			char ch=arr[i].c;
			int u=find(arr[i].u), v=find(arr[i].v);//没缩点的点祖先就是自己
			if(ch=='=') continue;
			if(u==v) flag1=1; //矛盾
			if(ch=='>'){
				add(u,v,0);
				in[v]++; 
			} 
			if(ch=='<'){
				add(v,u,0);
				in[u]++;
			}
		}
		if(flag1){
			cout<<"CONFLICT"<<'\n';
			continue;
		}
		tuo(); //拓扑排序
		if(cnt>0||flag1) cout<<"CONFLICT"<<'\n';
		else if(flag2) cout<<"UNCERTAIN"<<'\n';
		else cout<<"OK"<<'\n';
	}
	return 0;
 } 
```

