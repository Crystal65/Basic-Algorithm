# h.求矩形的并的面积

## 题目

给定n个矩形，求出它们的矩形面积之和，相交的面积算一次

## 思路

- **离散化**（适合矩阵个数较少）

  将组成的图形 按照给定点的横纵坐标划线，这样就可以将整个要求的图形的面积分成几个小块，然后依次求每个小块的面积。注意，首先要判断方块在不在包含的范围内。具体做法就是 先将 横纵坐标从小到大排序，去掉空白的方块，最后将各小方块相加，求出总面积。

- **扫描线**

  首先把矩形按y轴分成两条边, 上边和下边, 对x轴建树, 扫描线可以看成一根平行于x轴的直线. 
  从y=0开始往上扫, 下边表示要计算面积+1, 上边表示已经扫过了−1, 直到扫到最后一条平行于x轴的边 
  但是真正在做的时候, 不需要完全模拟这个过程, 一条一条边地插入线段树就好了

  ![](https://img-blog.csdn.net/20151005001227138)

  <center>初始状态

  ![](https://img-blog.csdn.net/20151005001522369)

  <center>扫到最下边的线, 点1→3更新为1

  ![](https://img-blog.csdn.net/20151005001541628)

  
  $$
  扫到第二根线, 此时S=l_{cnt!=0}∗h_{两根线之间}得到绿色的面积, 加到答案中去, 随后更新计数
  $$
  ![](https://img-blog.csdn.net/20151005001646747)

  <center>同上，将黄、灰、紫、蓝色的面积分别加入到答案中去

- **线段树**

  用于动态维护扫描线在往上走时, x轴哪些区域是有合法面积

## 代码

**离散化**

```C++
#include <iostream>
#include <cstring>
#include <iomanip>
#include <algorithm>
using namespace std;
 
#define MAXN 210
int flag[MAXN][MAXN];
double x[MAXN];   //保留所有x的坐标
double y[MAXN];   //保留所有y的坐标
 
struct rectangle{
    double a,b;
    double u,v;
}rec[MAXN];
 
int main(){
    int n;
    int i=0,j=0,k;
    double x1,y1,x2,y2;
    cin>>n;
    memset(flag,0,sizeof(flag));
    for(k=0;k<n;k++){
        cin>>x1>>y1>>x2>>y2;
        rec[j].a = x1;
        rec[j].b = y1;
        rec[j].u = x2;
        rec[j].v = y2;
        j++;
        x[i] = x1;
        y[i] = y1;
        i++;
        x[i] = x2;
        y[i] = y2;
        i++;
    }
    sort(x,x+2*n);
    sort(y,y+2*n);
    for(int h=0;h<n;h++){    //求包含的小矩形
        for(i=0;i<2*n;i++){
            if(x[i]>=rec[h].u)
                break;
            for(j=0;j<2*n;j++){
                if(y[j]>=rec[h].v)
                    break;
                if(x[i]>=rec[h].a && y[j]>=rec[h].b)
                    flag[i][j] = 1;
            }
        }
    }
    double sum = 0;
    for(i=0;i<2*n-1;i++){
        for(j=0;j<2*n-1;j++){
            sum += flag[i][j]*(x[i+1]-x[i])*(y[j+1]-y[j]);
        }
    }
    cout<<sum<<endl;
    return 0;
}
```

**扫描线**

```C++
#include <algorithm>
#include <cctype>
#include <cmath>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <iomanip>
#include <iostream>
#include <map>
#include <queue>
#include <string>
#include <set>
#include <vector>

using namespace std;
#define pr(x) cout << #x << " = " << x << "  "
#define prln(x) cout << #x << " = " << x << endl
const int N = 205, INF = 0x3f3f3f3f, MOD = 1e9 + 7;

int n;
struct Seg {
    double l, r, h; int d;
    Seg() {}
    Seg(double l, double r, double h, int d): l(l), r(r), h(h), d(d) {}
    bool operator< (const Seg& rhs) const {return h < rhs.h;}
} a[N];

int cnt[N << 2]; //根节点维护的是[l, r+1]的区间
double sum[N << 2], all[N];

#define lson l, m, rt << 1
#define rson m + 1, r, rt << 1 | 1

void push_up(int l, int r, int rt) {
    if(cnt[rt]) sum[rt] = all[r + 1] - all[l];
    else if(l == r) sum[rt] = 0; //leaves have no sons
    else sum[rt] = sum[rt << 1] + sum[rt << 1 | 1];
}

void update(int L, int R, int v, int l, int r, int rt) {
    if(L <= l && r <= R) {
        cnt[rt] += v;
        push_up(l, r, rt);
        return;
    }
    int m = l + r >> 1;
    if(L <= m) update(L, R, v, lson);
    if(R > m) update(L, R, v, rson);
    push_up(l, r, rt);
}

int main() {
#ifdef LOCAL
    freopen("in.txt", "r", stdin);
//  freopen("out.txt","w",stdout);
#endif
    ios_base::sync_with_stdio(0);

    int kase = 0;
    while(scanf("%d", &n) == 1 && n) {
        for(int i = 1; i <= n; ++i) {
            double x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            a[i] = Seg(x1, x2, y1, 1);
            a[i + n] = Seg(x1, x2, y2, -1);
            all[i] = x1; all[i + n] = x2;
        }
        n <<= 1;
        sort(a + 1, a + 1 + n);
        sort(all + 1, all + 1 + n);
        int m = unique(all + 1, all + 1 + n) - all - 1;

        memset(cnt, 0, sizeof cnt);
        memset(sum, 0, sizeof sum);

        double ans = 0;
        for(int i = 1; i < n; ++i) {
            int l = lower_bound(all + 1, all + 1 + m, a[i].l) - all;
            int r = lower_bound(all + 1, all + 1 + m, a[i].r) - all;
            if(l < r) update(l, r - 1, a[i].d, 1, m, 1);
            ans += sum[1] * (a[i + 1].h - a[i].h);
        }
        printf("Test case #%d\nTotal explored area: %.2f\n\n", ++kase, ans);
    }
    return 0;
}
```

