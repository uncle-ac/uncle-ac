---
title: dfs记忆化搜索
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/75.webp'
date: 2020-08-05 11:08:45
authorLink:
categories: 题目
tags: 集训
keywords:
description: dfs记忆化搜索训练，又是一个debug特别难，而且很抽象的算法
---

> dfs标记的位置太重要了，放在不同位置产生的效果都有巨大的差别，记录今天的一些dfs记忆化搜索题目

## A\- Function Run Fun

[题目链接](https://vjudge.net/contest/387584#problem/A)

```
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<iostream>
typedef long long ll;
using namespace std;
ll used[50][50][50];
ll w(int x,int y,int z){
	if(x<=0||y<=0||z<=0) return 1;
	if(x>20||y>20||z>20) return w(20,20,20);
	if(used[x][y][z]) return used[x][y][z];
	if(x<y&&y<z) return used[x][y][z]=w(x,y,z-1)+w(x,y-1,z-1)-w(x,y-1,z);
	return used[x][y][z]=w(x-1,y,z)+w(x-1,y-1,z)+w(x-1,y,z-1)-w(x-1,y-1,z-1);
}
int main()
{
	int a,b,c;
	while(cin>>a>>b>>c){
		if(a==-1&&b==-1&&c==-1) break;
		memset(used,0,sizeof used);
		printf("w(%d, %d, %d) = %lld\n",a,b,c,w(a,b,c));
	}
	return 0;
 } 
```

## B\- 滑雪

[题目链接](https://vjudge.net/contest/387584#problem/B)

```
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<iostream>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
typedef long long ll;
using namespace std;
int G[110][110],step[110][110];
int Next[4][2]={1,0,0,1,-1,0,0,-1};
int r,c;
int dfs(int x,int y){
	if(step[x][y]) return step[x][y];
	step[x][y]=1; //因为记忆化搜索要判断该点是否为零，所以初始化要放到这里
	for(int i=0;i<4;i++){
		int nx=x+Next[i][0];
		int ny=y+Next[i][1];
		if(nx>=1&&nx<=r&&ny>=1&&ny<=c&&G[nx][ny]<G[x][y]){
			step[x][y]=max(step[x][y],dfs(nx,ny)+1); 
		}
	}
	return step[x][y];
}
int main()
{
	ios;
	cin>>r>>c;
	for(int i=1;i<=r;i++){
		for(int j=1;j<=c;j++){
			cin>>G[i][j];
		}
	}
	int maxn=-1;
	for(int i=1;i<=r;i++){
		for(int j=1;j<=c;j++){
			maxn=max(maxn,dfs(i,j));
		}
	}
	cout<<maxn<<'\n';
	return 0;
 } 
```

## C\- 漫步校园

[题目链接](https://vjudge.net/contest/387584#problem/C)

这道题涉及到求每一个点到某一点的最短路径，听着很耳熟吧，没错就是dijistla算法！这不过这个是通过矩阵给你了，但是性质没变，依旧是用一个bfs，只不过找一个点的邻接点方式变成了上下左右！依旧是通过最小点去更新其他点，可以看以下两段代码，优化后时间31ms，没优化46ms，朴素版本的没有采用标记法，虽然答案一样，但是其走了许多不必要走的点，这些点的最短路径已经确定了，根本不可能更新但还是去比较了，浪费了时间

记忆化搜索在递归的过程中已经算出来了许多点的值，以后的递归过程中当用这个点的值时可以直接返回

### 优化 CODE

```c
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<queue>
#include<iostream>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
typedef long long ll;
using namespace std;
const int INF=0x3f3f3f3f;
struct node{
	int x,y,step;
	bool operator < (const node &o) const {
		return step>o.step;
	}
};
int G[110][110],dis[110][110],vis[110][110];
ll used[110][110];
int Next[4][2]={1,0,0,1,-1,0,0,-1};
int n;
priority_queue<node> pq;
void bfs(){
	while(!pq.empty()) pq.pop();
	memset(vis,0,sizeof vis);
	memset(dis,0x3f,sizeof dis);
	node s={n,n,G[n][n]};
	pq.push(s);
	dis[n][n]=G[n][n];
	while(!pq.empty()){
		node tmp=pq.top(); pq.pop();
		if(vis[tmp.x][tmp.y]) continue; //已经确定最短路径的点
		vis[tmp.x][tmp.y]=1;
		for(int i=0;i<4;i++){
			int nx=tmp.x+Next[i][0];
			int ny=tmp.y+Next[i][1];
			if(nx<1||nx>n||ny<1||ny>n) continue;
			if(dis[tmp.x][tmp.y]+G[nx][ny]<dis[nx][ny]){
				dis[nx][ny]=dis[tmp.x][tmp.y]+G[nx][ny];
				pq.push({nx,ny,dis[nx][ny]});
			}
		}
	}
}
ll dfs(int x,int y){ //注意ll
	if(used[x][y]) return used[x][y]; 
	used[x][y]=0;
	for(int i=0;i<4;i++){
		int nx=x+Next[i][0];
		int ny=y+Next[i][1];
		if(nx<1||nx>n||ny<1||ny>n) continue;
		if(dis[x][y]>dis[nx][ny]){
			used[x][y]+=dfs(nx,ny); //一个点的所有走法等于它周围所有点的走法之和
		}
	}
	return used[x][y];
}
int main()
{
	ios;
	while(cin>>n){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				cin>>G[i][j];
			}
		}
		bfs();
//		for(int i=1;i<=n;i++){
//			for(int j=1;j<=n;j++){
//				if(j!=n) cout<<dis[i][j]<<' ';
//				else cout<<dis[i][j]<<'\n';
//			}
//		}
		memset(used,0,sizeof used);
		used[n][n]=1;
		cout<<dfs(1,1)<<'\n';
	}
	return 0;
 } 
```

### 朴素 CODE

```c
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<queue>
#include<iostream>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
typedef long long ll;
using namespace std;
const int INF=0x3f3f3f3f;
struct node{
	int x,y;
};
int G[110][110],dis[110][110];
ll used[110][110];
int Next[4][2]={1,0,0,1,-1,0,0,-1};
int n;
queue<node> q;
void bfs(){
	while(!q.empty()) q.pop();
	memset(dis,0x3f,sizeof dis);
	node s={n,n};
	q.push(s);
	dis[n][n]=G[n][n];
	while(!q.empty()){
		node tmp=q.front(); q.pop();
		for(int i=0;i<4;i++){
			int nx=tmp.x+Next[i][0];
			int ny=tmp.y+Next[i][1];
			if(nx<1||nx>n||ny<1||ny>n) continue;
			if(dis[tmp.x][tmp.y]+G[nx][ny]<dis[nx][ny]){
				dis[nx][ny]=dis[tmp.x][tmp.y]+G[nx][ny];
				q.push({nx,ny});
			}
		}
	}
}
ll dfs(int x,int y){
	if(used[x][y]) return used[x][y];
	ll sum=0;
	for(int i=0;i<4;i++){
		int nx=x+Next[i][0];
		int ny=y+Next[i][1];
		if(nx<1||nx>n||ny<1||ny>n) continue;
		if(dis[x][y]>dis[nx][ny]){
			sum+=dfs(nx,ny);
		}
	}
	used[x][y]=sum;
	return sum;
}
int main()
{
	ios;
	while(cin>>n){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				cin>>G[i][j];
			}
		}
		bfs();
		memset(used,0,sizeof used);
		used[n][n]=1;
		cout<<dfs(1,1)<<'\n';
	}
	return 0;
 } 
```



## E  Zipper

[题目链接](https://vjudge.net/contest/387584#problem/E)

感觉是一道难题，可能是对我来说吧，这道题不能简单的遍历，当前两个字符串出现相同字符时，没有办法确定该字符在第三个字符串中的位置，例如样例2： `cat tree catrtee` 人眼第一眼看都以为输出no，但是仔细想一想，如果cat的t是最后一个t那么这就是可行的，这种判断是哪个t的操作普通遍历是不可行的，这种题目就有dp的思想，首先我们判断前两个字符串的首字母和最后一个字符串的首字母相同否，因为第三个字符串如果可以由前俩字符串组成，那么里面的所有字符要不是第一个字符串里的要不是第二个里面的，假若最后一个字符串的首字母既不是A里面的也不是B里面的，那一定是不可行的，若是A或B里面的，就分为3种情况

1. 假若是A里面的不是B里面的，那么A的第一个字符和C的第一个字符就可以同时去掉了！问题就变成了判断A(除了第一个字符)和B是否能组成C(除了第一个字符)，这就把问题分成了子问题，
2. 假若是B里面的不是A里面的，性质同第一种情况
3. 同时是A又是B里面的，这样就要分两种情况去走了，例如样例2`cat tree catrtee`，首先我们把t当作是第一个的t，然后再当作第二个的，分别看看能否有解，只要有一个有解就说明是可行的

这种子问题和原问题性质一样的题目就可以用递归去解，写一个dfs函数，三个参数分别是ABC的当前下标，什么时候结束呢？当前两个字符串的下标移到了最后就说明该解可行，标记flag返回就行了。

但是还没完，到这里还没用记忆化搜索，这样会超时，为什么用到了记忆化搜索？例如：`ttt ttt tttttt`，这是一组数据，因为里面每一个字符都相同，那么就都要分两种情况去搜索，有很多种情况都是重复搜索的，例如：`tt tt tttt`，既可以由`ttt tt ttttt`，转化过来也可以由`tt ttt ttttt`转化过来，那么就被计算了两次，那么就可以存下运算结果，标记这种情况已经算过了，以后再到这种情况直接返回

```c
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<queue>
#include<iostream>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
typedef long long ll;
using namespace std;
const int INF=0x3f3f3f3f;
string s1,s2,s3;
int n,len1,len2,len3,flag;
bool vis[500][500];
void dfs(int pos1,int pos2,int pos3){
	if(pos1==len1&&pos2==len2){ //找到可行解
		flag=1; //标记
		return ;
	}
	if(s1[pos1]!=s3[pos3]&&s2[pos2]!=s3[pos3]) return ; //C字符串字符一定是A或者B里的一个，不是则不可行
	if(vis[pos1][pos2]) return ; //记忆化搜索标记
	vis[pos1][pos2]=1;
	if(s1[pos1]==s3[pos3]) dfs(pos1+1,pos2,pos3+1); //分两种情况
	if(s2[pos2]==s3[pos3]) dfs(pos1,pos2+1,pos3+1);
}
int main()
{
	ios;
	int t,kase=0;
	cin>>t;
	while(t--){
		memset(vis,0,sizeof vis);
		cin>>s1>>s2>>s3;
		len1=s1.size();
		len2=s2.size();
		len3=s3.size();
		flag=0;
		dfs(0,0,0); 
		if(flag) cout<<"Data set "<<++kase<<": yes"<<'\n';
		else cout<<"Data set "<<++kase<<": no"<<'\n';
	}
	return 0;
 } 
```

## G\- FatMouse and Cheese

[题目链接](https://vjudge.net/contest/387584#problem/G)

和B滑雪很相似，代码一些细节很重要，标注部分，采用此方式速度甚至会快一些

```c
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<queue>
#include<iostream>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
typedef long long ll;
using namespace std;
const int INF=0x3f3f3f3f;
struct node{
	int x,y;
};
int G[110][110];
ll used[110][110];
int Next[4][2]={1,0,0,1,-1,0,0,-1};
int n,m;
ll dfs(int x,int y){
	if(used[x][y]) return used[x][y];
	used[x][y]=G[x][y]; //每一个点的初始分数都是它本身，这个不能放到主函数进行，因为会影响上一句if的判断，实际上这是一句细思极秒的操作，这个操作避免了许多临界问题
	for(int i=0;i<4;i++){
		for(int k=1;k<=m;k++){
			int nx=x+k*Next[i][0];
			int ny=y+k*Next[i][1];
			if(nx<1||nx>n||ny<1||ny>n) continue;
			if(G[nx][ny]>G[x][y]){
				used[x][y]=max(used[x][y],dfs(nx,ny)+G[x][y]); //这个点往下走后这个点就不会再用了，换句话说就是由这个点拓展出去的点不会反过来拓展该点，因此可以直接在循环里面更新其最值
			}
		}
	}
	return used[x][y];
}
int main()
{
	ios;
	while(cin>>n>>m){
		if(n==-1&&m==-1) break;
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				cin>>G[i][j];
			}
		}
		memset(used,0,sizeof used);
		cout<<dfs(1,1)<<'\n';
	}
	return 0;
 } 
```

