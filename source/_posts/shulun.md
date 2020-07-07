---
title: 数论的一些基本定理
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 算法
comments: true
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/55.webp
date: 2020-06-03 08:56:46
authorLink:
tags: 数论
keywords:
description: 数论的一些基本定理
---
## 欧几里得定理
其实就是求gcd的辗转相除法，gcd(a,b)==gcd(a-b,b)，由此可以把a中的b全部拿掉，gcd(a,b)==gcd(a%b,b)， ~a是大于b的~   
gcd(a,b)==gcd(b,a%b)
## 欧拉函数
[具体证明点击我](https://blog.csdn.net/qq_37493070/article/details/81988725)
X(N)==N * (1/p1) * (1/p2) * (1/p3) *... *(1/pn)(pi为N的质因子)   
### 性质
1. **对于任意一个质数 p ,φ(n)=n−1**
<font color="red">因为n为质数,与他互质的个数就是 n-1</font>

2. **当 gcd(n,m)=1时,φ(nm)=φ(n)φ(m)**
<font color="red">因为φ(n)是积性函数。 积性函数指对于所有互质的整数a和b有性质f(ab)=f(a)f(b)的数论函数。</font>

3. **若 n=p^k^ 其中p为质数,则φ(n)=p^k^−p^k−1^=(p−1)p^k−1^**
<font color="red">1→n中除了p的倍数，都与p^k^互质,1→n中p倍数的个数为 p^k^÷p=p^k−1^</font>

5. **所有小于n与n互质个数的和sum=n × φ(n)/2**

6. **如果 i mod p=0,其中p为质数,则 φ(i ∗ p)=p ∗ φ(i),否则φ(i ∗ p)=(p−1)φ(i)**

7. **n=∑d|nφ(d) (d|n)指n是d的倍数**

8. **当 N > 2 时，φ( N )是偶数**
## 欧拉定理

**对于互质的整数a,m,有 a^φ(m)^≡1 (mod m)。**

## 费马小定理

**费马小定理(Fermat's little theorem)是数论中的一个重要定理，在1636年提出。如果p是一个质数，而整数a不是p的倍数，则有a^（p-1）≡1（mod p）**

主要应用于求高阶次幂对某个数求余数，[点击我](https://baike.baidu.com/item/%E8%B4%B9%E9%A9%AC%E5%B0%8F%E5%AE%9A%E7%90%86/4776158?fr=aladdin)

## 逆元

**a^(m-1)^=1(mod m), a * a^(m-2)^=1(mod m)，所以a的逆元是：a^(m-2)^ ,当m与a互质时。**