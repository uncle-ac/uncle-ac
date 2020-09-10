---
title: 积分赛5
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/72.webp'
date: 2020-08-15 22:19:23
authorLink:
categories: 题目
tags: 集训
keywords:
description: 最后一次个人积分赛了，集训进入尾声，快要开学了
---

> ~~最友好的一次积分赛2333~~应该也是最后一个个人积分赛了，不知道最后一次会不会是团队赛，再说吧，已经八月十五了，自从集训以来每天都有训练，虽然晚上一般都是有些时间去记录一些题解的，但是懒癌晚期的我真的不想去写，终于今天下定决心，趁着这两天我要补一下题解，把最近有意义的题目记录一下，也算是一个复习吧🎉

## D 何不好的晨练

**描述**

何不好是一名晨练爱好者，每天他都要一大早出门进行晨跑锻炼，何不好生活的城市很大，

同时也比较繁荣，路边高楼耸立，道路上车辆往来，川流不息。

晨练的同时还可以欣赏路边的风景，于是，何不好打算每天选择 *M* 条路段，其中包括 *N*

个路口。

但何不好想要从起点开始不重复的跑完所有路段，并且跑回起点，苦于是学渣的他不知道

能不能完成这个条件，于是他向你寻求帮助。

**输入数据**

第一行给定两个整数 *N* , *M* 。

接下来 *M* 行，每行有两个整数 *U*，*V* 代表路段相连的两个路口。

1 *≤* *N* *≤* 100,000. 0 *≤* *M* *≤* 100,000. 毎对儿 *U*, *V* 互不相同，且 *U*, *V* 不同。

**输出数据**

若能完成这个条件则输出Yes，否则输出No。

**样例输入**

4 4

1 2

2 3

3 4

4 1

**样例输出**

Yes

---

不难的一道题，只要知道了一笔画问题就差不多能解决了，首先看两个概念

1. 欧拉通路：即可以不回到起点，但是必须经过每一条边，且只能一次。也叫"一笔画"问题。该路径成为欧拉路径。
2. 图G的一个回路，如果恰通过图G的每一条边，则该回路称为欧拉回路，具有欧拉回路的图称为欧拉图。欧拉图就是从图上的一点出发，经过所有边且只能经过一次，最终回到起点的路径。

**判定充要条件**

欧拉回路：    1:  图G是连通的，不能有孤立点存在。

　　　　　　2:  `对于无向图来说度数为奇数的点个数为0;  对于有向图来说每个点的入度必须等于出度`。

欧拉通路：    1:  图G是连通的，无孤立点存在。

　　　　　　2:  对于无向图来说，`度数为奇数的的点可以有2个或者0个，并且这两个奇点其中一个为起点另外一个为终点`。  对于有向图来说，可以`存在两个点，其入度不等于出度，其中一个出度比入度大1，为路径的起点；另外一个入度比出度大1，为路径的终点`。

知道了这个这个题目就只要判断`图是否联通+图上奇数度数点是否存在`，度数容易判断，图是否联通则可以用并查集来做，开一个son数组代表该结点作为根节点下方有多少点，合并时更新即可

```c
#include <iostream>
#include <cstring>
#include <cstdio>
#include <map>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
using namespace std;
const int MAXN=1e5+100;
int fa[MAXN],son[MAXN];
map<int,int> mp;
void init(){
    for(int i=0;i<MAXN;i++){ //初始化
    	fa[i]=i; son[i]=1;
    }
}
int find(int x){
	if(fa[x]==x) return x;
    return fa[x]=find(fa[x]);
}
void Union(int x,int y){
    x=find(x);
    y=find(y);
    if(x!=y){
        fa[x]=y;
        son[y]+=son[x]; //右面结点的son数量加上左面结点的son数量
    }
}
int main()
{
	ios;
    int n,m,a,b;
    cin>>n>>m;
    init();
    for(int i=0;i<m;i++){
        cin>>a>>b;
        mp[a]++,mp[b]++;
        Union(a,b);
    }
    bool f1=0,f2=0;
    for(auto p:mp){
        if(p.second&1) f1=1; //判奇数结点
        if(son[find(p.first)]!=n) f2=1; //若图联通则任意一个结点的祖先的son值一定等于所有节点的数量
    }
    if(f1||f2) cout<<"No"<<'\n';
    else cout<<"Yes"<<'\n';
    return 0;
}
```

## E 何不好的博弈游戏

**描述**

何不好要和他的好朋友Tom玩一个游戏，游戏规则是有n个硬币，正面向上围成一个圆圈，

并且排上序号1到n，确定一个数字k，每次只能翻序号连续的1到k个硬币（n和1连续），每个硬

币只能翻一次，如果最后谁没有硬币可以翻(所有的硬币都被翻过一次了)，谁就输掉了这个游

戏，每次都是何不好先手，如果两个人都很聪明，并且没有失误的话，问最后何不好能不能赢

得这次游戏。

**输入数据**

第一行是一个整数*T*(*T* ⩽ 10)

