---
title: 数学问题模板
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 记录
comments: true
date: 2020-04-28 09:46:42
tags: 数学问题记录
keywords:
description:
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/32.webp
---
## 筛选质因子
```c
  for(int i=2;i*i<=k;i++){
      if(k%i==0){
          p[++tail]=i;  //p就是储存质因子的数组
          while(k%i==0) k/=i;  //把k中所有i的质因子全部除去
      }
	}
    if(k>1) p[++tail]=k;
```
## 容斥原理
前言：
1. 计算1-n中m的的倍数的数量时，直接n/m
2. 容斥原理是在互质的数的基础上实现的
公式：

(A+B+C+D+E……)-(A*B+A*C+A*D……+B*C+B*D)+(A*B*C+B*C*D……)-(A*B*C*D+B*C*D*E……)……

规律： 奇数相加，偶数相减

代码：
```c
    p数组储存的是筛选的质因子
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
```
## 欧拉筛
```c
int vis[maxn],p[maxn/10]; //质数密度不大，除以10可以减少空间浪费
int cnt=0;
void prime()
{
    vis[1]=1;
    for(int i=2;i<=maxn;i++){
        if(!vis[i]) p[++cnt]=i;
        for(int j=1;j<=cnt&&i<=maxn/p[j];j++){
            vis[i*p[j]]=1;
            if(i%p[j]==0) break; //核心代码，如果i能整除这个质数则跳出循环
        }
    }
}
```
## 埃式筛
```c
int p[1000]; //p数组用来存储质数
void prime()
{
    vis[1]=1; //1不是质数
    for(int i=2;i<=n;i++){ //n是筛选的范围
        if(!vis[i]){
            p[++cnt]=i; //存储质数
            for(int j=2*i;j<=n;j+=i){  //这里可以有一个优化，j=2*i
                vis[j]=1; //标记质数的倍数
            }
        }
    }
}
```
## 数学公式
* 海伦公式
S=sqrt(p * (p-a) * (p-b) * (p-c))
## 快速幂
<font color="red">
非递归版本
</font>

```
int p(int a,int b){
  int t,y;
  t=1; y=a;
  while (b!=0){
    if (b&1==1) t=t*y%MOD;
    y=y*y%MOD;
    b=b>>1;
  }
  return t;
}
```
<font color="red">
递归版本
</font>

```c
ll pow(ll a,ll i){
  if (i==0) return 1;
  ll temp=pow(a,i>>1)%MOD;
  temp=temp*temp%MOD;
  if (i&1) temp=temp*a%MOD;
  return temp%MOD;
}
```
## 龟速乘(快速幂BUG)
```c
ll gsc(ll a,ll b){
	ll ans=0;
	while(b){
		if(b&1) ans=(ans+a)%MOD;
		a=a*2%MOD;
		b>>=1;
	}
	return ans;
}
```
## 快速乘(有误差)

```c
cin>>a>>b>>mod;
cout<<((a*b-(long long)((long double)a*b/mod)*mod+mod)%mod);
```



## 树状数组模板

```c
int lowbit(int n){
    return n&-n;
}

void add(int x,int y,int n){
    for(int i=x;i<=n;i+=lowbit(i))
        c[i] += y;
}

int getsum(int x){
    int ans = 0;
    for(int i=x;i;i-=lowbit(i))
        ans += c[i];
    return ans;
}
```
## 查并集
```c
void init(int n) {
    for(int i = 1; i <= n; i++) {
        f[i] = i;
    }
}

int getFriend(int v) {  
    if(f[v] == v) {
        return v;
    }
    return f[v] = getFriend(f[v]);
}

void merge(int a, int b) {
    int t1 = getFriend(a);
    int t2 = getFriend(b);
    if(t1 != t2) {  
        f[t2] = t1;
    }
}
```
## 快速排序
```c
//左右都是闭区间 
int qsort(int l,int r,int *v){
	int mid=(l+r)>>1;
	int i=l,j=r;
	int temp=v[mid];//基准值是会被交换的，必须备份 
	while(i<=j){//这里小于等于都行 
		while(v[j]>temp) j--;//找到第一个小于等于基准值的 
		while(v[i]<temp) i++;
		if(i<=j){ //这里必须是小于等于,因为当只有两个数时，两个指针一定会重合 
			swap(v[i],v[j]);
			i++; j--;//这里必须，因为当交换的两个数有基准值时，若没有这一步，两个指针必然重合，然后会无限重复 
		}
	}
	//i一定比j大，排序过后i左面都小于基准值，j右面都大于基准值就找到了基准值的位置  
	if(i<r) qsort(i,r,v);//再排基准值右面 
	if(j>l) qsort(l,j,v);//再排基准值左面 
 }
```
## 组合数打表(杨辉三角)
<font color="red">
int类型最多到33层，34层开始爆int，ll最多50层左右，打表最多1e3的量
</font>

```c
int c[50][50];
int zuhe(){//下标从0开始
	c[0][0]=1;
	c[1][0]=1;c[1][1]=1;
	for(int i=2;i<=33;i++){
		c[i][0]=1;
		for(int j=1;j<=i;j++){
			c[i][j]=c[i-1][j-1]+c[i-1][j];
		}
	}
}
```
>持续更新中……
