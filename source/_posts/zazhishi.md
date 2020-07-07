---
title: 杂类知识&&STL模板
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 记录
comments: true
date: 2020-04-26 17:01:18
tags: 杂类知识&&STL
keywords:
description: 一些有用的杂类知识
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/0.webp
---
# 通用模板
```
    #include<bits/stdc++.h>
    #define PI acos(-1)
    #define ios ios::sync_with_stdio(false)
    #define debug freopen("in.txt","r",stdin); freopen("out.txt","w",stdout)
    #define fs first
    #define sc second
    using namespace std;
    typedef long long ll;
    typedef pair<int,int> pii;
    const int maxn = __;
```
# auto用法
auto 后面的变量必须初始化，根据初始化的值来判断其类型，可以是自定义的类型，比如结构体
{% fb_img https://img-blog.csdnimg.cn/20190511114519108.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTU3OTIz,size_16,color_FFFFFF,t_70 %}

{% fb_img https://img-blog.csdnimg.cn/20190511114533192. %}

**注**:使用auto定义变量时必须对其进行初始化，在编译阶段编译器需要根据初始化表达式来推导auto的实际类型。因此auto并非是一种“类型”的声明，而是一个类型声明时的“占位符”，编译器在编译期会将auto替换为变量实际的类型。

**auto迭代**:
for循环后的括号由冒号" : "分为两部分：第一部分是范围内用于迭代的变量，第二
部分则表示被迭代的范围
{% fb_img https://img-blog.csdnimg.cn/20190511115149404. %}

{% fb_img https://img-blog.csdnimg.cn/20190511115216208. %}

auto& 可以对数组 a 中的元素进行修改.

{% fb_img https://img-blog.csdnimg.cn/20190511115532318.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTU3OTIz,size_16,color_FFFFFF,t_70 %}
```
    //遍历字符串
    std::string str = “hello, world”;  
    for(auto ch : str) {  
         std::cout << ch << std::endl;  
    }  
    //遍历数组
    int arr[] = {1, 2, 3, 4};  
    for(auto i : arr) {  
         std::cout<< i << std::endl;  
    }  
```
{% fb_img https://img-blog.csdnimg.cn/20190511115555145. %}

**auto声明迭代器**

例如：

**注意：** 遍历map返回的是pair变量，不是迭代器。
```
    map<int,int> mp;
    for(auto it=mp.begin();it!=mp.end();it++){
      cout<<it->first<<" "<<it->second<<endl;
    }
//或者
    map<int,int> mp;
	mp.insert(pair<int,int>(1,100));
	mp.insert(pair<int,int>(2,200));
	mp.insert(pair<int,int>(3,300));
	for(auto it:mp){
		cout<<it.first<<" "<<it.second<<'\n';
	}
```
**auto特性:**
1. auto不能作为函数参数
2. auto不能直接声明数组
3. 为了避免与C++98中的auto发生混淆，C++11只保留了auto作为类型指示符的用法
4. auto在实际中最常见的优势用法就是跟以后会讲到的C++11提供的新式for循环，还有lambda表达式等进行配合使用。
5. auto不能定义类的非静态成员变量
6. 实例化模板时不能使用auto作为模板参数

# 加快cin cout的速度
使用:

cin，cout之所以效率低，是因为先把要输出的东西存入缓冲区，再输出，导致效率降低，而这段语句可以来打消iostream的输入 输出缓存，可以节省许多时间，使效率与scanf与printf相差无几

ios::sync_with_stdio(false)

cin.tie(0) 解除cin和cout的绑定

cout.tie(0)

# cout<<endl的锅
使用cout换行时当数据量很大时最好使用cout<<"\n"这个速度快一点点，吃过这样的亏
# 向上取整
当求a/b向上取整时可以：
```
  int ans=(a-1)/b+1;
```
**取整函数：**
floor函数：

floor(x)返回小于等于x的整数部分 

如：floor(2.5) = 2  floor(-2.5) = -3    

ceil函数：

ceil(x)返回不大于x的最小整数

如： ceil(2.5) = 2  ceil(-2.5) = -2

**两者对整数没有区别，对负数结果不同**

# 二分
二分过程最好用mid = (l+r)>>1

对正数无影响，但负数因为涉及到向上还是向下取整，有区别，无脑全部用>>就行了
# 奇偶向上取整问题
该数是如果是偶数结果不用动，如果是奇数结果就加1，一个小技巧吧， ans + = ( n & 1 )
# STL
* find函数
```
find函数有三个参数, 分别代表 
(起点, 终点后一位, 要找的数) 
返回一个地址
可以是容器, 或者数组
如果没有找到, 则返回终点后一位的地址 
找到了, 返回区间[first,end)中第一个值等于value的元素的地址
```
* distance函数
```
distance是返回容器中两个地址之间的距离

参数为(地址, 地址) 
返回值为整型
例子：
#include <bits/stdc++.h>
using namespace std;
 
int main()
{
    int a[10];
    int dis = distance(a, a+2);
    cout << "距离为: " << dis << endl; 
 
    for(int i = 0; i < 10; ++i) {
        a[i] = i;
    }
    cout << "该数组中, 2和7的距离是: \n";
    cout << distance(find(a, a+10, 2), find(a, a+10, 7));
}
```
* next_permutation和prev_permutation
```
next_permutation求一段序列的下一个字典序，prevmutation与之相反
```
* vector
```
  size()  返回元素个数
  empty()  返回是否为空
  clear()  清空
  front()/back()
  push_back()/pop_back()
  begin()/end()
  []
  支持比较运算，按字典序
```
* pair<__, __>
```
  first, 第一个元素
  second, 第二个元素
  支持比较运算，以first为第一关键字，以second为第二关键字（字典序）
```
* string 字符串
```
  size()/length()  返回字符串长度
  empty()
  clear()
  substr(起始下标，(子串长度))  返回子串
  c_str()  返回字符串所在字符数组的起始地址
```
* queue, 队列
```
  size()
  empty()
  push()  向队尾插入一个元素
  front()  返回队头元素
  back()  返回队尾元素
  pop()  弹出队头元素
```
* priority_queue, 优先队列，默认是大根堆
```
  push()  插入一个元素
  top()  返回堆顶元素
  pop()  弹出堆顶元素
  定义成小根堆的方式：priority_queue<int, vector<int>, greater<int> > q;
```
* stack, 栈
```
  size()
  empty()
  push()  向栈顶插入一个元素
  top()  返回栈顶元素
  pop()  弹出栈顶元素
```
* deque, 双端队列
```
    size()
    empty()
    clear()
    front()/back()
    push_back()/pop_back()
    push_front()/pop_front()
    begin()/end()
    []
```
* set, map, multiset, multimap, 基于平衡二叉树（红黑树），动态维护有序序列
```
    size()
    empty()
    clear()
    begin()/end()
    ++, -- 返回前驱和后继，时间复杂度 O(logn)

    set/multiset
        insert()  插入一个数
        find()  查找一个数
        count()  返回某一个数的个数
        erase()
            (1) 输入是一个数x，删除所有x   O(k + logn)
            (2) 输入一个迭代器，删除这个迭代器
        lower_bound()/upper_bound()
            lower_bound(x)  返回大于等于x的最小的数的迭代器
            upper_bound(x)  返回大于x的最小的数的迭代器
    map/multimap
        insert()  插入的数是一个pair
        erase()  输入的参数是pair或者迭代器
        find()
        []  注意multimap不支持此操作。 时间复杂度是 O(logn)
        lower_bound()/upper_bound()
```
**注意：**

能用内置函数尽量用内置函数，比如lower_bound()内置的就比一般的快

* bitset, 圧位
```
    bitset<10000> s;
    ~, &, |, ^
    >>, <<
    ==, !=
    []

    count()  返回有多少个1

    any()  判断是否至少有一个1
    none()  判断是否全为0

    set()  把所有位置成1
    set(k, v)  将第k位变成v
    reset()  把所有位变成0
    flip()  等价于~
    flip(k) 把第k位取反
```