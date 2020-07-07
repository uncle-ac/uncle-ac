---
title: 基础算法练习
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 题目
comments: true
date: 2020-05-17 17:30:44
tags: 基础算法练习
keywords:
description: 基础练习题
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/47.webp
---
## A 前M大的数
暴力累加每两组数，再排序输出前M个
```
#include<cstdio>
#include<set>
#include<iostream>
#include<algorithm>
#define ios ios::sync_with_stdio(0);cin.tie(0);cout.tie(0) 
using namespace std;
const int MAXN=3e3+100;
int v1[MAXN],v2[5000000];
int main()
{
//	ios;
    int n,m;
	while(scanf("%d %d",&n,&m)!=EOF){
		for(int i=1;i<=n;i++) cin>>v1[i];
		int tail=0;
		for(int i=1;i<n;i++){
			for(int j=i+1;j<=n;j++){
				v2[++tail]=v1[i]+v1[j];
			}
		}
		sort(v2+1,v2+1+tail);
		int len=tail-m;
		for(int i=tail;i>len;i--){
			if(i!=len+1) cout<<v2[i]<<" ";
			else cout<<v2[i]<<'\n';
		}
	} 
    return 0;
}
```
## B 稳定排序
注意sort不是稳定的，用stable_sort排再一一比较
```
#include<cstdio>
#include<set>
#include<iostream>
#include<algorithm>
#include<string>
#include<string.h>
#define ios ios::sync_with_stdio(0);cin.tie(0);cout.tie(0) 
using namespace std;
const int MAXN=3e3+100;
struct node{
	int score;
	string name;
}stu[400],stu2[400];
bool cmp(node a,node b){
	return a.score>b.score;
}
int main()
{
//	ios;
    int n;
	while(scanf("%d",&n)!=EOF){
		for(int i=1;i<=n;i++) cin>>stu[i].name>>stu[i].score;
		for(int i=1;i<=n;i++) cin>>stu2[i].name>>stu2[i].score;
		stable_sort(stu+1,stu+1+n,cmp);
		int flag=0,book=0;
		for(int i=1;i<=n;i++){
			if(stu[i].score!=stu2[i].score){
				flag=1;
				break;
			}
		}
		for(int i=1;i<=n;i++){
			if(stu[i].name!=stu2[i].name){
				book=1;
				break;
			} 
		} 
		if(flag==1){
			cout<<"Error"<<endl;
			for(int i=1;i<=n;i++){
				cout<<stu[i].name<<" "<<stu[i].score<<endl;
			}
		}
		else{
			if(book==1){
				cout<<"Not Stable"<<endl;
				for(int i=1;i<=n;i++){
					cout<<stu[i].name<<" "<<stu[i].score<<endl;
				}
			}else cout<<"Right"<<endl;
		}
	} 
    return 0;
}
```
## C 开门人和关门人
技巧在于时间可以用字符串来代替，因为时间长度都是一样的
```
#include<cstdio>
#include<set>
#include<iostream>
#include<algorithm>
#include<string>
#include<string.h>
#define ios ios::sync_with_stdio(0);cin.tie(0);cout.tie(0) 
using namespace std;
const int MAXN=3e3+100;
struct node{
	string name,qt,ht;
}stu[4000];
bool cmp(node a,node b){
	return a.qt<b.qt;
}
bool cmp1(node a,node b){
	return a.ht>b.ht;
}
int main()
{
//	ios;
    int n;
	cin>>n;
	while(n--){
		int m; cin>>m;
		for(int i=1;i<=m;i++){
			cin>>stu[i].name>>stu[i].qt>>stu[i].ht;
		} 
		sort(stu+1,stu+1+m,cmp);
		cout<<stu[1].name<<" ";
		sort(stu+1,stu+1+m,cmp1);
		cout<<stu[1].name<<endl;
	}
    return 0;
}
```
## D EXCEL排序
注意字典序排序不是长的字符串大于短的字符串，而是一个个比较，遇到第一个不同的那个大那个字符串就大，当每个字符都一样则谁长谁大，strcmp也是如此
```
#include<iostream>
#include<stdio.h>
#include<string>
#include<string.h>
#include<algorithm>
#include<map>
using namespace std;
const int MAXN=1e5+100;
struct node{
	string name,id;
	int score;
}stu[MAXN]; 
bool cmp(node a,node b){
	return a.id<b.id;
}
bool cmp1(node a,node b){
	if(a.name==b.name) return a.id<b.id;
	return a.name<b.name;
}
bool cmp2(node a,node b){
	if(a.score==b.score) return a.id<b.id;
	return a.score<b.score;
}
int main()
{
	int n,c,kase=0;
	while(cin>>n>>c){
		if(n==0) break;
		for(int i=0;i<n;i++){
			stu[i].id.clear();
			stu[i].name.clear();
		}
		for(int i=0;i<n;i++){
			cin>>stu[i].id>>stu[i].name>>stu[i].score;
		}
		if(c==1) sort(stu,stu+n,cmp);
		if(c==2) sort(stu,stu+n,cmp1);
		if(c==3) sort(stu,stu+n,cmp2);
		printf("Case %d:\n",++kase);
		for(int i=0;i<n;i++){
			cout<<stu[i].id<<" "<<stu[i].name<<" "<<stu[i].score<<endl;
		}
	}
	return 0;
}
```
## E - {A} + {B}
用一个set来存，或者用桶标记,或者用unique去重
```
#include<cstdio>
#include<set>
#include<iostream>
#include<algorithm>
#include<string>
#include<string.h>
#define ios ios::sync_with_stdio(0);cin.tie(0);cout.tie(0) 
using namespace std;
const int MAXN=1e5+100;
int main()
{
//	ios;
	int n,m;
    while(cin>>n>>m){
    	set<int> st;
    	for(int i=0;i<n+m;i++){
    		int t; cin>>t;
    		st.insert(t);
		}
		cout<<*st.begin();
		st.erase(st.begin());
		while(!st.empty()){
			cout<<" "<<*st.begin(); 
			st.erase((st.begin()));
		}
		cout<<endl;
	}
    
    return 0;
}
```
## F - 水果
因为这道题涉及到求和的问题，直接用数组来做的话我觉得非常难输出，用map挺简单的，就是不熟练
```
#include<iostream>
#include<stdio.h>
#include<string>
#include<algorithm>
#include<map>
using namespace std;
struct node{
	map<string,int> mp;
}; 
int main()
{
	map<string,node> ma;
	map<string,node>::iterator it;
	map<string,int>::iterator mpit;
	string f,p;
	int cnt;
	int n,t;
	cin>>t;
	while(t--){
		ma.clear();
		cin>>n;
		while(n--){
			cin>>f>>p>>cnt;
			ma[p].mp[f]+=cnt;
		}
		for(it=ma.begin();it!=ma.end();it++){
			cout<<it->first<<endl;
			for(mpit=it->second.mp.begin();mpit!=it->second.mp.end();mpit++){
				cout<<"   |----"<<mpit->first<<"("<<mpit->second<<")"<<endl;
			}
		}
		if(t!=0) cout<<endl;
	}
	return 0;
}
```
## G - 不重复数字
用桶标记或者用map性质一样，都是桶标记的思想
```
#include<cstdio>
#include<set>
#include<iostream>
#include<algorithm>
#include<string>
#include<string.h>
#include<map>
#define ios ios::sync_with_stdio(0);cin.tie(0);cout.tie(0) 
using namespace std;
const int MAXN=1e5+100;
int val[MAXN];
int main()
{
//	ios;
	int t;
	cin>>t;
    while(t--){
    	map<int,int> st;
    	int n,maxx=-1e9,tail=0;
    	cin>>n;
    	for(int i=1;i<=n;i++){
    		int tt; cin>>tt;
    		maxx=max(maxx,tt);
    		val[++tail]=tt;
    		st[tt]=1;
		}
		cout<<val[1];
		st[val[1]]=0;
    	for(int i=2;i<=tail;i++){
    		if(st[val[i]]){
    			cout<<' '<<val[i];
    			st[val[i]]=0;
			}
		}
		cout<<endl;
	}
    
    return 0;
}
```
## H - 表达式括号匹配
用栈遇到一个左括号就入栈，遇到一个右括号如果栈里有元素就出栈，最后看看栈是否为空
昨天代码写的有问题，有一点xiao bug，改了一下，这没问题了
```
#include<bits/stdc++.h>
using namespace std;
int main()
{
	string sh;
	cin>>sh;
	stack<char> st;
	for(int i=0;i<sh.size();i++){
		if(sh[i]=='(') st.push(sh[i]);
		if(sh[i]==')'){
			if(st.empty()){
				cout<<"NO"<<endl;
				return 0;
			}else{
				st.pop();
			}
		}
	}
	if(!st.empty()) cout<<"NO"<<endl;
	else cout<<"YES"<<endl;
	
	return 0;
}
```
## I - 合并果子
每次合并最小的两个代价最小，每次排序合并，直到最后剩一堆
```
#include<iostream>
#include<stdio.h>
#include<algorithm>
using namespace std;
int a[1001];
int main()
{
	int t;
	cin>>t;
	while(t--){
		int n;
		cin>>n;
		for(int i=0;i<n;i++) cin>>a[i];
		sort(a,a+n);
		long long sum=0;
		int head=0;
		while(head+1<n){
			sum+=a[head]+a[head+1];
			a[head+1]+=a[head];
			if(a[head+1]>a[head+2]) sort(a+head+1,a+n);
			head++;
		}
		cout<<sum<<endl;
	}
}
```
## J - Covered Points Count
这道题用了离散化和差分，我这里还不熟，俩星期前我专门做了几道这种题，当时明白了，过了俩星期又忘了，现在还有些迷
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
using namespace std;
typedef long long ll;
const int maxn=2e5+100;
struct node{
	ll x,y;
}p[maxn];
ll a[maxn<<1],b[maxn<<1],c[maxn];
int main()
{
	ios::sync_with_stdio(false);
	int n,tail=0; cin>>n;
	for(int i=1;i<=n;i++){
		cin>>p[i].x>>p[i].y;
		a[++tail]=p[i].x;
		a[++tail]=p[i].y+1; 
	}
	sort(a+1,a+1+tail);
	int len=unique(a+1,a+1+tail)-a-1;
	for(int i=1;i<=n;i++){
		int x=lower_bound(a+1,a+1+len,p[i].x)-a;
		int y=lower_bound(a+1,a+1+len,p[i].y+1)-a;
		b[x]++;b[y]--;
	}
	for(int i=1;i<=len;i++){
		b[i]+=b[i-1];
		c[b[i]]+=a[i+1]-a[i];
	}
	for(int i=1;i<=n;i++) printf("%lld%c",c[i],i==n?'\n':' ');
}
```
## K - Ignatius and the Princess IV
非常水的题
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<map>
using namespace std;
typedef long long ll;
const int maxn=2e5+100;
int main()
{
	int n;
	while(cin>>n){
		map<int,int> mp;
		int temp=n;
		while(n--){
			int t; cin>>t;
			mp[t]++;
		}
		map<int,int>::iterator it;
		for(it=mp.begin();it!=mp.end();it++){
			if(it->second>=(temp+1)/2){
				cout<<it->first<<endl;
				break;
			}
		}
	}
	return 0;
}
```
## L - Stones
这道题用优先队列模拟，主要是排列方式如何自定义，这个我今天才学，只会个基础
```
#include<iostream>
#include<algorithm>
#include<set>
#include<queue>
using namespace std;
typedef long long ll;
typedef pair<int,int> pr;
struct node{
	int pos,dis;
	friend bool operator <(const node a,const node b){
		if(a.pos==b.pos) return a.dis>b.dis;
		return a.pos>b.pos;
	}
};

int main()
{
	int t;
	scanf("%d",&t);
	while(t--){
		priority_queue<node> oq;
		int n; 
		scanf("%d",&n);
		for(int i=0;i<n;i++){
			node r;
			scanf("%d %d",&r.pos,&r.dis);
			oq.push(r);
		}
		int cnt=1;
		node y;
		while(!oq.empty()){
			node x=oq.top();
			oq.pop();
			if(cnt&1){
				y.pos=x.pos+x.dis;;
				y.dis=x.dis;
				oq.push(y);
			}
			cnt++;
		}
		printf("%d\n",y.pos);
	}
    return 0;
}
```
## M - SnowWolf's Wine Shop
做了无数遍的题
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<set>
using namespace std;
typedef long long ll;
const int maxn=2e5+100;
multiset<int> mt;

