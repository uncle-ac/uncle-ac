---
title: 贪吃蛇
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 
comments: true
date: 2020-04-21 14:43:52
tags: 项目
keywords:
description: 一个功能较为完善的贪吃蛇小程序
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/15.webp
---
经历了几个月对C和C++的学习，已经懂得基本原理的我准备自己写一个小项目了:smile:
先来look look成果吧！
[最后成品](https://www.bilibili.com/video/BV1tK4y1k7az)
# Code
想写一个功能较为完备的贪吃蛇应该把它作为一个项目去写，而不仅仅是把所有代码写在一个文件里，这应该是程序员最基本的意识了

首先要创建一个结构体，因为我们要用到图形化编程，而图形界面是由一个个点组成的
## 坐标（头文件）
    #ifndef MYPOINT_H
    #define MYPOINT_H
    //描述点的结构
    struct myPoint {
      int x;
      int y;
    };
    #endif
接下来我们需要创建一个窗口，还是一样窗口的尺寸同样是一个常用的参量，我们也可以把它单独写在一个头文件里
## 窗口尺寸（头文件）
    #pragma once      //这里pragma once 和上面的ifndef __  define __ 是一样的
    #define width 450
    #define length 750
做贪吃蛇首先要明白贪吃蛇分几部分，第一蛇本身，第二就是食物，所以我们就分两部分来做

这里需要说明的是graphics.h这个头文件编译器不是默认安装的，需要自己下载，[下载地址点击我](https://pan.baidu.com/s/1IPfb6IhnpDrYoP-7bxT4uQ)，密码：awr0

## 蛇 （头文件）
    //做蛇
    #ifndef _SNAKE_H
    #define _SNAKE_H
    #include <graphics.h>			//图形化函数
    #include <conio.h>					//按键函数
    #include "Mypoint.h"				//自己写的点的坐标
    class snake {
    public:
      snake();						//构造函数
      void drawSnake();				//画蛇
      void moveSnake();				//移动蛇
      void keyDown();					//按键控制蛇
      //类的封装性
      int& getSnakeSize();			//用来修改蛇的长度接口
      myPoint getSnakePos(int pos);	//获取蛇的pos节坐标
      int judge();					//判断蛇是否死亡
    protected:
      //属性
      myPoint snakePos[100];			//蛇最多节数
      int snakeSize;					//当前节数
      char position;					//方向
      //把方向枚举出来
      enum snakePosition{right,left,down, up};
    };
    #endif
在蛇的头文件里面我们创建了一个类，写了关于蛇的一系列操作函数，接下来我们要一一实现这些函数，千万不要害怕，其实特别简单，除了一些图形处理的函数可能你不知道以外，其他的只要学过C语言一般就能理解
## 蛇函数的实现（cpp）
    #include "Snake.h"
    #include "window.h"
    snake::snake() {
      snakeSize = 3;
      //初始化蛇
      for (int i = snakeSize - 1; i >= 0; i--) {
        snakePos[i].x = 10 * (2 - i);
        snakePos[i].y = 0;
      }
      position = right;  //初始化方向：右面
    }
    void snake::drawSnake()//画蛇
    {
      //画地图
      setcolor(BLACK);
      line(0, 0, length, 0);
      //画蛇
      for (int i = 0; i < snakeSize; i++) {
        setfillcolor(RGB(rand() % 255, rand() % 255, rand() % 255));
        fillrectangle(snakePos[i].x, snakePos[i].y, snakePos[i].x + 10, snakePos[i].y + 10);
      }
    }
    void snake::moveSnake()//移动蛇
    {
      //处理除了蛇头以外的
      for (int i = snakeSize - 1; i > 0; i--) {
        snakePos[i].x = snakePos[i - 1].x;
        snakePos[i].y = snakePos[i - 1].y;
      }
      //处理蛇头
      switch (position) {
      case right:
        snakePos[0].x += 10;
        break;
      case left:
        snakePos[0].x -= 10;
        break;
      case down:
        snakePos[0].y += 10;
        break;
      case up:
        snakePos[0].y -= 10;
        break;
      }
    }
    void snake::keyDown()//按键控制蛇
    {
      //按键控制，72,75,77,80是小键盘的上下左右
      char useKey = 0;
      useKey = _getch();		//conion.h
      switch (useKey) {
      case 'w':
      case 'W':
      case 72:
        if (position != down) {
          position = up;
        }
        break;
      case 'A':
      case 'a':
      case 75:
        if (position != right) {
          position = left;
        }
        break;
      case 's':
      case 'S':
      case 80:
        if (position != up) {
          position = down;
        }
        break;
      case 'D':
      case 'd':
      case 77:
        if (position != left) {
          position = right;
        }
        break;
      case 32:
        char ch;
        while ( ch = _getch()) {	//空格暂停
          if (ch == 32) break;
        }
        break;
      }
    }
    int& snake::getSnakeSize()//用来修改蛇的长度接口
    {
      return snakeSize;
    }
    myPoint snake::getSnakePos(int pos)//获取蛇的pos节坐标
    {
      return snakePos[pos];
    }
    int snake::judge()
    {
      //蛇撞墙
      if (snakePos[0].x < 0 || snakePos[0].x>(length-10) || snakePos[0].y < 0 || snakePos[0].y > (width-10) )
      {
        return 0;
      }
      //蛇咬住自己
      for (int i = 1; i < snakeSize; i++)
      {
        if (snakePos[i].x == snakePos[0].x && snakePos[i].y == snakePos[0].y)
        {
          return 0;
        }
      }
      return 1;
    }
实现了蛇，就该写食物了，与蛇类似，先写头文件创建类
## 食物（头文件）
    #ifndef _FOOD_H
    #define _FOOD_H
    #include <graphics.h>
    #include "Mypoint.h"
    class food{
    public:
      food();
      void drawFood();
      void setFoodPos();
      int& getFlag();
      myPoint getFoodPos(); 

    protected:
      myPoint foodPos;
      int foodFlag;
    };
    #endif
## 食物实现（cpp）
    #include "food.h"
    #include "window.h"
    food::food() {
      foodPos.x = rand() % (length / 10) * 10;	//食物放在以10为倍数的位置防止吃不到
      foodPos.y = rand() % (width / 10) * 10;
      foodFlag = 1;					//食物存在
    }
    void food::drawFood() {
      setfillcolor(BLACK);
      fillrectangle(foodPos.x, foodPos.y, foodPos.x + 10, foodPos.y + 10);
    }
    void food::setFoodPos() {
      //食物可能生成在蛇的身上
      foodPos.x = rand() % (length/10) * 10;	//食物放在以10为倍数的位置防止吃不到
      foodPos.y = rand() % (width/10) * 10;
      foodFlag = 1;					//食物存在
    }
    int& food::getFlag() {
      return foodFlag;
      //改变标记
    }
    myPoint food::getFoodPos() {
      return foodPos;
    }
这就做完了，最后只需要调用这些函数就行了
## 主函数（cpp）
    #define _CRT_SECURE_NO_WARNINGS   //使用outtextxy()需要这一句
    #pragma comment(lib, "winmm.lib") //添加背景音乐需要添加这一句
    #include <iostream>
    #include <easyx.h>   //outtextxy函数内容
    #include <math.h> 
    #include "Snake.h"   //记住包含进来我们写的头文件
    #include "food.h"
    #include "window.h"

    using namespace std;
    HWND hwnd = NULL;			//windows 编程的知识，句柄
    int Speed = 180;			//蛇的速度
    int score = 0;				//分数
    int r = 1;				//等级
    char grade[100] = { 0 };	//分数
    char Rank[10] = { 0 };		//等级
    //交互处理，写在这里
    void eatFood(snake *pSnake,food *pFood) {
      //当蛇头坐标和食物坐标相等就可以吃
      //类外访问保护
      if (pSnake->getSnakePos(0).x == pFood->getFoodPos().x &&
        pSnake->getSnakePos(0).y == pFood->getFoodPos().y) {
        //游戏引擎
        score += 10;				//增加分数
        if (score % 50 == 0) {
          sprintf(Rank, "%d", r++);
          Speed -= (int)sqrt(300 - score);
          setbkmode(TRANSPARENT);		//设置模式为透明
          settextcolor(BLACK);			//设置颜色为黑色
          outtextxy((length / 2), (width / 2), "UP-等级提升-当前等级：");//输出到指定位置
          outtextxy((length / 2)+20, (width / 2) + 20, Rank);
          Sleep(3000);
          cleardevice();
        }
        cleardevice();
        pSnake->getSnakeSize()++;	//蛇长度加一
        pFood->getFlag() = 0;		//食物标志变0
      }
    }
    void ptscore(int score) {//更新分数
      sprintf(grade, "%d", score); //将score转换到字符串中
      setbkmode(TRANSPARENT);		//设置模式为透明
      settextcolor(RED);			//设置颜色为红色
      outtextxy((length-40), 20, "分数:");//输出到指定位置
      outtextxy((length-40), 40, grade);	//输出分数
    }
    void Up(int rank) {
      if (score % 50 == 0) {
        sprintf(Rank, "%d", rank++);
        Speed -= (int)sqrt(300-score);
        setbkmode(TRANSPARENT);		//设置模式为透明
        settextcolor(BLACK);			//设置颜色为黑色
        outtextxy((length/2),(width/2) , "UP-等级提升");//输出到指定位置
        outtextxy((length/2), (width/2)+20, Rank);
        Sleep(3000);
        cleardevice();
      }
    }
    int main()
    {
      hwnd=initgraph(length, width);			//初始化长度和宽度窗口
      setbkcolor(GREEN);						//设置背景颜色
      //cleardevice();
      PlaySound("C:\\Users\\86155\\Desktop\\学习\\C语言\\VS2019\\贪吃蛇\\背景音乐\\Ari Pulkkinen - Title Theme.wav", NULL, SND_FILENAME | SND_ASYNC | SND_LOOP);
      snake *pSnake = new snake;
      food *pFood = new food;
      pSnake->drawSnake();//画蛇
      while (1) {
        cleardevice();
        if (pFood->getFlag() == 0) {		//当食物没有时
          //指针指向运算符，访问结构体
          pFood->setFoodPos();			//设置食物位置
        }
        pSnake->drawSnake();				//画蛇
        if (pSnake->judge()==0) {
          MessageBox(hwnd, "游戏结束", "Game Over!!!", 0);
          break;							//退出主循环
        }
        pSnake->moveSnake();				//移动蛇
        pFood->drawFood();					//画食物
        ptscore(score);						//打印分数
        eatFood(pSnake, pFood);				//吃食物做判断
        //只有存在按键才做处理
        if (_kbhit()) {						//存在按键返回1，不存在返回0
          pSnake->keyDown();
        }
        Sleep(Speed);						//速度
      }
      closegraph();
      return 0;
    }
上面的playground()函数是用来播放背景音乐的，第一个参数是音乐在电脑的路径，音乐必须是wav格式的

到这里一个贪吃蛇程序就做好了，原理非常简单，难的就是图形化处理很多函数不知道为此我给大家一个学习图形化处理函数的博客地址：[点击我](https://blog.csdn.net/qq_37385726/article/details/54809305)

>告诉你一个秘密点击下面的赏会有惊喜哦（o w o）