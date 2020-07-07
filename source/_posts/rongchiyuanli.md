---
layout: post
title: 容斥原理
date: 2020-04-07 17:52:08
tags: 算法
categories: 算法
author: uncleacc
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/4.webp
---

今天学习了容斥原理，感觉智商又一次遭到了蹂躏（eoe），百度了CSDN上面的讲解，感觉讲的都不是很详细（或许真的是我笨吧，哎~），还是结合题目来讲吧，上题：

I - Co-prime 

Given a number N, you are asked to count the number of integers between A and B inclusive which are relatively prime to N.
Two integers are said to be co-prime or relatively prime if they have no common positive divisors other than 1 or, equivalently, if their greatest common divisor is 1. The number 1 is relatively prime to every integer. 

Input

    The first line on input contains T (0 < T <= 100) the number of test cases, each of the next T lines contains three integers A, B, N where (1 <= A <= B <= 10^15) and (1 <=N <= 10^9).
Output

    For each test case, print the number of integers between A and B inclusive which are relatively prime to N. Follow the output format below.
Sample Input

    2
    1 10 2
    3 15 5

Sample Output

    Case #1: 5
    Case #2: 10

Hint

    In the first test case, the five integers in range [1,10] which are relatively prime to 2 are {1,3,5,7,9}. 

题目的意思给定一个区间的左端点和右端点，再给你一个数，求在这个区间里面和这个数互质的数的个数，只要您不是初学者就应该知道这么10^15如此大区间不可能去遍历它，这时候就要用到容斥原理了，接下来是重点（。。敲黑板。。），注意看！！

## 原理
举个例子，如何把一个区间中2，3，5的的倍数全部筛掉，假如区间是1-20，我们先筛2的倍数，那么筛掉的数量就是20/2=10，同理筛3的倍数筛掉的数量就是20/3（注意向下取整），5同理，ok，筛完了，但是实际上答案是不对的，细心的你一定发现了，2和3的乘积6被多次筛选了，同理2和5，3和5也被多次筛选了，因此我们还要把6，10，15的倍数再减一遍，然后我们再去找2，3，5的乘积筛了几遍，上面我们单独筛2，3，5的时候我们是把2，3，5的乘积筛了3遍，之后又筛选两两乘积的时候又筛了三遍，因为单独筛选时用的是加法，而两两乘积的筛选用的是减法，这就导致了2，3，5的乘积实际上一次没有筛，所以我们最后还要加上2，3，5的乘积的筛选个数，什么？看不懂，行，给你画图：

{% fb_img https://img-blog.csdnimg.cn/20200703230236206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

其实就是求图形的面积，我们第一次加上了2，3，5的3个大圆面积，之后又减去了6，10，15的3个椭圆的面积，而最中间的小圆相当于一次没加，最后加上就行了，然后我们会发现，出现奇数个数，就用加法，偶数个数用减法。

最后的式子是这样的：k / 2 + k / 3 + k / 5 - k / (2 * 3) - k / (3 * 5) - k / (2 * 5) + k / (2 * 3 * 5)是不是很简单呢（qwq）

## 分析题解
ok，讲完了容斥的原理，接下来就能做题了，题目要求区间段内与k互质的个数，就是求区间段内与k不互质的个数，然后再用区间长度减去这个数就行了，求[l,r]中与k不互质的个数，就是求[1,r]-[1,l-1]（一定注意是l-1，因为包括l），理清了思路，就可以敲代码了

## 分解质因子的代码
上面忘了说要筛掉一个区间中与k不互质的个数，首先要把k分解成质因子，首先应该知道任何一个数要不是质数，要不是可以由多个质数相乘得到，例如：10=2 * 5, 30=2 * 3 * 5 , 50=2 * 5 * 5，利用这个性质，就可以分解了

    for(int i=2;i*i<=k;i++){
        if(k%i==0){
            p[++tail]=i;  //p就是储存质因子的数组
            while(k%i==0) k/=i;  //把k中所有i的质因子全部除去
        }
    }
    if(k>1) p[++tail]=k;  //最后如果大于一，则最后一个数一定是质因子，这一步可能有一点难理解，可以多想想
## 实现容斥定理的代码

    long long fun(long long x){
        long long res=0;  //res储存的是1-x中与K不互质的数量 
        for(int i=1;i<(1<<tail);i++){  //这里的1<<tail是指2的tail次方，表示tail个质因子有多少种组合情况 
            long long cur=1,cnt=0;  //cur表示在当前选中的质因子中的乘积，cnt表示当前选中的数量是奇数还是偶数 
            for(int j=0;j<tail;j++){  //这个循环是枚举tail的二进制形式 
                if((i>>j)&1){  //这个是判断i的第j位是不是1，如果是则表示选中第j个数 
                    cnt++;  //表示选中了几个数，每选中一个就加一 
                    cur*=p[j+1];  //选中第j个数就用cur乘以第j个质因子数，注意质因子数组是从1开始的，所以要加一 
                }
            }
            if(cnt&1) res+=x/cur;  //如果cnt是偶数就相加 
            else res-=x/cur;  //奇数就相减 
        }
        return x-res;  //res储存的是1-x中与K不互质的数量，所以要用x-res得到互质的数量 
    }

这一段代码我觉得好难理解，尤其是我在网上查资料感觉讲的不是很详细，想了一下午，才茅塞顿开，因此我希望广大网友在这里能够少花时间，代码里的（1<<tail）
实际上就是2^tail，表示n个质因子的组合情况数，需要说明这里面包括空集，所以循环从条件是小于而不是小于等于，然后每一个i的二进制形式每一位要不是1要不是0，而1<<tail
的二进制形式位数正好等于质因子数，我们就把每一位数字用1表示选中，0表示没有选中，那么我们遍历二进制的每一位数，如果是1，设这是第n位数（从左到右数），就类乘第n个质因子数，最后判断选中了多少位，如果是偶数就加，否则减，然后就完成了，二进制这一段代码确实比较难以理解，不过只要仔细想一想还是能想通的（加油）

然后就结束了，贴一下AC代码吧

    #include<iostream>
    #include<stdio.h>
    #include<algorithm>
    using namespace std;
    int p[1000000];
    int tail;
    long long fun(long long x){
        long long res=0;
        for(int i=1;i<(1<<tail);i++){
            long long cur=1,cnt=0;
            for(int j=0;j<tail;j++){
                if((i>>j)&1){
                    cnt++;
                    cur*=p[j+1];
                }
            }
            if(cnt&1) res+=x/cur;
            else res-=x/cur;
        }
        return x-res;
    }
    int main()
    {
        int t,times=0;
        cin>>t;
        while(t--){
            tail=0;
            long long l,r,k;
            cin>>l>>r>>k;
            for(int i=2;i*i<=k;i++){
                if(k%i==0){
                    p[++tail]=i;
                    while(k%i==0) k/=i;
                }
            }
            if(k>1) p[++tail]=k;
            long long ans=fun(r)-fun(l-1);
            printf("Case #%d: %lld\n",++times,ans);
        }
    }

如果有不懂的，留下评论，我会按时解答的哦

>告诉你一个小秘密，点击下方的赏字，就能够赞助我了哦（乖乖的说）