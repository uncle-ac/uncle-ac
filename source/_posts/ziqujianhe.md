---
title: 所有子区间最值求和
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/57.webp
date: 2020-06-04 12:02:18
tags: 数据结构
keywords:
description: 求区间子区间最值和问题
---
>这道题用单调栈解决了一个区间的所有子区间最值之和的问题，以后如果碰到类试问题可以直接套用这个模板的

## 题目
![题目](https://img-blog.csdnimg.cn/20200604120946815.png)
![题目](https://img-blog.csdnimg.cn/20200604120946818.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70)
![题目](https://img-blog.csdnimg.cn/20200604120946793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70)

### 题解
不是我不想写，因为我觉得自己写的没有人家好😂单纯记录一下
[一篇很详细的题解](https://blog.csdn.net/Code92007/article/details/83689045?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
如果你看完了这篇题解，我就强调一点吧，当往左扩展或者网友扩展其中一个一定不能有等于号，因为如果数据中有两个或多个值相同时，如果你都写成了大于等于，那么算出的区间就是以该值为最大值的严格意义上的那段区间，比如：
6 5 5 4 8
如果你都写成了大于等于，那么数据中两个5对应的左右端点就都是：
2 4
这样没毛病，再[2,4]上确实5是最大值，但是当你算子区间最值之和时就会重复，以第一个5为最值的子区间有：[2,2]、[2,3]、[2,4]，而算第二个5时：[2,3]、[3,4]、[2,4]、[3,3]，很明显[2,3]、[2,4]这俩区间重复了，我们应该把它们减去，如何减去呢？其实只要把大于等于改成大于就行了，这样5的右端点就拓展到第一个大于等于5的前一个位置，也就是第一个5区间变成了[2,2]，就完成了去重
### CODE
```
#include<iostream>
#include<cstdio>
#include<set>
#include<cmath>
#include<cstring>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<stack>
#include<algorithm>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
typedef long long ll;
using namespace std;
const int MAXN=1e5+100;
ll l[MAXN],r[MAXN],a[MAXN]; 
ll fun(ll n){
	stack<ll> st1,st2;
	memset(l,0,sizeof l);
	memset(r,0,sizeof r);
	for(ll i=1;i<=n;i++){
		while(st1.size()&&a[i]>=a[st1.top()]){
			st1.pop();
		}
		if(st1.empty()) l[i]=1;
		else l[i]=st1.top()+1;
		st1.push(i);
	}
	for(ll i=n;i;i--){
		while(st2.size()&&a[i]>a[st2.top()]){ //一定注意这里是大于号
			st2.pop();
		}
		if(st2.empty()) r[i]=n;
		else r[i]=st2.top()-1;
		st2.push(i);
	}
	ll ans=0;
	for(ll i=1;i<=n;i++){
		ans+=a[i]*((r[i]-l[i])+(i-l[i])*(r[i]-i));
	}
	return ans;
}
int main()
{
	ios;
	ll t;
	cin>>t;
	while(t--){
		ll n; cin>>n;
		for(ll i=1;i<=n;i++) cin>>a[i];
		ll ans=0;
		ans+=fun(n);
		for(ll i=1;i<=n;i++) a[i]=-a[i];
		ans+=fun(n);
		cout<<ans<<'\n';
	}
	return 0;
}
```