接下来有T行，每一行有两个整数*n*(1 ⩽ *n* ⩽ 103)，*k*(1 ⩽ *k* ⩽ 103)。

**输出数据**

如果何不好能赢这次游戏，输出“Yes”，如果不能，输出“No”。

**样例输入**

3

3 1

5 2

5 6

**样例输出**

Yes

No

Yes

---

**思路**

博弈论个人理解就是智力题，这道题对不同情况分别讨论：

1. 如果 k<n:
   1. 当k=1时，如果n是奇数，先手一定能赢，n是偶数后手赢
   2. 当k>1时，后手一定赢（先手第一次翻什么位置，后手就翻对称的位置，使剩下的硬币分为两个相同长度的序列），然后无论先手接下来怎么翻，后手就翻对称的位置，这样保证后手永远有硬币可以翻

2. 如果k>=n:  后手赢，直接翻完就完事了

把思路直接写成代码就行了

```c
#include <bits/stdc ++.h>
using namespace std;
typedef long long ll;
int main ()
{
	int t;
	cin>>t;
	while(t--){
		int n,k;
		cin>>n>>k;
		int flag=0;
		if(k<n){
			if(k==1&&(n&1)) flag=1;
		}
		if(k>=n) flag=1;
		if(flag==1) cout <<"Yes"<<endl;
		else cout <<"No"<<endl;
	}
	return 0;
}
```

## I 何不好的字符串

**描述**

何不好非常不好，因为别人给他一个字符串，要让他解答问题，何不好懒得写就把他交给

你们了，何不好可以不回答，但你们必须回答，给你一个字符串*S*,先将字符串扩充一次，比

如*AB*变成*ABAB*,在把字符串中随意加入一个字母构成字符串*U*，让你重新构造出字符串*S*;

**输入数据**

第一行输入表示字符串*U*长度为*N*(*N* ⩽ 20000001)，第二行输入字符串*U*;

**输出数据**

输出字符串*S*, 如果符串无法按照上述方法构造出来，输出*NOT P OSSIBLE*; 如果字符串*S*不唯一，输出*NOT UNIQUE*;

**样例输入1**

7

ABXCABC

**样例输出1**

ABC

**样例输入2** 

6

ABCDEF

**样例输出3**

NOT POSSIBLE

---

**思路**

哈希预处理，使得字符串比较复杂度降低为O(1)，因此枚举每一个位置删除该位置后看看前半部分和后半部分是否相同，时间复杂度为O(n)，不过这个数据真的吓人2e7的字符串长度，计算机1s能算5e8的数据，能过的

现在给你两个字符串的哈希值，让你求两个字符串拼接后的哈希值，只需要让左面的乘以p^右面字符串长度^加上右面字符串哈希值就行了，这里卡了好久

```c
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <map>
using namespace std;
typedef unsigned long long ull;
const ull base=2333;
const ull N=20000101;
map<ull,ull> mp;
char s[N];
int n,len,cnt;
ull Hash[N],p[N];
ull gethash(int x,int y){
    if(x>y) return 0;
    return Hash[y]-Hash[x-1]*p[y-x+1];
}
bool check(int pos){
    ull hash1,hash2;
    if(pos<=n+1){
    	hash1=Hash[pos-1]*p[n+1-pos]+gethash(pos+1,n+1); //算出删除点左面哈希值和右面哈希值，然后把左面乘以相应权值然后加上右面就是拼接好的哈希值
    	hash2=gethash(n+2,len); //因为删除点在左半部分，因此右面直接算哈希
    }
    else{
    	hash1=Hash[n]; //删除点在右半部分，左面直接算
    	hash2=gethash(n+1,pos-1)*p[len-pos]+gethash(pos+1,len);
    }
    if(hash1==hash2){
        mp[hash1]++; //把新得到的字符串的哈希值存到map里
        if(mp[hash1]==1) cnt++; //若该字符串是第一次出现则计数
        return 1;
    }
    return 0;
}
int main()
{
    scanf("%d",&n);
    scanf("%s",s+1);
    len=n;
    if(n%2==0){ //原串字符数量是偶数时直接输出不可能
        puts("NOT POSSIBLE");
        return 0;
    }
    n/=2; //找到中点
    p[0]=1; //打一个每一位权值的表
    for(int i=1;i<=len;++i){ //打哈希表加权值表
    	p[i]=p[i-1]*base;
    	Hash[i]=Hash[i-1]*base+(s[i]-'A'+1);
    }
    int pos;
    for(int i=1;i<=len;++i){
    	if(check(i)) pos=i; //枚举每一个点，当删除该点后左右字符串哈希值一样则表示符合条件记录该位置
    }
    if(!cnt) puts("NOT POSSIBLE"); //枚举每一个点左右两边的哈希值都不一样就不可能
    else if(cnt>1) puts("NOT UNIQUE"); //当出现了两个解输出不唯一
    else{
        if(pos<=n+1) for(int i=n+2;i<=len;++i) printf("%c",s[i]); //删除的字符在左面就输出右面
        else for(int i=1;i<=n;++i) printf("%c",s[i]); //否则输出左面
    }
    return 0;
}
```

