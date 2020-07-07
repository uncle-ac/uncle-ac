---
title: 飞翔的小鸟C语言小游戏
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
date: 2020-05-26 19:01:49
tags: 小游戏
keywords:
description: C语言小游戏
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/51.webp
---
>今天有些疲倦，不想学习，就去网上学习做了一个小游戏，如果你是网友，没接触过图形库，要先安装esayx库，网上有许多，在这里不贴了，素材地址： https://pan.baidu.com/s/1GWnLePCiLcxlJHOaBKEeaA 密码：pmzq 💪

* 成品视频: [Here](https://www.bilibili.com/video/BV1354y1Q7iB)
<center>
<font color="black" size=4>
希望该文章能帮助到您
</font>
<font color="red" size=5>
不要白嫖了！！！留下您的评论吧
</font>
</center>
<center>
<font color="green" size=2>
谁能帮我测试一下下面的赏是不是出错了😘
</font>
</center>

```
/*
1.创建一个可视化窗口
2.显示一张背景图
*/
#include <stdio.h>
#include <graphics.h>
#include <conio.h>
#include <stdlib.h>
#include <windows.h>
#include <mmsystem.h>
#include <time.h>
#pragma comment(lib,"winmm.lib")

IMAGE mybird[2];//给图片起名字
IMAGE BG;//背景图片
IMAGE overimg[2];//游戏结束图片
IMAGE up[2];//上面柱子
IMAGE down[2];//下面柱子

struct bird {//鸟的属性
    int x, y;
    int speed;
};
struct zhuzi {
    int x, y;//起始坐标
    int h;//高度
};

bird flybird = { 124,304,100 };//初始化鸟
void loadresource() {
    loadimage(&BG, "飞翔的小鸟/background.bmp");//背景
    loadimage(&mybird[0], "飞翔的小鸟/birdy.bmp", 48, 48);
    loadimage(&mybird[1], "飞翔的小鸟/bird.bmp", 48, 48);
    loadimage(&overimg[0], "飞翔的小鸟/endy.bmp");
    loadimage(&overimg[1], "飞翔的小鸟/end.bmp");
    loadimage(&up[0], "飞翔的小鸟/upy.bmp");
    loadimage(&up[1], "飞翔的小鸟/up.bmp");
    loadimage(&down[0], "飞翔的小鸟/downy.bmp");
    loadimage(&down[1], "飞翔的小鸟/down.bmp");
}
void drawbird(int x,int y) {
    //SCAND的方式贴掩码图
    putimage(x, y, &mybird[0], SRCAND);
    //SRCPAINT的方式贴背景图
    putimage(x, y, &mybird[1], SRCPAINT);
}
//多线程处理音乐因为直接在按键函数里面写会有画面停顿感
DWORD WINAPI playmusic(LPVOID lpParamer) {
    mciSendString("open 飞翔的小鸟/jump.mp3", 0, 0, 0);
    mciSendString("play 飞翔的小鸟/jump.mp3 wait", 0, 0, 0);
    mciSendString("close 飞翔的小鸟/jump.mp3", 0, 0, 0);
    return 0;
}
//撞击柱子
DWORD WINAPI playMusic1(LPVOID lpParamer) { //多线程
    mciSendString("open 飞翔的小鸟/hit.mp3", 0, 0, 0); 
    mciSendString("play 飞翔的小鸟/hit.mp3 wait", 0, 0, 0);
    mciSendString("close 飞翔的小鸟/hit.mp3", 0, 0, 0);
    return 0;
}
DWORD WINAPI playMusic2(LPVOID lpParamer) { //多线程
    mciSendString("open 飞翔的小鸟/gameover.mp3", 0, 0, 0);
    mciSendString("play 飞翔的小鸟/gameover.mp3 wait", 0, 0, 0);
    mciSendString("close 飞翔的小鸟/gameover.mp3", 0, 0, 0);
    return 0;
}
//结束动画
void Gameover() {
    //图片初始位置
    int x = 50;
    int y = 608;
    while (y >= 240) {
        putimage(0, 0, &BG);
        putimage(x, y, &overimg[0], SRCAND);
        putimage(x, y, &overimg[1], SRCPAINT);
        y -= 50;
        Sleep(50);
    }
    CreateThread(NULL, NULL, playMusic2, NULL, NULL, NULL);
    Sleep(5000);
}
//撞击地板或是天花板
int hitfloor() {
    if (flybird.y <= 0 || flybird.y >= (608 - 96)) {
        return 1;
    }
    return 0;
}
//撞到柱子
int hitPillar(zhuzi myPillar[]) {  //撞到柱子
    for (int i = 0; i < 3; ++i) {
        if ((flybird.x+48) >= myPillar[i].x && (flybird.x+48) <= myPillar[i].x + 52|| 
            flybird.x >= myPillar[i].x && flybird.x <= myPillar[i].x + 52)
        {
            if (flybird.y+12 <= myPillar[i].h || flybird.y+12 >= (512 - 320 + myPillar[i].h)||
                (flybird.y+36) <= myPillar[i].h || (flybird.y+36) >= (512 - 320 + myPillar[i].h))
            {
                CreateThread(NULL, NULL, playMusic1, NULL, NULL, NULL);
                return 1;
            }
        }
    }
    return 0;
}
//用户按键处理
void keydown(zhuzi pillar[]) {
    /*
        mciSendString("指令",0,0,0)
        指令:
            open: 打开
            play: 播放
            pause: 暂停
            wait: 等待
            stop: 暂停
            close: 关闭
    */

    char userkey = _getch();
    switch (userkey) {
    case ' ':
        for (int i = 1; i <= flybird.speed - 30; i++) {
            flybird.y -= 1;
            if (hitPillar(pillar)) Gameover();
        }
        CreateThread(NULL, NULL, playmusic, NULL, NULL, NULL);
        break;
    case'z': case'Z':
            char c;
            while (c = _getch()) {
                if (c == 'z' || c == 'Z') break;
            }
            break;
    default:
        break;
    }
}

void initpillar(zhuzi pillar[], int i) {
    pillar[i].h = rand() % 100 + 160;//最低高度160，最大259，[160,259]
    pillar[i].x = 288;
    pillar[i].y = 0;
}

void drawpillar(zhuzi pillar) {
    putimage(pillar.x, 0, 52, pillar.h, &down[0], 0, 320 - pillar.h, SRCAND);//柱子宽度为52
    putimage(pillar.x, 0, 52, pillar.h, &down[1], 0, 320 - pillar.h, SRCPAINT);
    putimage(pillar.x, 512 - (320 - pillar.h), 52, 320 - pillar.h, &up[0], 0, 0, SRCAND);
    putimage(pillar.x, 512 - (320 - pillar.h), 52, 320 - pillar.h, &up[1], 0, 0, SRCPAINT);
}
void xiazhui(zhuzi pillar[]) {
    for (int i = 1; i <= 5; i++) {
        flybird.y += 1;
        if (hitfloor() || hitPillar(pillar)) Gameover();
    }
}
int main() {
    initgraph(288, 608);
    srand((unsigned int)time(NULL));
    zhuzi pillar[3];//柱子英文pillar,窗口大小只能容下三根柱子
    for (int i = 0; i < 3; i++) {
        initpillar(pillar, i);
        //让柱子之间有差距
        pillar[i].x = 288 + i * 150;//柱子之间间距150
    }

    loadresource();
    putimage(0, 0, &BG);
    while (1) {
        putimage(0, 0, &BG);
        drawbird(flybird.x, flybird.y);
        xiazhui(pillar);
        for (int i = 0; i < 3; i++) {
            pillar[i].x -= 10;
            if (pillar[i].x < (-52 - 150)) {
                initpillar(pillar, i);
            }
        }
        for (int i = 0; i < 3; i++) {
            drawpillar(pillar[i]);
        }
        //一定判断按键是否存在
        if (_kbhit()){
            keydown(pillar);
        }
        if (hitfloor() || hitPillar(pillar)) {
            Gameover();//这里不break可以无限播放Gameover动画
            break;
        }
        Sleep(50);
    }
    closegraph();
    return 0;
}
```