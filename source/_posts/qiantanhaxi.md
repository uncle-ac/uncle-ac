---
title: 浅谈哈希
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
comments: true
photos: 'https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/74.webp'
date: 2020-08-16 21:14:19
authorLink:
categories: 算法
tags: 集训
keywords:
description: 快速比较字符串是否相等的一种算法
---

> **前言**
>
> 趁着还没忘记录一下吧🐷

## 哈希能干些啥？

学一个新算法首先一定要知道学这个能干些啥对吧，我们是为了用某个东西而去学这个东西而不是盲目目的的学，现在假如给你两段字符串让你去比较他们是否相同，如果暴力做法就是从头到尾扫一遍，都相同则相同，复杂度为O(N)，假如数据量非常大，而且字符串长度很大，现在题目就变成了给你n个字符串，现在又给你t个字符串问你每一个字符串是否在这n个字符串中，平常做法时间复杂度O(t*t个字符串每一个字符串长度\*n)，若用哈希预处理时间复杂度降到O(t\*字符串长度\+n)，也就是把那n个字符串预先处理一下，使得接下来比较每一个字符串时时间复杂度变成O(1)，让我们来看一下具体怎么操作吧

## 思想

其实哈希和二进制思想是类似的，如果让你比较两个二进制串是否相同，你肯定会想到把二进制转成十进制比较起来会更方便，因为复杂度降低到了O(1)，不用你去逐位比较了，那字符串也是可以像二进制那样转换成为一个数字的，例如：abcd这个字符串可以转换成为a*p^3^+b\*p^2^+c\*p^1^+d\*p^0^，这里的p值是多少先不说，每一个字符都是有acall值的，我们可以直接利用这一点把每一个字符串转换成这样的数字，其中如果涉及到字符串子串问题，例如找一个字符串中是否含有某一段子串，我们就可用前缀的思想讲一个字符及之前的所有字符转换成数字存到数组里面，例如abcd我们就开一个Hash[5]，Hash[1]存a的哈希值，Hash[2]存ab的哈希值，Hash[3]存abc的哈希值，以此类推，这样假如我们要找其中一段子串的哈希值就可以前缀减一下也在O(1)的时间得出来，但是注意的是这里每一位权值不同不能简单做一下减法，具体实现继续看

## 实现

这段代码就实现了哈希

```c
Hash[0]=0;
for(int i=1;i<=n;i++) Hash[i]=(Hash[i-1]*p+s[i])%mod;//前i 个字符的 hash值
```

是不是很简单呢？注意的是字符串转换的哈希值是特别特别大的，你想一想64位二进制都到了longlon的程度这里的p还比2大也就是说字符串不到64位都存不下了，因此但总归每一个字符串映射过来的数字基本都是不同的，因此我们可以取一下模，取模后数字依旧是不同的，需要注意的是虽然数字相同概率非常小，但是假如p和mod取得数字不对依旧是有很大几率两段不同字符串映射到相同数字的，因此这里的p和mod都应该是一个质数，以降低重复概率，，`p 一般取 131、13331 或者 2333，mod 一般取 1e9+7，1e9+9 或者 2^64^`。需要说明的是C语言的unsigned long long 无符号整数当存储数字超过范围后会自动取模，不会爆，所以hash数组可以用ull定义，只不过有的题目可能会卡内存，因此还是建议自己实现取模操作

到这里就实现了把字符串转换成为数字，接下来如何取子串的哈希值呢？

Hash[r]=s[1]\*p^r-1^+s[2]\*p^r-2^...s[r]*p^0^

Hash[l-1]=s[1]\*p^l^+s[2]\*p^l-1^...s[r]*p^0^

Hash[l~r]=s[l]*p^r-l^+s[l+1]\*p^r-l+1^...s[r]\*p^0^

发现3式等于1式减去2式*p^r-l+1^，因此取字串哈希值代码如下：

```c
void get_hash(){
	return ((hash[r]-hash[l-1]*pow(p,r-l+1))%mod+mod)%mod;
}
```

其中的pow函数当有多组数据时可以用po数组预先处理一下，存储p的i次方，这里的复杂度就降低为O(1)了，本来pow函数复杂度为O(N)的

po数组代码

```c
po[i-1]=1;
for(int i=1;i<=n;i++) po[i]=po[i-1]*p%MOD;
```

这样整个就结束了

