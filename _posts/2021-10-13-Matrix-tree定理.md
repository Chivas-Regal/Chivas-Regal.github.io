---
layout: article
title: Matrix-tree定理
subtitle: 图上的总生成树问题还能用矩阵玩出花？🤔
categories: 图论
tags: [图论]
aside:
  toc: true
sidebar:
  nav: docs-en
---

>对于一个无向图，我们想知道它的生成树数量

# 前置知识——行列式

## 定义

一个$n$阶方阵也就是 $A$ 行列式的值记为 $det(A)$ 或 $|A|$  
$$det(A)=\sum\limits_P(-1)^{inv(P)}\prod\limits_iA_{1,P_1}...A_{n,P_n}$$  
$P=permutation(1...n)$  
$inv$是当前排列$P$的逆序对数量

## 性质  

$1.$ 交换行列式两行(列)，行列式的值<span style="color: red;">变号</span>  
$2.$ 一个上三角行列式的值是<span style="color: red;">主对角线的累乘</span>  
$3.$ 行列式一行(列)乘或除任意数值不影响整个行列式的值  
$4.$ 行列式一行(列)减去另一行(列)后不影响整个行列式的值

## 性质应用——高斯消元  

高斯消元可以将一个行列式<mark>主对角线下面的部分解成全部是0</mark>的情况  
主体思路请看[高斯消元](https://chivas-regal.github.io/%E6%95%B0%E5%AD%A6/2021/08/23/%E9%AB%98%E6%96%AF%E6%B6%88%E5%85%83.html)  
在这里我们要完整地解出一个全部都是整数的行列式的整数值  
由于辗转相除法是可以将第二个元素消到$0$，所以可以<mark>结合辗转相除法</mark>使用  
对于一个我们想要通过第$A_{jj}$个单位消到$0$的单位$A_{ij}$  
我们可以只考虑他们两个，但是由于操作时为了保证行列式值的正确性我们要<span style="color: red;">操作整行</span>  
所以记录一下$A_{jj}$除$A_{ij}$后向下取整后的值$d$，对于这个两个我们让$A_{jj}-=A_{ij}\times d$便可实现取模操作，然后对于这两行每一列的两个单位进行同样的操作  
最后再互换一下，直到我们要消的元素变成0为止

## 计算程序

```cpp
inline int Gauss ( int n ) {
	int res = 1;
	for ( int i = 1; i <= n; i ++ ) { // 在(i, i)上进行消元
		for ( int ii = i + 1; ii <= n; ii ++ ) { // 将(ii, i)变成0
			while ( a[ii][i] ) { // 还没有消到 0
				int d = a[i][i] / a[ii][i];
				for ( int j = i; j <= n; j ++ )
					a[i][j] = (a[i][j] - (ll)d * a[ii][j] % mod + mod) % mod,
					swap ( a[i][j], a[ii][j] );
				res = -res; // 行(列)互换最后的值要相反
			}
		}
		res = (ll)res * a[i][i] % mod; // 乘对角线
		if ( res == 0 ) return 0;
	}
	return (res % mod + mod) % mod;
}
```

# 矩阵树定理

<mark>这里只指无向图</mark>

## 生成树个数
对于一个图，我们可以按这样的规则<mark>建立一个行列式$A$</mark>    
$A_{ij}\quad i\ne j$ 表示$i$点和$j$点的连边数量的<span style="color: red;">相反数</span>  
$A_{ii}$ 表示$i$点的度数  

例：
<img src="https://img-blog.csdnimg.cn/17b66c7892004e2ea4b360c695258b7f.png">  
对于这样一个图，我们可以建出如下矩阵  
$$\left\{\begin{matrix}
    &2 &-1 &-1 &-1 &-1\\
    &-1 &2 &-1 &-1 &-1\\
    &-1 &-1 &2 &-1 &-1\\
    &-1 &-1 &-1 &2 &-1\\
    &-1 &-1 &-1 &-1 &2
\end{matrix}\right\}$$

那么这样的一个<mark>行列式的值就是生成树的个数</mark>  

## 生成树权值积的和

可以将$A_{ij}$换成<mark>连边的权值和</mark>

## 生成树权值和的和

我们求不了和，但是可以利用权值积利用有效的部分变成权值和  
首先看一个<mark>多项式</mark>乘积形式：  
$$(ax+b)(cx+d)=acx^2+(ad+bc)x+bd$$  
在$b=d=1$时，多项式的一次项系数就是$a+c$  
这里就转化成和的形式了  
所以每次将$A_{ij}$换成$(连边的权值和\times x+1)$即可  
  
在高斯消元中，我们还涉及到其他运算  
所以<mark>要定义一下它们的其他操作</mark>  
由于每次只看最后两项，所以在四种运算时多项式不用变得太长  
$$\begin{aligned}
&(ax+b)+(cx+d)=(a+c)x+(b+d)\\
&(ax+b)-(cx-d)=(a-c)x+(b-d)\\
&(ax+b)\times (cx-d) = ...+(ad + bc)x + bd\\
&(ax+b)\div (cx-d)=\frac {ad-bc}{d^2}x+\frac bd
\end{aligned}$$  

# 例题 
**1.洛谷P2144 轮状病毒**  
<a href="https://www.luogu.com.cn/problem/P2144">题目地址</a>  
<a href="https://github.com/Chivas-Regal/ACM/blob/main/Code/%E5%9B%BE%E8%AE%BA/%E7%9F%A9%E9%98%B5%E6%A0%91/%E6%B4%9B%E8%B0%B7P2144_%E8%BD%AE%E7%8A%B6%E7%97%85%E6%AF%92.md">题解地址</a>  
**2.洛谷P4111 小Z的房间**  
<a href="https://www.luogu.com.cn/problem/P4111">题目地址</a>  
<a href="https://github.com/Chivas-Regal/ACM/blob/main/Code/%E5%9B%BE%E8%AE%BA/%E7%9F%A9%E9%98%B5%E6%A0%91/%E6%B4%9B%E8%B0%B7P4111_%E5%B0%8FZ%E7%9A%84%E6%88%BF%E9%97%B4.md">题解地址</a>  
**3.洛谷P4336 黑暗前的幻想乡**  
<a href="https://www.luogu.com.cn/problem/P4336">题目地址</a>  
<a href="https://github.com/Chivas-Regal/ACM/blob/main/Code/%E5%9B%BE%E8%AE%BA/%E7%9F%A9%E9%98%B5%E6%A0%91/%E6%B4%9B%E8%B0%B7P4336_%E9%BB%91%E6%9A%97%E5%89%8D%E7%9A%84%E5%B9%BB%E6%83%B3%E4%B9%A1.md">题解地址</a>  

