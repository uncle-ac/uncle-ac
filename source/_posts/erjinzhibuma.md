---
title: 二进制补码
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 记录
comments: true
date: 2020-04-12 22:49:22
tags: 二进制
keywords:
description:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/3.webp
---

这道题看百度上的题解把我看蒙了，想了半天没想通，直到我看见在计算机中负数是用补码来表示的，我才恍然大悟，咋把这个给忘了（抓狂）
{% fb_img https://img-blog.csdnimg.cn/20200703225037185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

## 题目
蒜头君有一个 int\text{int}int 的整数，输出它的 323232 位二进制补码。

输入格式

一个整型整数。

输出格式

输出一行，即该整数的补码表示。

输出时每行末尾的多余空格，不影响答案正确性

样例输入：

7

样例输出

00000000000000000000000000000111

## 分析：
做这道题就是明白一点：计算机中负数用补码来表示，因为整数补码是本身，所以这道题其实就是输出一个数在计算机中的二进制形式，超级简单了
## code

    #include<iostream>
    #include<stdio.h>
    using namespace std;
    int map1[110][110],map2[110][110];
    int main()
    {
      int n;
      cin>>n;
      for(int i=31;i>=0;i--){
        cout<<((n>>i)&1); //输出这一位的数
      }
    }

## 补充
[传送门](https://zhidao.baidu.com/question/1370612947989125499.html)
### %i的作用
%i和%d区别在scanf中，%i功能更强大，能根据输入的形式转换成十进制并赋值给变量，比如下面程序：

    int n;
      scanf("%i",&n);
      cout<<n;
    }
输入0X10，输出16,输入0010输出8(八进制)
>c语言不能直接表示二进制,没有数字前缀表示
### itoa函数
[传送门](https://baike.baidu.com/item/itoa%E5%87%BD%E6%95%B0/3260813?fr=aladdin)

C语言中可以用%o %d/%i %x输出8 10 16进制的数但是没有二进制的输出字符，但是有itoa的函数，
>注意: 计算机中负数用补码表示，所以itoa函数求出的负数也是补码形式

函数原型：

    char *itoa( int value, char *string,int radix); [1] 
    原型说明：
    value：欲转换的数据。
    string：目标字符串的地址。
    radix：转换后的进制数，可以是10进制、16进制等。

参考代码：

1

    #include <stdlib.h>
    #include <stdio.h>
    int main(void)
    {
        int number = 12345;
        char string[32];
        itoa(number, string, 10);
        printf("integer = %d string = %s\n", number, string);
        return 0;
    }
2

    /* itoa example */
    #include <stdio.h>
    #include <stdlib.h>
    int main (){
        int i;
        char buffer[33];
        printf ("Enter a number: ");
        scanf ("%d",&i);    //输入整数i
        itoa (i,buffer,10);    //将i转化为10进制数，存到buffer中
        printf ("decimal: %s\n",buffer);    //输出打印buffer
        itoa (i,buffer,16);    //将i转化为16进制数，存到buffer中
        printf ("hexadecimal: %s\n",buffer);    //输出打印buffer
        itoa (i,buffer,2);    //将i转化为2进制数，存到buffer中
        printf ("binary: %s\n",buffer);    //输出打印buffer
        return 0;}
    OUTPUT:
    Enter a number: 1750
    decimal: 1750
    hexadecimal: 6d6
    binary: 11011010110

>注意事项

itoa() 函数有3个参数：第一个参数是要转换的数字，第二个参数是要写入转换结果的目标字符串，第三个参数是转移数字时所用的基数(进制)。在上例中，转换基数为10，就意味着以10为转换进制。10：十进制；2：二进制...

itoa 并不是一个标准的C函数，它是Windows特有的，如果要写跨平台的程序，请用sprintf。

标准库中有sprintf，功能比这个更强，用法跟printf类似：

char str[255];

sprintf(str, "%x", 100); //将100转为16进制表示的字符串。

下列函数也可以将相应类型的整数转换为字符串：

```    char *ultoa(unsigned long value,char *string,int radix)
　　将无符号整型数value转换成字符串并返回该字符串,radix为转换时所用基数
　　char *ltoa(long value,char *string,int radix)
　　将长整型数value转换成字符串并返回该字符串,radix为转换时所用基数
　　char *itoa(int value,char *string,int radix)
　　将整数value转换成字串存入string,radix为转换所用基数.
　　double atof(char *nptr)
　　将字符串nptr转换成双精度数,并返回这个数,错误返回0
　　int atoi(char *nptr)
　　将字符串nptr转换成整型数, 并返回这个数,错误返回0
　　long atol(char *nptr)
　　将字符串nptr转换成长整型数,并返回这个数,错误返回0
　　double strtod(char *str,char **endptr)
　　将字符串str转换成双精度数,并返回这个数,
　　long strtol(char *str,char **endptr,int base)
　　将字符串str转换成长整型数, 并返回这个数
```
ok,又11点多了，哎！啥也不是