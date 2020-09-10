---
title: 积分赛4
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/73.webp'
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

```c++
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
typedef long long ll;
const ll MOD=1e9+7;
struct node{
	ll mat[2][2];
	void set(){
		memset(mat,0,sizeof mat);
		for(int i=0;i<2;i++) mat[i][i]=1;
	}
	node operator * (const node &o){
		node ans;
		for(int i=0;i<2;i++){
			for(int j=0;j<2;j++){
				ll sum=0;
				for(int k=0;k<2;k++){
					sum=(sum+mat[i][k]*o.mat[k][j]%MOD)%MOD;
				}
				ans.mat[i][j]=sum;
			}
		}
		return ans;
	}
};
node p={
	1,1,
	1,0
};
node ksm(node x,ll n){
	node ans;
	ans.set();
	while(n){
		if(n&1) ans=ans*x;
		x=x*x;
		n>>=1;
	}
	return ans;
}
int main()
{
	ios;
	ll n;
	cin>>n;
	node unit=ksm(p,n-2);
	ll a=(unit.mat[0][0]+unit.mat[0][1])%MOD;
	ll b=(unit.mat[1][0]+unit.mat[1][1])%MOD;
	ll c=(a+b)%MOD;
	cout<<(a*c)%MOD<<'\n';
	return 0;
}
```

##  **C.** 胡图图公司大整改

这道题是线段树的升级版，朴素版线段树是对一个连续的线段实现单点修改区间查询，而这道题则是在树上实现单点修改，区间查询，1结点是根节点，所以这道题就添加了一个操作把这n个点建成树的样子，然后对它们进行编号，编号方式是优先遍历子树，遍历到谁编号加一，这里用low数组来实现这个功能，到这里单点修改已经可以实现了，已经把一个线段上的每一个点都映射到了树上，但是区间查询还没有办法实现，为什么实现不了？这里要查询的是一个点的子树，这里的子树就相当于区间，我们上面优先遍历子树因此遍历完后，子树上的每一个点的序号是连续的，所以可以用high数组来记录子树区间的右端点，当遍历完一个点的所有子节点时就记录一下当前的序号，那么[lowi,highi]就表示了子树里面的点

总之这道题知识点就是用了一个dfs实现了将线段上的点映射到树上并且给之编号，实现了查询一个点的子树功能

