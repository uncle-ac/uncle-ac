---
layout: post
title: 快速幂详解
date: 2020-04-05 19:39:12
tags: 基础数学
categories: 算法
author: uncleacc
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/8.webp
---

## 原理
先举个例子，比如说算3^6,你要怎么算，用6个6相乘对不对，那要是3^1000呢？1000个3相乘，复杂度为O(N)，现在我们这样算，6的二进制是110，所以6=1*(2^2)+1*(2^1)+0*(2^0)，那么3^6就变成了3^( 1*(2^2)+1*(2^1)+0*(2^0) )=3^(1*(2^2)) * 3^(1*(2^1)) * 3^(0*(2^0))，这其实就是快速幂的原理，看起来麻烦了对吗？OK，先不看复杂度，先看用代码如何实现，我们可以用一个数来充当3^(1*(2^2))、3^(1*(2^1))、3^(0*(2^0))，在下面的代码中y就是这个变量，不是每一次都要算的，比如3^(0*(2^0))=1，乘不乘都一样，那怎么判断呢？我们每次取二进制数的最后一位，要么是0要么是1，如果是0，就不用不用乘，否则就乘，先看代码：
```
int p(int a,int b){
  int t,y;  //定义两个变量，t起到类乘的作用，而y则就是每一次要乘的数
  t=1; y=a;  //注意一定要初始化
  while (b!=0){  //只要二进制位数还没有遍历完就还要循环
    if (b&1==1) t=t*y; //y就是幂的形式a^(2^0),a^(2^1),a^(2^2),a^(2^3)
    y=y*y;  //
    b=b>>1;  //每次要舍去二进制数的最后一位
  }
  return t;
}
```
这就是原理是不是很简单呢（qwq）
## 复杂度
也许你会疑惑明明步骤变多了怎么会快了呢？这只是你的直观感觉，代码是变长了，但次数确确实实变少了，我们来看上面的代码，复杂度主要就在于循环上，循环的条件是b!=0，而每次不都要除以2，设循环次数为x，那么2^x=b，x=log(2,b)，循环里面运算次数最多为3次，那么复杂度最大就是3log(2,N)，3是常数，当数值很大时，可以省略，复杂度为log(2,N)，那么假如算2^10000，常规需要10000次运算，而快速幂只需要14*3次运算，是不是很神奇呢？
## 应用
学习了这个算法有什么用呢？实际上快速幂是一个非常常用的算法，它经常与其他算法一起混用，一般来说快速幂适用于求一个指数非常高的数对某个数的余数，例如：题目：
现在星期日，问：再过2^10000天后星期几？学习了快速幂这种题就是送分题了，每次对7取余就行了
## 题目

附上链接，有兴趣的小伙伴可以前往相关网站做一下

    Fermat's theorem states that for any prime number p and for any integer a > 1, ap = a (mod p). That is, if we raise a to the pth power and divide by p, the remainder is a. Some (but not very many) non-prime values of p, known as base-a pseudoprimes, have this property for some a. (And some, known as Carmichael Numbers, are base-a pseudoprimes for all a.)

    Given 2 < p ≤ 1000000000 and 1 < a < p, determine whether or not p is a base-a pseudoprime.
Input

    Input contains several test cases followed by a line containing "0 0". Each test case consists of a line containing p and a.
Output

    For each test case, output "yes" if p is a base-a pseudoprime; otherwise output "no".
Sample Input

    3 2
    10 3
    341 2
    341 3
    1105 2
    1105 3
    0 0

Sample Output

    no
    no
    yes
    no
    yes
    yes
AC代码：

    #include<stdio.h>
    #include<iostream>
    #include<algorithm>
    using namespace std;
    long long p(long long a,long long b){
        long long t,y,n=b;
        t=1; y=a;
        while (b!=0){
            if (b&1==1) t=t*y%n;
            y=y*y%n;
            b=b>>1;
        }	
        return t;
    }
    int main()
    {
        long long m,n;
        while(scanf("%lld %lld",&m,&n)!=EOF){
            if(m==0&&n==0) break;
            int flag=0;
            for(int i=2;i*i<=m;i++){
                if(m%i==0){
                    flag=1;
                    break;
                }
            }
            if(flag==0) cout<<"no"<<endl;
            else{
                if(p(n,m)==n) cout<<"yes"<<endl;
                else cout<<"no"<<endl;
            }
        }
    }
地址：http://poj.org/problem?id=3641

Rightmost Digit 

Given a positive integer N, you should output the most right digit of N^N.

Input
   
    The input contains several test cases. The first line of the input is a single integer T which is the number of test cases. T test cases follow.
    Each test case contains a single positive integer N(1<=N<=1,000,000,000).
Output
    For each test case, you should output the rightmost digit of N^N.
Sample Input

    2
    3
    4

Sample Output

    7
    6


            
      

Hint

    In the first case, 3 * 3 * 3 = 27, so the rightmost digit is 7.
    In the second case, 4 * 4 * 4 * 4 = 256, so the rightmost digit is 6.

AC代码：

    #include<stdio.h>
    #include<iostream>
    #include<algorithm>
    using namespace std;
    long long p(long long a){
        long long t,y;
        t=1; y=a;
        while(a!=0){
            if (a&1==1) t=t*y%10;
            y=y*y%10; 
            a=a>>1;
        }
        return t;
    }
    int main()
    {
        int t;
        cin>>t;
        while(t--){
            long long n;
            scanf("%lld",&n);
            cout<<p(n)<<endl;
        }
    }
地址：http://acm.hdu.edu.cn/showproblem.php?pid=1061

小蒜想知道：假设今天是星期日，那么过 aba^bab 天之后是星期几？

输入格式

两个正整数 aaa，bbb，中间用单个空格隔开。0<a≤100,0<b≤100000 < a \le 100, 0 < b \le 100000<a≤100,0<b≤10000。

输出格式

一个字符串，代表过 aba^bab ​天之后是星期几。

其中，Monday 是星期一，Tuesday 是星期二，Wednesday 是星期三，Thursday 是星期四，Friday 是星期五，Saturday 是星期六，Sunday 是星期日。

输出时每行末尾的多余空格，不影响答案正确性

样例输入

    3 2000

样例输出

    Tuesday
AC代码

    #include<iostream>
    #include<stdio.h>
    #include<math.h>
    #include<string.h>
    using namespace std;
    typedef long long ll;
    ll p(ll a,ll b){
        int t,y;
        t=1; y=a;
        while (b!=0){
            if (b&1==1) t=t*y%7;
            y=y*y%7; 
            b=b>>1;
        }
        return t;
    }
    int main()
    {
        int a,b;
        cin>>a>>b;
        switch(p(a,b)){
            case 1: printf("Monday\n");break;
            case 2: printf("Tuesday\n");break;
            case 3: printf("Wednesday\n");break;
            case 4: printf("Thursday\n");break;
            case 5: printf("Friday\n");break;
            case 6: printf("Saturday \n");break;
            case 0: printf("Sunday\n");break;
        }
    }
地址：https://nanti.jisuanke.com/t/T1234

>制作不易，您的赞助是我最大的动力，留下您的评论，有条件的小伙伴可以打赏2毛钱，谢谢各位老板（owo）

![](https://dss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1690746261,260501959&fm=111&gp=0.jpg)