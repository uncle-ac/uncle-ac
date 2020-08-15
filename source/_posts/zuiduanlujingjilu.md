---
title: 最短路径记录问题
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/65.webp'
date: 2020-08-06 21:05:42
authorLink:
categories: 题目
tags: 图论
keywords:
description: 迪杰斯特拉算法记录路径问题
---

## [Dijkstra?](https://vjudge.net/problem/CodeForces-20C)

You are given a weighted undirected graph. The vertices are enumerated from 1 to *n*. Your task is to find the shortest path between the vertex 1 and the vertex *n*.

**Input**

The first line contains two integers *n* and *m* (2 ≤ *n* ≤ 105, 0 ≤ *m* ≤ 105), where *n* is the number of vertices and *m* is the number of edges. Following *m* lines contain one edge each in form *a* *i*, *b* *i* and *w* *i* (1 ≤ *a* *i*, *b* *i* ≤ *n*, 1 ≤ *w* *i* ≤ 106), where *a* *i*, *b* *i* are edge endpoints and *w* *i* is the length of the edge.

It is possible that the graph has loops and multiple edges between pair of vertices.

**Output**

Write the only integer -1 in case of no path. Write the shortest path in opposite case. If there are many solutions, print any of them.

**Examples**

Input

```
5 6
1 2 2
2 5 5
2 3 4
1 4 1
4 3 3
3 5 1
```

Output

```
1 4 3 5 
```

Input

```
5 6
1 2 2
2 5 5
2 3 4
1 4 1
4 3 3
3 5 1
```

Output

```
1 4 3 5 
```

### 分析

和最短路模板题目就差一点，这道题要你输出路径，那么每次更新最短路径时我们就可以记录更新当前点的那个点，因为更新就相当于两点之间添加线段，表明这是一条路径，这样一来，我们就可以知道每一个点到起点的最短路径的长度和路径本身

### CODE

```c
#include <stdio.h>
#include <iostream>
#include <string.h>
#include <vector>
#include <queue>
#include <map> 
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0) 
using namespace std;
typedef long long ll;
const ll INF = 1ll<<62;
const int MAXN=1e5+100;
struct node{
    ll u,cost;
    bool operator <(const node &o)const{
	   return cost>o.cost;
    }
};
int n,m,tail;
vector<node> G[MAXN];
priority_queue<node> pq;
ll dis[MAXN];
int pre[MAXN],ans[MAXN];
bool vis[MAXN];
void init(int n){
    for(int i=0;i<=n;i++) G[i].clear();
    memset(vis,0,sizeof(vis));
    for(int i=0;i<=n;i++) dis[i]=INF;
}
void dij(int start){
	while(!pq.empty()) pq.pop();
	dis[start]=0;
	pq.push({start,0});
	while(!pq.empty()){
		node tmp=pq.top(); pq.pop();
		int u=tmp.u;
		if(vis[u]) continue;
		vis[u]=1;
		for(int i=0;i<G[u].size();i++){
			int v=G[u][i].u, w=G[u][i].cost;
			if(vis[v]) continue;
			if(dis[v]>dis[u]+(ll)w){
				pre[v]=u; //记录这个点的上一个点
				dis[v]=dis[u]+w;
				pq.push({v,dis[v]});
			}
		}
	}
}
int main()
{
	ios;
	cin>>n>>m;
    init(n);
    for(int i=0;i<m;i++){
		int a,b,c;
		cin>>a>>b>>c;
        G[a].push_back({b,c});
        G[b].push_back({a,c});
    }
    int s=1,e=n;
    dij(s);
    if(dis[e]==INF) cout<<-1<<'\n';
    else{
    	int cur=e; //知道了终点从终点开始往前走直到走到起点
    	while(cur!=s){
    		ans[++tail]=cur; //逆序
    		cur=pre[cur];
		}
		ans[++tail]=1;
		for(int i=tail;i>=1;i--){
			if(i!=1) cout<<ans[i]<<' ';
			else cout<<ans[i]<<'\n';
		}
	}
    return 0;
}
```

