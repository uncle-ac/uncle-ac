---
title: 深度优先搜索DFS详解 
date: 2020-03-31 09:33:15
tags: [算法,模板]
categories: 算法
author: uncleacc
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/13.webp
---

今天学习了深度优先搜索，觉得这一章讲的特别好，连我这么笨的人都看懂了，相信您也一定能看懂，所以我就想把它分享出来，至于为什么不自己写，我觉得原文更加容易理解（绝对不是因为我懒哈），说到这里必须吐槽一下WPS，把图转化成Word还要充钱，还那么贵，哎只能怪我本人太穷，废话少说，贴图吧

{% fb_img https://img-blog.csdnimg.cn/20200416093359941.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093423583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093433137.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093442750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093450809.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093500934.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093510445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/2020041609352021.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

ok，是不是很详细呢，大力推荐啊哈算法这本书，讲的真的很好，在这里我把第二道题的代码贴出来吧，我觉得这是一道非常经典的DFS模板题，便于您复制（尽管您不需要）

```
#include<iostream>
#include<stdio.h>
#include<algorithm>
using namespace std;
int Min=99999,m,n,tx,ty;
int next[4][2]={{1,0} , {0,1} , {-1,0} , {0,-1}},a[100][100];
int vis[100][100];
void dfs(int x,int y,int step){
	int temp_x=x,temp_y=y;
	if(x==tx&&y==ty){
		if(step<Min) Min=step;
		return ;
	}
	for(int i=0;i<=3;i++){
		x=temp_x+next[i][0];
		y=temp_y+next[i][1];
		if(x<1||y<1||x>m||y>n) continue;
		if(!a[x][y]&&!vis[x][y]){
			vis[x][y]=1;
			dfs(x,y,step+1);
			vis[x][y]=0;
		}
	}
	return;
}
int main()
{
	cin>>m>>n;
	int sx,sy;
	for(int i=1;i<=m;i++){
		for(int j=1;j<=n;j++){
			cin>>a[i][j];
		}
	}
	cin>>sx>>sy>>tx>>ty;
	vis[sx][sy]=1;
	dfs(sx,sy,0);
	cout<<Min<<endl;
}
```