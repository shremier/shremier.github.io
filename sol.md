﻿
## T1 sol

显然我们发现这个生成的序列一定是一个V字形，1是最底端。

然后这个删除序列一定有这样一个性质：构造删除序列的时候，新加入的数字要么是未出现过的数字的最大值，要么严格小于出现过的数字的最小值。

证明：我们假设当前最小值是从左边取出来的，那么从左边取一定是严格小于它的，从右边取的话，你不可能取出小于未出现过的数字的最大值的数，因为这个未出现的最大值还没有被取出来，比他小的自然也取不出来。而如果从最小值到最大值都取了的话，你下一步取出的一定是新的最小值。

所以我们设$f[i][j]$表示删除序列大小为i，当前最小值为j的方案数，那么$f[i][j]=\sum_{k=j}^{n}f[i-1][k]$，也就是要么原来的最小值就是j，这次取出来了一个未出现的最大值，要么这次取的数是j。

用前缀和优化可以做到$O(n^2)$。

但是有个问题，1可能在k位之前就出现了，所以$f[k][1]$不是答案，答案是$f[k][1]-f[k-1][1]$，这样就减去了1提前出现的情况，之后后面的序列方案数就是$2^{n-k-1}$，即：枚举它是从左边还是右边出来的。

## T2 sol

我们可以把斜着的和横竖分开算，横竖的答案就是$ m*C(n,3)+n*C(m,3) $，然后考虑斜着的答案：

首先斜率为正和斜率为负的方案数显然是相同的，这里只考虑斜率为正的答案。

我们枚举两端的点的横纵坐标之差$i,j​$，然后中间整点的个数就随之确定了，然后这样的“两端的点对”的个数正是$(n-i)*(m-j)​$，中间的点个数是$gcd(i,j)-1​$，原因看下图：
![hh](http://images.cnblogs.com/cnblogs_com/CK6100LGEV2/1263985/o_hh.png)
可以把原图切割成$gcd(i,j)$个边长为$(i/gcd(i,j),j/gcd(i,j))$的小三角形，所以整点就有$gcd(i,j)-1$个。
所以得出$ans=\sum_{i=1}^{n}\sum_{j=1}^{m}(gcd(i,j)-1)(n-i)(m-j)$
$$=\sum_{i=1}^{n}\sum_{j=1}^{m}(n-i)(m-j)\sum_{d|gcd(i,j)}\varphi(d)-\sum_{i=1}^{n}\sum_{j=1}^{m}(n-i)(m-j)$$
假设$$n<m$$

$$=\sum_{d=1}^{n}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}(n-id)(m-id)\varphi(d)-\sum_{i=1}^{n}\sum_{j=1}^{m}(n-i)(m-j)$$

因为$(n-id),(m-id),(n-i)(m-j)$这些都是等差数列，能够$$O(1)$$计算，所以最终的复杂度为$$O(n)$$。

## T3 Sol

首先我们可以推出来：

$$p[i]=max(a_j+\sqrt{i-j})-a_i(j<i)$$

$$p[i]=max(a_j+\sqrt{j-i})-a_i(j>i)$$

然后我们可以分两次dp。

但是暴力dp是$O(n^2)​$的，会TLE，我们发现这题满足决策单调性的性质。具体地，如果k这个位置的决策是j，那么对于任意$$i<j​$$，i就不会再产生任何贡献，因为根号的增长会越来越慢，j的增长会比i还要快。而且每个点的贡献区域一定是一个区间，这样的话我们可以进行整体二分，我们每次找到一个区间mid的决策位置pos（直接暴力找），然后根据$$[l,mid)​$$的决策区域是$$[L,pos]​$$，$$(mid,r]​$$的决策区域是$$[pos,R]​$$继续进行求解，注意大小写，小写是处理区间，大写是可决策区间。

求出$p_i$之后，就转化成了区间小于等于某个数的数字个数，这个可以使用主席树来解决。

时间复杂度$O(nlogn+mlogp_i)$。

本题可能卡常！本题可能卡常！本题可能卡常！（也许是出题人自带大常数= =）

我的程序在我的老爷机上跑了4.7秒左右，当然我的机子比学校的机子慢，和ccf老爷机速度差不多。
