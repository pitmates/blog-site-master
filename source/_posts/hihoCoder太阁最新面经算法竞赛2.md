---
title: hihoCoder太阁最新面经算法竞赛2
date: 2016-06-02 21:00:30
tags: contest
category:
 - algorithm
 - hihocoder
---

　　[hihoCoder太阁最新面经算法竞赛2](http://hihocoder.com/contest/hihointerview7) 

<!-- more -->

## [A、任务分配](http://hihocoder.com/contest/hihointerview7/problem/1)
### 题意：
　　给定 n 个任务，第 i 个任务起始时间为si，结束时间为ei。计算最少需要多少台机器才能按时完成所有任务。同一时间一台机器上最多进行一项任务，并且一项任务必须从头到尾保持在一台机器上进行。任务切换不需要时间。

### 解法：
　　将任务按起始时间从小到大排序，按时间依次执行任务。另开一个数据结构存储正在执行的任务的机器的结束时间。

　　如果即将要来执行的任务的起始时间比正在执行的所有机器的结束时间要小的话，另开一台机器；否则，将任务放在任一台结束时间比之小的机器上（一般选择最早结束任务的机器），并更新该台机器的结束时间。

　　`multiset`能很快得解决该问题。

### code

```
#include <bits/stdc++.h>

#define rep(i, j, k) for(int i = (int) j; i < (int) k; ++i)
#define ll long long
#define mp make_pair
#define pii pair<int, int >
#define fi first
#define se second

using namespace std;

const int N = 1e5+5;
pii p[N];
multiset<int > s;
int n;

int main()
{
    ios_base::sync_with_stdio(false);

    cin >> n;
    rep(i, 0, n) cin >> p[i].fi >> p[i].se;
    sort(p, p+n);
    s.clear();
    s.insert(p[0].se);
    multiset<int >::iterator it;
    rep(i, 1, n) {
        it = s.begin();
        if((*it) <= p[i].fi) s.erase(it);
        s.insert(p[i].se);
    }
    cout << s.size() << endl;
    return 0;
}
```



## [B、岛屿](http://hihocoder.com/contest/hihointerview7/problem/2)

### 题意：
　　给你一张某一海域卫星照片，你需要统计：

　　1. 照片中海岛的数目
　　2. 照片中面积不同的海岛数目
　　3. 照片中形状不同的海岛数目
　　
　　卫星最大为 50 x 50。

### 解法：
　　1. 海岛数量不同说，简单`bfs`就可以统计出来。

　　2. 面积不同：在`bfs`时同时记录同块海岛的其它位置的坐标，并统计块数。最后统计一下海岛数中块数不同的个数就OK了。按海岛块数多少排序一下线性扫一遍就解决了。

　　3. 形状不同：<b>面积不同则形状一定不同</b>，那么我们只需计算在面积相同的海岛内形状不同的个数。根据 2 的排序，我们可以知道面积相同的海岛在一个连续的区间里面。在这区间里面比较2个海岛是否形状一样。采用`向量比较法`：
　　　假设 i < j，
　　　第i块海岛由(xi1, yi1), (xi2, yi2), ..., (xik, yik)构成，
　　　第j块海岛由(xj1, yj1), (xj2, yj2), ..., (xjk, yjk)构成。
　　　p从2到k比较(xip-xi1, yip-yi1)与(xjp-xj1, yjp-yj1)是否一样，如果2->k都完全一样，代表2块海岛形状一样，把j标记为其他颜色。</center>
　　最后统计最初颜色的海岛有几块就是形状不一样的海岛数目了。

### code

```
#include <bits/stdc++.h>

#define rep1(i, j, k) for(int i = (int) j; i < (int) k; ++i)
#define rep(i, j, k) for(int i = (int) j; i <= (int) k; ++i)
#define ll long long
#define mp make_pair
#define pii pair<int, int >
#define fi first
#define se second

using namespace std;
const int N = 53;
char c[N];
bool g[N][N];
bool used[N][N];
pair<int, vector<pii > > sharp[N*N];
int t;
int n, m;
int dx[] = {-1, 0, 0, 1};
int dy[] = {0, -1, 1, 0};

void bfs(int x, int y) {
    queue<pii > q;
    q.push(mp(x, y));
    used[x][y] = 1;
    ++t; int z = 0;
    while(!q.empty()) {
        pii p = q.front();
        q.pop();
        ++z;
        sharp[t].se.push_back(mp(p.fi, p.se));
        rep(i, 0, 3) {
            int u = p.fi + dx[i], v = p.se + dy[i];
            if(!used[u][v] && g[u][v] && u && u <= n && v && v <= m) {
                used[u][v] = 1;
                q.push(mp(u, v));
                ++z;
            }
        }
    }
    sharp[t].fi = z;
    return ;
}

int trace(int u, int v) {
    int sz = sharp[u].se.size();
//printf("u:%d v:%d sz:%d\n", u, v, sz);
    if(sz == 1) return 1;
    int f = 1;
    rep1(i, 1, sz) {
        int x1 = sharp[u].se[i].fi - sharp[u].se[i-1].fi;
        int y1 = sharp[u].se[i].se - sharp[u].se[i-1].se;
        int x2 = sharp[v].se[i].fi - sharp[v].se[i-1].fi;
        int y2 = sharp[v].se[i].se - sharp[v].se[i-1].se;
//printf("i:%d (x1:%d y1:%d) (x2:%d y2:%d)\n", i, x1, y1, x2, y2);
        if(x1 != x2 || y1 != y2) return 0;
    }
    return 1;
}


void solve() {
    memset(used, false, sizeof used);
    t = -1;
    rep(i, 1, n) {
        rep(j, 1, m) {
            if(!used[i][j] && g[i][j]) bfs(i, j);
        }
    }
    ++t; // the number of islands

    sort(sharp, sharp+t);
    sharp[t].fi = -1;

    int tt = 0; // the islands of different area
    rep1(i, 0, t) if(sharp[i].fi != sharp[i+1].fi) ++tt;

    int ttt = 0;
    int l = 0, r = 0;
    bool vis[N*N];
    memset(vis, 0, sizeof vis);
    rep(i, 1, t) {
        if(sharp[i].fi == sharp[i-1].fi) r = i;
        else {
            //printf("l:%d   r:%d\n", l, r);
            if(r > l){
                for(int j = l; j <= r; ++j) {
                    if(vis[j]) continue;
                    for(int k = j+1; k <= r; ++k) {
                        if(vis[k]) continue;
                        if(trace(j, k)) vis[k] = 1;
                    }
                }
       // rep(j, l, r) printf("#%d ", vis[j]); puts("");
            }
            l = r = i;
        }
    }
    //rep1(i, 0, t) printf("%d ", vis[i]); puts("");
    rep1(i, 0, t) if(!vis[i]) ++ttt;

    printf("%d %d %d\n", t, tt, ttt);
}

int main()
{
#ifdef PIT
freopen("2.in", "r", stdin);
#endif // PIT
    scanf("%d %d", &n, &m);
    rep(i, 1, n) {
        scanf("%s", c);
        rep1(j, 0, m) g[i][j+1] = (c[j] == '#');
    }
    solve();
    return 0;
}
```

## [C、二进制小数](http://hihocoder.com/contest/hihointerview7/problem/3)

### 题意：
　　给定一个十进制小数X，判断X的二进制表示是否是有限确定的。

　　例如0.5的二进制表示是0.1，0.75的二进制表示是0.11，0.3没有确定有限的二进制表示。

　　小数部分长度不超过100。

### 解法：

　　1. $ 10^{-100} $ 化成二进制大约为334位，所以我们开一个500位的字符串数组，第i个字符串存储2^(-i)的字符串表示， 因为i每次`+1`，最多增加一个有效位`0`和最后一位数字`5`,所以单个字符串最长不超过1000。当超过500还未表示完，我们就把它归为无限二进制小数。

　　2. i从1-500比较字符串i与待定字符串的大小，如果小，则为0；大则为1，然后减去字符串i的值。

　　3. 若i的位置大于原定串的长度，则判断减完值是否为`000···`，若是，则跳出循环。也可以算完500个后判断值是否为`000···`，再去掉后缀0就行。

### code

```
#include <bits/stdc++.h>

#define rep(i, j, k) for(int i = (int) j; i < (int) k; ++i)
#define ll long long
#define mp make_pair

using namespace std;

const int N = 505;

char s[N][N<<1];
int T, len;
char c[N];
int ans[N<<1], t;

void init() {
    s[1][0] = '0';s[1][1] = '.';s[1][2] = '5';
    rep(i, 2, N) {
        int len = strlen(s[i-1]);
        int m = 0, d;
        rep(j, 0, len) {
            if(s[i-1][j] == '.') { s[i][j] = '.'; continue;}
            d = m * 10 + s[i-1][j] - '0';
            s[i][j] = d / 2 + '0';
            m = d % 2;
        }
        if(m) s[i][len] = '5';
    }
    return ;
}

void sub(char s[]) {
    int le = strlen(s);
    int lend = 0, va, i = le-1;
    if(len < le) {
        for( ; i >= len; --i) {
            va = (i == le-1) ? 10 : 9;
            c[i] = va - s[i]  + '0' + '0';
        }
        va = i;
        while(c[va] == '0') {c[va] = '9'; --va; }
        if(c[va] != '.') c[va] = c[va] - 1;

    }
    for( ; i > 1; --i) {
//printf("i:%d c:%c s:%c \n", i, c[i], s[i]);
        if(c[i] < s[i]) {
            c[i] = 10 + c[i] - s[i] + '0';
            va = i-1;
            while(c[va] == '0') { c[va] = '9'; --va;}
            c[va] = c[va] - 1;
//printf("c:  %s\n", c);
        }
        else c[i] = c[i] - s[i] + '0';
//printf("==> i:%d c:%s  \n", i, c);
    }
    return ;
}

void solve() {
    bool f; t = 1;
    rep(i, 1, N) {
        len = strlen(c); f = 1;
        rep(j, 2, len) if(c[j] != '0') {f = 0; break;}
        if(f) break;
        int v = strcmp(c, s[i]);
        if(v < 0) { ans[t++] = 0; continue; }
        ans[t++] = 1;
        sub(s[i]);
    }
    if(!f) puts("NO");
    else {
        printf("0.");
        rep(i, 1, t) printf("%d", ans[i]);
        puts("");
    }
    return ;
}

int main()
{
#ifdef PIT
freopen("3.in", "r", stdin);
#endif // PIT
    init();
    for(scanf("%d", &T) ; T; --T) {
        scanf("%s", c);
        solve();
    }
    return 0;
}

```