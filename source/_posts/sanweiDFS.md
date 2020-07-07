---
title: 简单三维DFS搜索
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-03 16:24:11
tags: DFS
keywords:
description: 简单三维DFS搜索
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/37.webp
---
# 题目
[problem link](http://acm.hdu.edu.cn/showproblem.php?pid=2102)   
可怜的公主在一次次被魔王掳走一次次被骑士们救回来之后，而今，不幸的她再一次面临生命的考验。魔王已经发出消息说将在T时刻吃掉公主，因为他听信谣言说吃公主的肉也能长生不老。年迈的国王正是心急如焚，告招天下勇士来拯救公主。不过公主早已习以为常，她深信智勇的骑士LJ肯定能将她救出。   
现据密探所报，公主被关在一个两层的迷宫里，迷宫的入口是S（0，0，0），公主的位置用P表示，时空传输机用#表示，墙用*表示，平地用.表示。骑士们一进入时空传输机就会被转到另一层的相对位置，但如果被转到的位置是墙的话，那骑士们就会被撞死。骑士们在一层中只能前后左右移动，每移动一格花1时刻。层间的移动只能通过时空传输机，且不需要任何时间。   
`Input`   
输入的第一行C表示共有C个测试数据，每个测试数据的前一行有三个整数N，M，T。 N，M迷宫的大小NM（1 <= N,M <=10)。T如上所意。接下去的前NM表示迷宫的第一层的布置情况，后NM表示迷宫第二层的布置情况。   
`Output`   
如果骑士们能够在T时刻能找到公主就输出“YES”，否则输出“NO”。   
`Sample Input`   
1   
5 5 14   
S#*.   
.#…   
…   
****.   
…#.   

….P   
#.…   
**…   
….   
*.#…   
`Sample Output`   
YES

解题思路：类似于三维空间，但完全可以用二维数组做，标记一下在哪一层就可以了。注意两层同一位置都是传输机的情况，可能会造成无限循环。
# Code
```
#include<iostream>
#include<cstring>
#include<cmath>
using namespace std;
int ex,ey,ez;
int n,m,t,flag,T;
int d[4][2]={1,0,-1,0,0,-1,0,1};
int vis[15][15][15];
char a[15][15][15];
void dfs(int x,int y,int z,int step){
	if(flag) return ;
	if(x==ex&&y==ey&&z==ez){
		if(step<=T){
			flag=1;
		}
		return ;
	}
	if(step>=T) return ;
	for(int i=0;i<4;i++){
		int dx=x+d[i][0];
		int dy=y+d[i][1];
		int dz=z;
		if(dx>=0&&dx<n&&dy>=0&&dy<m&&!vis[dz][dx][dy]&&a[dz][dx][dy]!='*'){
			vis[dz][dx][dy]=1;
			if(a[dz][dx][dy]=='#'){
				vis[abs(dz-1)][dx][dy]=1;
				dfs(dx,dy,abs(dz-1),step+1);
				vis[abs(dz-1)][dx][dy]=0;
			}
			else {
				dfs(dx,dy,dz,step+1);
			}
			vis[dz][dx][dy]=0;
		}
	}
}
int main(){
	int t;
	cin>>t;
	while(t--){
		memset(vis,0,sizeof(vis));
		flag=0;
		cin>>n>>m>>T;
		for(int i=0;i<2;i++){
			for(int j=0;j<n;j++){
				for(int k=0;k<m;k++){
					cin>>a[i][j][k];
					if(a[i][j][k]=='P'){
						ex=j,ey=k,ez=i;
					}
				}
			}
		}
		for(int i=0;i<n;i++){
			for(int j=0;j<m;j++){
				if(a[0][i][j]=='#'&&a[1][i][j]=='#'){//两边都是#的情况 ，会造成无限循环 
					vis[0][i][j]=vis[1][i][j]=1;
				}
				if(a[0][i][j]=='*'&&a[1][i][j]=='#'){
					vis[0][i][j]=vis[1][i][j]=1;
				}
				if(a[0][i][j]=='#'&&a[1][i][j]=='*'){
					vis[0][i][j]=vis[1][i][j]=1;
				}
			}
		}
		dfs(0,0,0,0);
		if(flag) cout<<"YES"<<endl;
		else cout<<"NO"<<endl;
	}
	
	return 0;
}
```