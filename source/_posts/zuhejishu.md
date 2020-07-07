---
layout: post
title: 组合数计数详解
date: 2020-04-06 22:32:27
tags: 高性能
categories: 算法
author: uncleacc
avatar: 'https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg'
authorLink:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/18.webp
---

这个算法我打算结合具体题目来讲解
## 题目背景
因为题目中有好多特殊字符，我不会用所以只给一个链接吧

题目链接 https://www.luogu.com.cn/problem/P2822
这道题就考了两个知识点，一个是组合计数，一个是前缀和，先讲一下组合计数
## 组合计数
C(m,n)这个学过高中数学的都知道，就等于(m!)/(n!*(m-n)!)，但是仅仅知道这个是远远不够的，就像这道题，如果每次枚举都一个一个计算此时C(m,n)的值的话，百分百TLE，所以我们要学习一些巧妙的方法，看这个公式C(m,n)=C(m-1,n)+C(m-1,n-1)，这是组合计数的核心公式，他是怎么推出来的呢？可以看这张图

{% fb_img https://img-blog.csdnimg.cn/20200703230035630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

知道了这个公式我们就可以先初始化前两个值，然后对后面的进行打表，用二维数组来表示C(m,n)

代码：

    int C[210][210];
    void init(){
        C[1][1]=1;
        C[2][1]=C[2][2]=1;
        for(int i=3;i<=200;i++){
            C[i][1]=1;
            for(int j=2;j<=i;j++){
                C[i][j]=C[i-1][j]+C[i-1][j-1];
            }
        }
    }
看一下运行结果（这里只打印十行）：

{% fb_img https://img-blog.csdnimg.cn/20200703230034749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}
细心的你发现了吗？这其实是一个杨辉三角，上面的组合计数公式就是杨辉三角的产生规则，打出了表，这道题其实就完成了一半了，接下来对代码进行一下优化，因为这道题是求C(m,n)能够整除k的情况，看一下数据范围，m和n都是最多可以到2000的，而杨辉三角层数int范围以内的最大容纳层数只有35层，可以参考下面代码：

    #include<bits/stdc++.h>
    using namespace std;
    int C[210][210];
    void init(){
        C[1][1]=1;
        C[2][1]=C[2][2]=1;
        for(int i=3;i<=200;i++){
            C[i][1]=1;
            for(int j=2;j<=i;j++){
                C[i][j]=C[i-1][j]+C[i-1][j-1];
            }
        }
    }
    int main()
    {
        init();
        for(int i=1;i<=100;i++){
            for(int j=1;j<=i;j++){
                if(C[i][j]<0){
                    cout<<endl;
                    cout<<i<<endl; goto skip;
                }
    //			if(j==i) cout<<C[i][j]<<endl;
    //			else cout<<C[i][j]<<" ";
            }
        }
        skip:;
    } 
所以这道题就要每次运算都对k取模，并把运算后的值赋给C[i][j]，当C[i][j]==0时条件成立，ok到这里如果你写出代码不出bug就能混到95分了，参考代码（这里的read是快读，我写的前几篇博客里面有）：

    inline void build()
    {
    c[0][0]=1;
    c[1][0]=c[1][1]=1;
    for(int i=2;i<=2000;i++)
    {
        c[i][0]=1;
        for(int j=1;j<=2000;j++)
        {
        c[i][j]=(c[i-1][j-1]+c[i-1][j])%k;
        if(c[i][j]%k==0)s[i][j]=1;
        }
    }
    }
    inline void solve()
    {
    t=read(),k=read();
    build();
    while(t--)
    {
        ans=0;
        n=read(),m=read();
        for(int i=0;i<=n;i++)
        for(int j=0;j<=my_min(i,m);j++)
            ans+=s[i][j];
        printf("%lld\n",ans);
    }
    }
可是还是不能AC，问题就出在这里时给了t组询问，每次询问都得重新枚举，非常耗费时间，对于这种查询次数多的题目，就要想到前缀和算法，这个算法直接每次查询的结果表达成为一个公式，每次查询复杂度O(1)，在这里我不讲解前缀和，可以参考 https://blog.csdn.net/k_r_forever/article/details/81775899 ok了解了前缀和，这道题就有思路了，我们可以新开一个二维数组来储存右下角坐标为该数组坐标的矩阵中C(m,n)%k==0的所有情况之和，具体看代码：

    #include <iostream>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    int t,k,n,m;
    int c[2005][2005],s[2005][2005];  //s就是记录二维前缀和的数组
    void init(){
        c[0][0]=1;
        c[1][0]=c[1][1]=1;
        for(int i=2;i<=2000;i++){
            c[i][0]=1;
            for(int j=1;j<=i;j++){
                c[i][j]=(c[i-1][j]+c[i-1][j-1])%k;
                s[i][j]=s[i-1][j]+s[i][j-1]-s[i-1][j-1];  //二位前缀和公式
                if(!c[i][j]) s[i][j]++;  //满足条件该点的数值加一
            }
            s[i][i+1]=s[i][i]; //这一步也是关键，下面讲解
        }
    }
    int main(){
        cin>>t>>k;
        init();
        while(t--){
            cin>>n>>m;
            if(m>n) m=n;
            cout<<s[n][m]<<endl;
        }    
        return 0;
    } 

上面的s[i][i+1]=s[i][i]可能你看不懂，ok我来解释
{% fb_img https://img-blog.csdnimg.cn/20200703230033713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}

鼠标写字丑，谅解一下。下面就用颜色来说了，如果有色盲或者色弱的同学lz表示抱歉）

以上三个输出的是什么呢，当输入k的时候，将代码中的flag数组前10x10 位打出来，此处k=5.

图1，是正确的写法，代码如下：（和前面差不多，此处只是为了输出比较，可以跳过这个代码不看）

    void yh(){
    f[0][0]=f[1][0]=f[1][1]=1;
    for (int i=2;i<=2000;i++){
        f[i][0]=1;
        for (int j=1;j<=i;j++){//图3是j<=2000，为了把整个矩形打出来看
            f[i][j]=(f[i-1][j-1]%k+f[i-1][j]%k)%k;
            flag[i][j]=flag[i-1][j]+flag[i][j-1]-flag[i-1][j-1];
            if (f[i][j]==0) flag[i][j]++;
        }
        flag[i][i+1]=flag[i][i];//图1的写了这句重要的话，图2没写
    }//i是行数，j是列数 
    for (int i=0;i<=10;i++){
        for (int j=0;j<=10;j++){
            cout<<flag[i][j]<<"   ";
        }
        cout<<endl;
    }
    }
图2图3代码与图1的区别在上面代码的注释中

对比1、2的结果来看，从 j=6开始，输出就不一样了（米色笔圈出来的地方），且2的输出要比1的小。

为了方便说明原因举个栗子，如图中用白色光标点出来的部分（图1是7，图2是3）。为什么图2会比正确答案小呢？我们看看图3就知道了。图3中蓝笔圈出来的，就是图1没打出来的部分flag数组。

关注点转移到三个结果都用绿色笔圈出来的部分。由3可一看出，1中（绿色圈出来的）右上角没打出来的其实是4，（因为flag[i][i+1]flag[i][i+1]flag[i][i+1]=flag[i][i]flag[i][i]flag[i][i]，这个4实际上是由空格处前一个的4转移来的）；而因为没用这句话，2中右上角的空格实际上是0（没有进行过操作，就是flag数组的初值0）

如果将这个空格里的数设为x，白色光标的数设为y的话，那么由于flag中的数是由前缀和算出来的，y=x+4+7+4-4=x+4+7.如果没用这句话，那么结果就会像2中一样，反而比前面小。范围大了反而符合要求的数字变少了，这很显然就是错误的。

还有一点，为什么这个式子就是正确的？

既然要证实这个，我们不妨抛弃掉所以有技巧的方法，就用最最基本的公式死算暴力出答案看看是不是这样的，实验结果如下：
{% fb_img https://img-blog.csdnimg.cn/20200703230033259.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}
可以看出，flag[i][i+1]flag[i][i+1]flag[i][i+1]和flag[i][i]flag[i][i]flag[i][i]的的确确是一样的，那么flag[i][i+1] flag[i][i+1]flag[i][i+1]=flag[i][i]flag[i][i]flag[i][i]就能合情合理且正确地用了。简单理解可以就把它认为是设置边界。

OK，讲完了，真不容易啊，已经晚上11点半了

>制作不易，能否点击下面的赏赞助我这只蒟蒻2毛钱呢，在此谢谢各位老板了（owo）

留下您的困惑，我会按时解答哦