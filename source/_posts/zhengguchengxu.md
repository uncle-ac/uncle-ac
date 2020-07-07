---
title: 整蛊小程序消息轰炸
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 
comments: true
date: 2020-05-06 13:29:58
tags: 消息轰炸
keywords:
description: 和你的女朋友分手的最快方法
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/41.webp
---

刚看了抖音上面的微信轰炸小程序，用VBS写的，我不会VBS，但是我会C，闲着没事就去网上查了一下，没想到用C也是这么简洁:grinning:   
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/0.webp %}     
步骤：
  >1：编译此段代码
  >2：打开你要轰炸的联系人的聊天界面
  >3： 提前复制某段内容
  >4： 运行程序
  >5： 输入聊天好友的备注（窗口上面对应的名称）
  >6： 输入轰炸次数
  >7： Ending

{% p center small, 来上代码： %}
```
#include<stdio.h>
#include<Windows.h>
#define time ?//更改发送消息间隔时间 
int main()
{
	int i;
	char name[100];//使用前先复制要轰炸的内容 
	printf("输入你要轰炸的对象的名称：");
	scanf("%s",&name,40);
	printf("输入你要轰炸的次数：");
	scanf("%d",&i);
	HWND H =FindWindow(0,name);//找到对话窗口 
	while(i-->0){
		SendMessage(H,WM_PASTE,0,0);//粘贴内容 
		SendMessage(H,WM_KEYDOWN,VK_RETURN,0);//回车发送
		Sleep(time);
	}
	return 0; 
} 
```
`看看效果：`   
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/Sucai/0~1.webp %}