```c++
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)
#define debug freopen("in.txt","r",stdin),freopen("out.txt","w",stdout );
#define pb push_back
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pii;
const ll maxn=1e5+10;
const ll inf=0x3f3f3f3f ;

ll N,Q;
ll w[maxn]; //每一个部门的薪资
vector<ll> ve[maxn]; //建边
ll high[maxn],low[maxn],cnt; //给树上的点编号
struct node{
    ll l,r;
    ll minn,maxx;
}tr[maxn*4];

void pushup(ll u){
    tr[u].minn=min(tr[u<<1].minn,tr[u<<1|1].minn);
    tr[u].maxx=max(tr[u<<1].maxx,tr[u<<1|1].maxx);
}
void build(ll u,ll l,ll r){ //每一段的编号，范围
    tr[u]={l,r};
    if(l!=r){
        ll mid=(l+r)>>1;
        build(u<<1,l,mid); //建左端
        build(u<<1|1,mid+1,r); //建右端
    }
}
void add(ll u,ll x,ll c){ //编号，要修改的位置，添加的值
    if(tr[u].l==x && x==tr[u].r){ //找到位置
        tr[u].minn+=c; //此时只有一个点，因此最小值和最大值可以直接修改
        tr[u].maxx+=c;
    }else{
        ll mid=(tr[u].l+tr[u].r)>>1; //看看这个位置在左半边还是右半边
        if(x<=mid) add(u<<1,x,c); //左半边
        else add(u<<1|1,x,c); //右半边
        pushup(u); //每次修改节点的值后，都要上报给上级
    }
}
node query(ll u,ll l,ll r){ //编号，范围
    if(l<=tr[u].l && tr[u].r<=r) return tr[u]; //假若该段包含在给定范围内则直接返回
    else{
        ll mid=(tr[u].l+tr[u].r)>>1;
        if(r<=mid) return query(u<<1,l,r); //左端
        else if(l>mid) return query(u<<1|1,l,r); //右端
        else{ //左右两端中间
            node ans,L,R;
            L=query(u<<1,l,r); //左半段
            R=query(u<<1|1,l,r); //右半段
            ans.minn=min(L.minn,R.minn);
            ans.maxx=max(L.maxx,R.maxx);
            return ans;
        }
    }
}
//[low[x],high[x]]表示x结点下的结点编号范围
//high[x]-low[x]表示x结点下的子节点数量
void dfs(ll u,ll fa=-1){ //优先遍历每一个节点的子节点，给每一个结点编号
    low[u]=++cnt; //进入该节点时编个号
    for(auto v:ve[u]){ //遍历每一个节点的子节点
        if(v==fa) continue; //注意不能返回原来的路
        dfs(v,u); //优先遍历子节点
    }
    high[u]=cnt; //遍历完该节点的子节点编个号，注意这里不能加
}
int main(){
    ios;
    cin>>N>>Q; //N个部门，Q组查询
    for(ll i=1;i<=N;i++) cin>>w[i]; //N个部门的薪资
    for(ll i=1;i<=N-1;i++){ //建树
        ll u,v; cin>>u>>v; //两个部门之间有从属关系即两者有边
        ve[u].pb(v);
        ve[v].pb(u);
    }
    dfs(1); //给树上的点进行编号，注意1是根节点
    build(1,1,N); //建树，以上就是线段树的模板
    for(ll i=1;i<=N;i++) add(1,low[i],w[i]); //把原来的每一个点的权值映射到树上
    while(Q--){
        ll op,x,y;
        cin>>op;
        if(op==1){
            cin>>x>>y;
            add(1,low[x],y); //x映射到书上就是low[x]的位置
        }else{
            cin>>x;
            node ans=query(1,low[x],high[x]); //x的子树的范围就是[low[x],high[x]]
            cout<< ans.maxx-ans.minn <<'\n';
        }
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
		fa[y]=x; //有一点要明白，并查集合并操作是对两个根节点的操作，儿子结点和根节点的联系是通过压缩路径去实现的
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

## H. 胡图图的好兄弟

**记忆化搜索**

首先不考虑*a*1为何值，先预处理出当*x >* 1时的结果，首先定义一个数组dp[N] [2]，*dp*[*i*][*j*]是指*x* = *i*并且执行第*j*步操作时的最终结果。在过程中记录每一个*dp*[*i*][*j*]下一次再次来到*i*点便可以直接返回*dp*[*i*][*j*]。在这个过程中可能会遇到回到了*a*1，但是*a*1是不确定的。不过并没有影响，题目描述算法输出有两种情况，一个是*y*，一个是*ᱟ* 1，什么时候出现*⧠* 1，当出现循环的时候，比如说在一次程序运行中，已经计算了*dp*[*i*] [*j*]但是后来有来到了*dp*[*i*][*j*]，很明显此时陷入循环了，那么就会输出*ࠪ* 1，对于再次经过*a*1有两种情况，一种是*i* = 1*, j* = 0，即下一步*x* = *x* + *a*1，因为程序的第一步都是*i* = 1*, j* = 0，所以说，到这里陷入循环输出*ࠪ* 1。另外一种*i* = 1 *j* = 1是不会出现的。预处理完后，我们便可以逐个输dp*[i][1] + *ii* 1(*i >* 1)，*ii* 1是指初始时经过*a*1也就是*dp*[1][0]的值。

### CODE

```c
#include <bits/stdc++.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
typedef long long ll;
const int N=2e5+100;
ll dp[N][2];
int arr[N];
bool vis[N][2];
int n;
void dfs(int i,int j){ //表示当前在第几个点，当前经过了奇数还是偶数次操作
    if(vis[i][j]) return ; //如果当前点之前已经经过了则直接返回
    int nx; //下一个点
    vis[i][j]=1; //标记当前点已经访问过了
    if(j&1) nx=i-arr[i]; //奇数次操作减
    else nx=i+arr[i]; //偶数次操作加
    if(nx<=0||nx>n){ //下一个点结束，则现在就可以直接记录答案，表示从第i个点开始经过奇偶次操作后结束答案为arr[i]
        dp[i][j]=arr[i];
        return;
    }
    dfs(nx,j^1); //下一个点没有结束则需要继续搜索，j变换奇偶
    if(dp[nx][j^1]!=-1){ //搜索完后如果从下一个点开始可以结束
        dp[i][j]=arr[i]+dp[nx][j^1]; //加上下一步的答案
    }
}
int main()
{
    ios;
    vis[1][1]=1; //表示从第一个点开始，经过了1次操作，也就是第一步后到达的第一个点
    memset(dp,-1,sizeof dp); //初始化每一个点的答案都是-1，表示有循环
    cin>>n;
    for(int i=2;i<=n;i++) cin>>arr[i];
    for(int i=2;i<=n;i++) dfs(i,1); //预处理，得出每一个点的答案
    for(int i=2;i<=n;i++){
        if(dp[i][1]!=-1) dp[i][1]+=i-1; //因为是从第二步开始的，第一步i=1,j=0，因此需要加上a1，也就是i-1
    }
    for(int i=2;i<=n;i++){
        cout<<dp[i][1]<<'\n';
    }
    return 0;
}
```

##  **I.** 胡图图吃桃子

完全背包问题，算是比较模板，不过你得想到完全背包%%%

完全背包初始化我还不理解，不懂什么时候必须初始化一些东西，反正我现在的做法就是把能直接看出来答案的数都初始化一下

[详细点击我](https://www.luogu.com.cn/problem/solution/P1679)

```c
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int p[1000],dp[120010];
int main()
{
    int w;
    cin>>w;
    for(int i=1;i<=18;i++) p[i]=pow(i,4); //数据范围到1e5所以要算到18
    memset(dp,0x3f,sizeof dp); //初始化为无穷大
    for(int i=1;i<=18;i++) dp[p[i]]=1; //因为是4次方所以最小值都是1
    for(int i=1;p[i]<=w;i++){ //注意条件
        for(int j=p[i];j<=w;j++){
            dp[j]=min(dp[j],dp[j-p[i]]+1);
        }
    }
    cout<<dp[w]<<'\n';
    return 0;
}
```

这道题我刚开始一直以为贪心做就可以了，但事实证明认为可行也只是我的个人感觉，我刚开始的做法就是每次对这个数开四次根号，再减去这个数的四次方，之前已知都没想到哪里错了，举了很多例子都找不出来错误，事实上例子确实不好找，跑了一下706到1里面错误的都很少，比如704=(4^4^)^2^+(2^4^)^12^,也就是14次，但是贪心每次都去最大四次方数就是二十次，错了