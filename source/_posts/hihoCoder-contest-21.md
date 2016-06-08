---
title: hihoCoder contest 21
date: 2016-06-08 16:55:04
tags: contest
category:
 - algorithm
 - hihocoder
---

　　[hihoCoder挑战赛21](http://hihocoder.com/contest/challenge21) 

<!-- more -->

## [A、Treasure Boxes](http://hihocoder.com/problemset/problem/1313)
### 题意：
#### 描述
　　ZZX走进了一个神秘的洞穴。在他面前摆放着 k 堆箱子，其中第 i 堆中有 a[i] 个箱子，这 a[i] 个箱子从底向上堆叠成一座塔的形状。

　　ZZX知道这些箱子中有一些是装满宝物的宝箱，而剩下的箱子中只装着废铜烂铁。

　　现在他希望将所有的宝箱都搬到车上运走，而剩下的箱子都留在洞穴中。

　　ZZX每次可以执行两种操作之一，花费一分钟的时间：

　　　操作1：对于 i ≠ j，将第 i 堆最顶部的那个箱子搬到第 j 堆的顶部。（第 j 堆允许是空的，这时箱子会被搬到第 j 堆所在位置的地面上）

　　　操作2：对于 i，如果第 i 堆最顶部的箱子是宝箱，那么他可以把这个箱子搬到车上。

　　现在请你帮助ZZX计算完成任务所需要花费的最少时间。

#### 输入
　　第一行一个整数 k。(2 ≤ k ≤ 100)

　　接下来 k 行，每行一个由'o'和'x'组成的非空字符串，表示一堆箱子，最左边的字符表示底部的箱子，最右边的字符表示顶部的箱子，'o'表示装有宝物的箱子，'x'表示没有宝物的箱子。每行字符串长度不超过100。

　　保证洞穴中至少有一个箱子是装有宝物的。

#### 输出
　　输出一行表示最少需要几分钟。

### 解法：
　　贪心找到需要移动空箱子最少（设为ms）的那一组，先移动它，所有需要移动的次数就是` ms + 所有需要移动的箱子的个数 `

### code

```
#include <bits/stdc++.h>

#define rep(i,j,k) for(int i = (int) j; i < (int) k; ++i)
#define mp make_pair
#define ll long long
using namespace std;
const int N = 111;

int n;
char c[N][N];
int le[N], len[N];

int main()
{

    scanf("%d", &n);
    rep(i, 0, n) scanf("%s", c[i]);
    memset(le, 0, sizeof le);
    rep(i, 0, n) {
        int x = strlen(c[i]);
        rep(j, 0, x) if(c[i][j] == 'o') {
            le[i] = x - j;
            break;
        }
    }
    memset(len, 0, sizeof len);
    rep(i, 0, n) {
        int x = strlen(c[i]);
        rep(j, 0, x) if(c[i][j] == 'o') {
            len[i]++;
        }
    }

    int mi = N, ans = 0;
    rep(i, 0, n) {
        if(mi > le[i]-len[i]) mi = le[i]-len[i];
        ans += le[i];
    }
    ans += mi;
    printf("%d\n", ans);
    return 0;
}


```


## [B、Longest Non-Decreasing Subsequence](http://hihocoder.com/problemset/problem/1314)
### 题意：
#### 描述
<script type="text/javascript"
   src="https://cdn.mathjax.org/mathjax/latest/MathJax.js">
</script>
　　ZZX喜欢01序列，尤其是不下降的01序列。一个不下降的01序列 s[1..k] 需要满足 s[i] ≤ s[i+1],1 ≤ i < k。

　　对于一个01序列，ZZX关心的是它的最长不下降子序列。y 是 x 的子序列，当且仅当可以将序列 x 的某些元素删除，使得剩下的元素能按照原顺序组成序列 y。

　　现在ZZX要生成一个随机01序列 a[1..n]，他给序列的每一位都独立地随机生成了一个数，其中 a[i] 有 p[i] / 1000 的概率为1，有 (1000 - p[i]) / 1000的概率为0，

　　他想知道 a 的最长不下降子序列的长度的期望值是多少，设这个期望值为 E。为了避免精度问题，你只需要输出 $E * (1000^{n})$ 模 $10^9+7$后的结果。

　　现在请你帮助ZZX计算完成任务所需要花费的最少时间。

#### 输入
　　第一行一个整数 n。(1 ≤ n ≤ 200)

　　第二行有 n 个空格隔开的整数 p[1], p[2],..., p[n]。(0 ≤ p[i] ≤ 1000)

#### 输出
　　输出期望长度乘($1000^{n}$)模${10^{9}+7}$后的结果。

### 解法：
　　动态规划，`dp[i][j][k]`表示前i位，
　　　以`0`结尾的最长子序列长度为`j`的方法数；
　　　以`1`结尾的最长子序列长度为`k`的方法数。
　　那么，`dp[i-1][j][k]`的转移就很明显了：
　　　当第 i 位为`0`时，（`0`只能和`0`连接，所以是`j+1`）
```
　　　dp[i][j+1][k] = dp[i][j+1][k] + dp[i-1][j][k] * (1000 - p[i]);
```
　　　当 i 位为 `1`时，(`1`可以和`0`或`1`连接，所以是`max(j+1, k+1)`)
```
　　　dp[i][j][max(j+1, k+1)] = dp[i][j][max(k+1, j+1)] + dp[i-1][j][k] * p[i];
```
　　
　　最后，我们把`dp[n][j][k]`乘以`max(j, k)`相加后，就得出期望了。
　　总复杂度 O(n^3)。

### code

```
#include <bits/stdc++.h>

#define rep(i, j, k) for(int i = (int) j; i <= (int) k; ++i)
#define ll long long
#define mp make_pair

using namespace std;
const int mo = 1e9+7;
const int N = 202;

int dp[N][N][N];
int n, p;

inline void add(int &a, int b) { if((a += b) >= mo) a -= mo; }

int main()
{
    ios_base::sync_with_stdio(false);

    cin >> n;
    dp[0][0][0] = 1;
    rep(i, 1, n) {
        cin >> p;
        rep(j, 0, i) rep(k, 0, i) {
                add(dp[i][j+1][k], 1ll*(1000-p)*dp[i-1][j][k] % mo);
                add(dp[i][j][max(k+1, j+1)], 1ll*p*dp[i-1][j][k] % mo);
            }
    }
    int ans = 0;
    rep(i, 0, n) rep(j, 0, n) add(ans, 1ll*dp[n][i][j] * max(i, j) % mo);
    cout << ans << endl;
    return 0;
}

```

## C、D 待解决