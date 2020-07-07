---
title: 数独
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-04-14 23:05:26
tags: dfs
keywords:
description: 经典dfs
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/14.webp
---
## 题目
题目描述

数独是根据9×9盘面上的已知数字，推理出所有剩余空格的数字，并满足每一行、每一列、每一个粗线宫内的数字均含1-9，不重复。每一道合格的数独谜题都有且仅有唯一答案，推理方法也以此为基础，任何无解或多解的题目都是不合格的。

芬兰一位数学家号称设计出全球最难的“数独游戏”，并刊登在报纸上，让大家去挑战。

这位数学家说，他相信只有“智慧最顶尖”的人才有可能破解这个“数独之谜”。

据介绍，目前数独游戏的难度的等级有一道五级，一是入门等级，五则比较难。不过这位数学家说，他所设计的数独游戏难度等级是十一，可以说是所以数独游戏中，难度最高的等级他还表示，他目前还没遇到解不出来的数独游戏，因此他认为“最具挑战性”的数独游戏并没有出现。
输入格式

一个未填的数独

输出格式

填好的数独

输入输出样例

输入 #1

8 0 0 0 0 0 0 0 0 

0 0 3 6 0 0 0 0 0 

0 7 0 0 9 0 2 0 0 

0 5 0 0 0 7 0 0 0 

0 0 0 0 4 5 7 0 0 

0 0 0 1 0 0 0 3 0 

0 0 1 0 0 0 0 6 8 

0 0 8 5 0 0 0 1 0 

0 9 0 0 0 0 4 0 0

输出 #1

8 1 2 7 5 3 6 4 9 

9 4 3 6 8 2 1 7 5 

6 7 5 4 9 1 2 8 3 

1 5 4 2 3 7 8 9 6 

3 6 9 8 4 5 7 2 1 

2 8 7 1 6 9 5 3 4 

5 2 1 9 7 4 3 6 8 

4 3 8 5 2 6 9 1 7 

7 9 6 3 1 8 4 5 2

## Code：
    #include<cstdio>
    #include<cstring>
    #include<cstdlib>
    #include<iostream>
    using namespace std;
    int map[10][10];
    bool row[10][10],col[10][10],g[10][10];//行，列，第几个格子
    void print()
    {
        for(int i=1;i<=9;i++)
        {
            for(int j=1;j<=9;j++)
                printf("%d ",map[i][j]);
            printf("\n");
        }
        exit(0);
    }
    void dfs(int x,int y)//深搜 
    {
      if(map[x][y]!=0)//9*9中不为零的数直接跳过 
      {
          if(x==9&&y==9) 
              print();//搜索结束后输出 
          if(y==9) //行到顶端后搜索列 
              dfs(x+1,1); 
          else //搜索行 
              dfs(x,y+1);
      }
      if(map[x][y]==0)//等于零时 
      {
          for(int i=1;i<=9;i++)
          { 
              if(!row[x][i]&&!col[y][i]&&!g[(x-1)/3*3+(y-1)/3+1][i])
              {
                  map[x][y]=i;
                  row[x][i]=1;
                  col[y][i]=1;
                  g[(x-1)/3*3+(y-1)/3+1][i]=1;
                  if(x==9&&y==9)                 
                      print();
                  if(y==9) dfs(x+1,1); else dfs(x,y+1);
                  map[x][y]=0;
                  row[x][i]=0;
                  col[y][i]=0;
                  g[(x-1)/3*3+(y-1)/3+1][i]=0;
              }
          } 
      }
    }
    int main()
    {
        for(int i=1;i<=9;i++)
        {
            for(int j=1;j<=9;j++)
            {
                scanf("%d",&map[i][j]);
                if(map[i][j]>0)
                {
                    row[i][map[i][j]]=1;
                    col[j][map[i][j]]=1; 
                    g[(i-1)/3*3+(j-1)/3+1][map[i][j]]=1;
                }
            }
        } 
        dfs(1,1);
        return 0;
    }

## 教训
一定明白一个概念递归不是之后的代码就不执行了，在dfs函数中调用dfs之后后面的代码还是会执行的！dfs输出一定要写在出口那里，不能写在main函数中，注意3*3的格子中也要符合1到9唯一性