[来看一道例题吧](https://vjudge.net/contest/388681#problem/A)

甚至可以直接看代码，知道实现的大致思路

## AC代码

```c
#include <stdio.h>
#include <iostream>
#include <string>
#include <string.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define maxn 500005
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int MAXN=400100;
ull ha[MAXN],po[MAXN];
int ans[MAXN];
char ch[MAXN];
int p=131;
ull get_ha(int l,int r){
	return ha[r]-ha[l-1]*po[r-l+1];
}
int main(){
	ios;
	while(cin>>ch+1){
		int len=strlen(ch+1);
		ha[0]=0; po[0]=1;
		for(int i=1;i<=len;i++){
			ha[i]=ha[i-1]*p+ch[i]-'a'+1;
			po[i]=po[i-1]*p;
		}
		int tail =0;
		for(int i=1;i<=len;i++){
			if(get_ha(1,i)==get_ha(len-i+1,len)) ans[++tail]=i;
		}
		for(int i=1;i<=tail;i++){
			if(i!=tail) cout<<ans[i]<<' ';
			else cout<<ans[i]<<'\n';
		}
	}
	return 0;
}
```

哈希还可以后缀，利用后缀和前缀比较可以快速判断回文串

[例题](https://vjudge.net/contest/388813#problem/D)

## CODE

```c
#include <stdio.h>
#include <iostream>
#include <string>
#include <string.h>
#define ios ios::sync_with_stdio(0); cin.tie(0); cout.tie(0)
#define maxn 500005
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int MAXN=100100;
ull ha1[MAXN],ha2[MAXN],po[MAXN];
char ch[MAXN];
int p=131;
ull get_h1(int l,int r){
	return ha1[r]-ha1[l-1]*po[r-l+1];
}
ull get_h2(int l,int r){
	return ha2[l]-ha2[r+1]*po[r-l+1];
}
int main(){
	ios;
	po[0]=1;
	for(int i=1;i<MAXN;i++) po[i]=p*po[i-1];
	while(cin>>ch+1){
		int len=strlen(ch+1);
		ha1[0]=ha2[len+1]=0;
		for(int i=1;i<=len;i++)
			ha1[i]=ha1[i-1]*p+ch[i];
		for(int i=len;i>=1;i--)
			ha2[i]=ha2[i+1]*p+ch[i];
		int pos;
		for(int i=1;i<=len;i++){
			if(get_h1(i,len)==get_h2(i,len)){
				pos=i-1;
				break;
			}
		}
		cout<<ch+1;
		for(int i=pos;i>=1;i--) cout<<ch[i];
		cout<<'\n';
	}
	return 0;
}
```

## 双哈希乃至多哈希

需要明白的一点是虽然错误率是极低的，但是依旧有错误率的，为了进一步降低错误率，我们可以用双哈希乃至多哈希降低错误率，也就是对一个字符串定义两个hash数组，分别对不同的p和mod操作，当两个字符串比较时，除非两个hash数组都一样才说两个字符串相同，[例题](https://vjudge.net/contest/388813#problem/F)

```c
#include <iostream>
#include <cstdio>
using namespace std;

typedef unsigned long long LL;
#define base 33
LL p[1000100];
LL h1[1000100],h2[1000100];

inline LL gethash1(int l,int r) {
    LL tp = l?h1[l-1]:0;
    return h1[r] - tp*p[r-l+1];
}

inline LL gethash2(int l,int r) {
    LL tp = l?h2[l-1]:0;
    return h2[r] - tp*p[r-l+1];
}

bool Is_palindrome(string s) {
    for (int i = 0; i < s.size()/2; i++)
        if (s[i] != s[s.size()-1-i]) return 0;
    return 1;
}

string find(string s) {
    string ans = "";
    int len = s.size();
    if (len == 0) return ans;
    for (int i = 0; i < len; i++) {
       if (i == 0) h1[i] = s[i];
       else h1[i] = h1[i-1]*base+s[i];
    }
    for (int i = 0; i < len; i++) {
        if (i == 0) h2[i] = s[len-i-1];
        else h2[i] = h2[i-1]*base+s[len-i-1];
    }
    for (int i = len-1; i >= 0; i--) {
        if (gethash1(0,i) == gethash2(len-1-i,len-1) && Is_palindrome(s.substr(0,i+1))) return s.substr(0,i+1);
    }
    return ans;
}
void work() {
    string s;
    cin >> s;
    string comm = "";
    int i;
    for (i = 0; i < s.size()/2; i++)
      if (s[i] == s[s.size()-1-i]) comm += s[i];
      else break;
    s = s.substr(i,s.size()-i-i);
    string ss = "";
    for (int i = s.size()-1; i>=0; i--) ss += s[i];
    string s1 = find(s),s2 = find(ss);
    cout << comm  << (s1.size()>s2.size()?s1:s2);
    for (int i = comm.size()-1; i >= 0; i--)
      cout << comm[i];
    puts("");
}
int main() {
    p[0] = 1;
    for (int i = 1; i <= 1000000; i++)
       p[i] = p[i-1]*base;
    int n;
    cin >> n;
    while (n--) {
        work();
    }
    return 0;
}

```

