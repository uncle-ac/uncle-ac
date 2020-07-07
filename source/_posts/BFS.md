---
layout: post
title: BFS模板题
date: 2020-04-04 22:54:10
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
tags: 搜索
categories: 算法
keywords:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/20.webp
---

上一篇文章讲了dfs的记忆化搜索，来看看上一道题 “仙岛求药” 

## 仙岛求药

少年李逍遥的婶婶病了，王小虎介绍他去一趟仙灵岛，向仙女姐姐要仙丹救婶婶。叛逆但孝顺的李逍遥闯进了仙灵岛，克服了千险万难来到岛的中心，发现仙药摆在了迷阵的深处。迷阵由 M×NM \times NM×N 个方格组成，有的方格内有可以瞬秒李逍遥的怪物，而有的方格内则是安全。现在李逍遥想尽快找到仙药，显然他应避开有怪物的方格，并经过最少的方格，而且那里会有神秘人物等待着他。现在要求你来帮助他实现这个目标。

输入格式

第一行输入两个非零整数 MMM 和 NNN，两者均不大于 202020。MMM 表示迷阵行数, NNN 表示迷阵列数。

接下来有 MMM 行, 每行包含 NNN 个字符,不同字符分别代表不同含义:

1) '@'：少年李逍遥所在的位置；2) '.'：可以安全通行的方格；3) '#'：有怪物的方格；4) '*'：仙药所在位置。

输出格式

输出一行，该行包含李逍遥找到仙药需要穿过的最少的方格数目(计数包括初始位置的方块)。如果他不可能找到仙药, 则输出 −1-1−1。

输出时每行末尾的多余空格，不影响答案正确性

样例输入1

8 8
.@##...#
#....#.#
#.#.##..
..#.###.
#.#...#.
..###.#.
...#.*..
.#...###

样例输出1

10

样例输入2

6 5

.*.#.

.#...

..##.

.....

.#...

....@

样例输出2

8

样例输入3

9 6

.#..#. 

.#.*.# 

.####. 

..#... 

..#... 

..#... 

..#... 

#.@.## 

.#..#.

样例输出3

-1

ok，如果直接用dfs做而不加任何优化会TLE，现在我们来用BFS做，BFS和DFS比较，它的优点就是时间快，但相应的空间上也耗损的更多，个人感觉如果仅仅是打ACM，这个特点比较好，毕竟大多数题还是卡时间而不是卡空间，来 see yi see BFS吧

{% fb_img https://img-blog.csdnimg.cn/20200416093927119.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093935363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093942750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416093956703.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20200416094004328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

还是比较容易理解的吧，毕竟只是一个模板没有加任何优化

现在回到上一道题，贴代码：
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
using namespace std;
struct node{
	int x,y;
	int step;
	int f;
}que[500];
int vis[30][30];
int main()
{
	int m,n,stx,sty,tgx,tgy,head=1,tail=1;  //定义了地图大小，起始坐标，目标坐标，队列的头和尾 
	int Next[4][2]={{1,0},{0,1},{-1,0},{0,-1}};//定义了下一步的操作 
	char map[30][30];//定义了地图 
	cin>>m>>n;//输入地图大小 
	for(int i=0;i<m;i++){
		for(int j=0;j<n;j++){
			scanf(" %c",&map[i][j]);//输入地图 
			if(map[i][j]=='@'){//找到起始位置 
				stx=i; sty=j;
			}
			if(map[i][j]=='*'){//找到目标位置 
				tgx=i; tgy=j;
			}
		}
	}
	vis[stx][sty]=1;//标记起始位置 
	que[tail].x=stx;//初始化队列 
	que[tail].y=sty; 
	que[tail].step=0;//开始位置步数为零 
	que[tail].f=0; 
	tail++;       //tail指向队列最后一个元素的下一个位置 
	int tx,ty,flag=0; //定义一个临时坐标和flag判断是否到达目标位置 
	while(head<tail){
		for(int i=0;i<4;i++){  //移动坐标 
			tx=que[head].x+Next[i][0];
			ty=que[head].y+Next[i][1];
			if(tx<0||tx>=m||ty<0||ty>=n) continue;//判断边界 
			if(!vis[tx][ty]&&map[tx][ty]!='#'){ //判断当前位置是否走过，当前位置能不能走 
				vis[tx][ty]=1;  //标记此位置已走过 				 
				que[tail].x=tx;  //入队 
				que[tail].y=ty;
				que[tail].f=head;  //新入队的父亲节点是队列的头 
				que[tail].step=que[que[tail].f].step+1; //当前步数等于父亲的步数加一 
				tail++;  //tail后移 
			}
			if(tx==tgx&&ty==tgy){  //判断是否到到达目标点 
				flag=1;  //更新flag 
				break;   //到达目标break 
			}
		}
		if(flag==1) break;  //到达目标break 
		head++;  //出队，让后面的点进行扩展 
	}
	if(flag==1) cout<<que[tail-1].step<<endl;
	else cout<<"-1"<<endl;	
	
}
```
如果您精通STL的容器，可以直接使用STL自带的queue，这里只是模板，自己写一个队列并不难

>制作不易，可否赞助一下我这只小可怜两毛钱呢，小可怜已经连饭都吃不上了（owo）