int main()
{
	int t; cin>>t; int kase=0;
	while(t--){
		mt.clear();
		printf("Case %d:\n",++kase);
		int n,q,y;
		cin>>n>>q>>y;
		for(int i=0;i<n;i++){
			int tt; cin>>tt;
			mt.insert(tt);
		}
		while(q--){
			int v; cin>>v;
			auto it=mt.lower_bound(v);
			if(it==mt.end()||*it-y>v) printf("-1\n");
			else{
				printf("%d\n",*it);
				mt.erase(it);
			}
		}
	}
	return 0;
}
```
## N - Alice, Bob and Candies
双指针模拟，以前不自信，总是不敢用STL，现在发现所谓STL也不过如此，加上我两分钟写好的双向队列代码🐷
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
deque<int> dq;
int main()
{	
	int t;
	cin>>t;
	while(t--){
		dq.clear();
		int n; cin>>n;
		for(int i=1;i<=n;i++){
			int temp; cin>>temp;
			dq.push_back(temp);
		}
		ll pa=0,pb=0,suma=0,sumb=0,res=1,ansa=0,ansb=0,cnt=0;
		while(!dq.empty()){
			cnt++;
			suma=sumb=0;
			if(res&1){
				while(suma<=pb&&!dq.empty()){
					suma+=dq.front();
					dq.pop_front();
				}
				ansa+=suma; 
				pa=suma;
			}else{
				while(sumb<=pa&&!dq.empty()){
					sumb+=dq.back();
					dq.pop_back();
				}
				ansb+=sumb;
				pb=sumb;
			}
			res++;
		}
		cout<<cnt<<" "<<ansa<<" "<<ansb<<endl;
	}
	return 0;
}
```
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4;
int val[MAXN];
int main()
{
	ios;
	int t; cin>>t;
	while(t--){
		int n,pa=0,pb=0,flg=0;; cin>>n;
		int sum=0;
		for(int i=1;i<=n;i++){
			cin>>val[i];
			sum+=val[i];
		}
		int ea=0,eb=0,cnt=1,sa,sb,head=1,tail=n;
		while(tail-head>=0){
			sa=0; sb=0;
			if(flg==0&&sum-ea-eb>pb){
				while(sa<=pb){
					sa+=val[head];
					ea+=val[head];
					head++; 
				}
				pa=sa; flg=1; cnt++;
				continue;
			}
			if(flg==1&&sum-ea-eb>pa){
				while(sb<=pa){
					sb+=val[tail];
					eb+=val[tail];
					tail--;
				}
				pb=sb; flg=0; cnt++;
				continue;
			}
			cnt++;
			if(flg==0){
				ea+=sum-ea-eb;
				break;
			}else{
				eb+=sum-ea-eb;
				break;
			}
		}
		cout<<cnt-1<<" "<<ea<<" "<<eb<<" "<<endl;
	} 
	return 0;
}
```
## O - Special Elements
div4的那道桶标记，用前缀和把复杂度将为O(1)，就做出来了
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e4;
int val[MAXN],book[MAXN];
int main()
{
	ios;
	int t; cin>>t;
	while(t--){
		memset(book,0,sizeof book);
		memset(val,0,sizeof val);
		int n; cin>>n;
		for(int i=1;i<=n;i++){
			cin>>val[i];
			book[val[i]]++;
			val[i]+=val[i-1];
		}
		int cnt=0;
		for(int i=0;i<n-1;i++){
			for(int j=i+2;j<=n;j++){
				int sum=val[j]-val[i];
//				cout<<sum<<' '<<book[sum]<<endl;
				if(sum<=n&&book[sum]){
					cnt+=book[sum]; 					
					book[sum]=0;
				}
			}
		}
		cout<<cnt<<endl;
	} 
	return 0;
}
```
## P - Max Sum
这道题有点dp的味道，我觉得有点难，反正我没做出来，查了以前的代码才懂了，不过说实话这思路稍微把题目变一下我不知道还能做出来不能，这道题很坑的是用C++可以过，G++不行
```
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>
#define ios ios::sync_with_stdio(0)
using namespace std;
const int MAXN=1e5+100;
int val[MAXN];
int main()
{
	ios;
	int t,kase=0; cin>>t;
	while(t--){
		memset(val,0,sizeof val);
		int n; cin>>n;
		for(int i=1;i<=n;i++) cin>>val[i];
		int summax=-1e9,b=0,e=0,k=1,sum=0; 
		for(int i=1;i<=n;i++){
			sum+=val[i];
			if(sum>summax){
				b=k;
				e=i;
				summax=sum;
			}
			if(sum<0){
				sum=0;
				k=i+1;
			}
		}
		printf("Case %d:\n",++kase);
		cout<<summax<<" "<<b<<" "<<e<<endl;
//		printf("Case %d:\n%d %d %d\n",++kase,summax,b,e);
		if(t!=0) printf("\n");
//		if(t!=0) cout<<endl; 
	} 
	return 0;
}
```
## 总结
以后尽量都用scanf和printf吧，这两个最稳定，不会报稀奇古怪的错误，然后记得字典序比较不是长的大于短的，记得字典序的排列方式，当用容器时一定记得判断容器中有没有元素，有了再pop，sort是不稳定排序，cin和cout配套,scanf和printf配套，两者不要混用