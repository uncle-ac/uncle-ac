---
layout: post
categories: 算法
author: uncleacc
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
title: dfs记忆化搜索
date: 2020-04-04 22:19:57
tags: 算法
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/21.webp
---

今天学习了记忆化搜索，也练习了许多题，我果然是一个蒟蒻（qwq）

希望下面的讲解对您有所帮助

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

这是一道典型的搜索题目，不过如果你只贴上dfs的模板指定是过不了的，必须进行优化，但是使用BFS的话就直接可以过了，BFS我下一篇文章讲有，我们知道dfs的思路是不撞南墙不回头，但其实有些路径走到一半就知道这是一条不归路了，我们可以用一个数组来储存到达每一个坐标点的最短路径，当搜索的时候如果现在的步数加一大于到达下一个坐标位置的最短路径，则无需继续，直接改变方向，贴代码：

```
#include<iostream>
#include<cmath>
#include<cstring>
using namespace std;
char a[25][25];  //地图
int step[25][25];  //记录每一个走到的位置的最短路径
int dx[5]={1,0,0,-1};  //下一个位置
int dy[5]={0,1,-1,0};
int n,m;  //地图大小
bool flag = false;  //标记
void dfs(int x,int y)
{
    if(a[x][y]=='*'){  //到达终点返回
        flag = true;
        return ;
    }
    for(int i=0;i<4;i++){
//就是优化到这里了，加了一个判断如果当前位置的步数+1 > 下一个位置的最小步数，就不用走了否则就更新下一个位置的最小步数

        int tx=x+dx[i];
        int ty=y+dy[i];
        if(tx>=0 && tx<n && ty<m && ty>=0 && a[tx][ty]!='#'&& step[x][y]+1<step[tx][ty]){
            step[tx][ty] = step[x][y]+1;  //更新这个位置的最小坐标
        	dfs(tx,ty);  //继续搜索下一位置
        }
    }
}
int main()
{
    int qx,zx,qy,zy;
    cin >> n >> m;
    for(int i=0;i<n;i++)
        for(int j=0;j<m;j++)
            step[i][j]=1<<30; //初始化每一个坐标的最小路径，初始值很大
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            cin >> a[i][j];
            if(a[i][j]=='@'){
                qx = i; qy = j; //找到起始位置，把起始位置的最短路径设置为0
                step[qx][qy]=0;
            }
            if(a[i][j]=='*'){  //找到终点
                zx = i; zy = j;
            }
        }
    }
    dfs(qx,qy);
    if(flag)
        cout << step[zx][zy];
    else
        cout << "-1";
    return 0;
}


```

## 滑雪

Description
Michael喜欢滑雪百这并不奇怪， 因为滑雪的确很刺激。可是为了获得速度，滑的区域必须向下倾斜，而且当你滑到坡底，你不得不再次走上坡或者等待升降机来载你。Michael想知道载一个区域中最长底滑坡。区域由一个二维数组给出。数组的每个数字代表点的高度。下面是一个例子 

 1  2  3  4 5

16 17 18 19 6

15 24 25 20 7

14 23 22 21 8

13 12 11 10 9


一个人可以从某个点滑向上下左右相邻四个点之一，当且仅当高度减小。在上面的例子中，一条可滑行的滑坡为24-17-16-1。当然25-24-23-...-3-2-1更长。事实上，这是最长的一条。

Input

输入的第一行表示区域的行数R和列数C(1 <= R,C <= 100)。下面是R行，每行有C个整数，代表高度h，0<=h<=10000。

Output

输出最长区域的长度。

Sample Input

5 5

1 2 3 4 5

16 17 18 19 6

15 24 25 20 7

14 23 22 21 8

13 12 11 10 9

Sample Output

25

Source
SHTSC 2002


解题思路：

记忆化搜索比普通的搜索效率要高，已经搜索过的就不用再搜索了，有DP的思想.step[i][j] 保存的是当前坐标i,j可以到达的最大距离（不包括自己），比如3 2 1，当前是3，那么可以到达的最大距离为2，对于每个坐标，可以上下左右四个方向搜索，取距离最大的那个，作为该坐标的step[][]。记忆化搜索这里体现的就是当搜索到某一个坐标时，该坐标的step[][]已经有值（搜索过了），且肯定是最优的，那么直接返回该step[][]值就可以了。

代码：
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
using namespace std;
const int maxn=110;
int map[maxn][maxn];
int step[maxn][maxn];
int Next[][2]={{1,0},{0,1},{-1,0},{0,-1}};
int m,n;
int judge(int x,int y){
	if(x>=0&&x<m&&y>=0&&y<n) return 1;
	else return 0;
}
int dfs(int x,int y){
	if(step[x][y]) return step[x][y];
	for(int i=0;i<4;i++){
		int tx=x+Next[i][0];
		int ty=y+Next[i][1];
		if(judge(tx,ty)&&map[tx][ty]<map[x][y]){
			int temp=dfs(tx,ty)+1;
			step[x][y]=max(temp,step[x][y]);
		}
	}
	return step[x][y];
}
int main()
{
	int ans=0;
	cin>>m>>n;
	for(int i=0;i<m;i++){
		for(int j=0;j<n;j++){
			cin>>map[i][j];
		}
	}
	for(int i=0;i<m;i++){
		for(int j=0;j<n;j++){
			step[i][j]=dfs(i,j);
			if(ans<step[i][j]) ans=step[i][j];
		}
	}
	cout<<ans+1<<endl;
}
```

>制作不易，各位老板，可否赏小可怜2毛钱呢（owo），感谢各位老板