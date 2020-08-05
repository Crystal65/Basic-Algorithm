# e.判断点是否在多边形内

## 思路

- **面积和判别法**：判断目标点与多边形的每条边组成的三角形面积和是否等于该多边形，相等则在多边形内部。
- **夹角和判别法**：判断目标点与所有边的夹角和是否为360度，为360度则在多边形内部。
- **引射线法**（详解）：从目标点出发引一条射线，看这条射线和多边形所有边的交点数目。如果有奇数个交点，则说明在内部，如果有偶数个交点，则说明在外部。

![](https://img-blog.csdn.net/20150702110951157?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveWFuanVubXU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

如果从点P作水平向左的射线的话，假设P在多边形内部，那么这条射线与多边形的交点必为奇数，如果P在多边形外部，则交点个数必为偶数（0也在内）。

所以，我们可以顺序（顺时针或逆时针）考虑多边形的每条边，求出交点的总个数。

还有一些特殊情况要考虑。假如考虑边(P1,P2)，
1) 如果射线正好穿过P1或者P2,那么这个交点会被算作2次，处理办法是如果P的从坐标与P1,P2中较小的纵坐标相同，则直接忽略这种情况
2) 如果射线水平，则射线要么与其无交点，要么有无数个，这种情况也直接忽略。
3) 如果射线竖直，而P的横坐标小于P1,P2的横坐标，则必然相交。
4) 再判断相交之前，先判断P是否在边(P1,P2)的上面，如果在，则直接得出结论：P再多边形内部。

## 代码

### C

```C
#define min(a,b) a<b?a:b
#define max(a,b) a>b?a:b

typedef struct tagST_POINT {
    int x;
    int y;
} ST_POINT;
 
/**
 * 功能：判断点是否在多边形内
 * 方法：求解通过该点的水平线（射线）与多边形各边的交点
 * 结论：单边交点为奇数，成立!
 * 参数：p 指定的某个点
         ptPolygon 多边形的各个顶点坐标（首末点可以不一致） 
         nCount 多边形定点的个数
 * 说明：
 */
bool PtInPolygon(ST_POINT p, ST_POINT* ptPolygon, int nCount) 
{ 
    int nCross = 0, i;
    double x;
    ST_POINT p1, p2;
    
    for (i = 0; i < nCount; i++) 
    { 
        p1 = ptPolygon[i]; 
        p2 = ptPolygon[(i + 1) % nCount];
        // 求解 y=p.y 与 p1p2 的交点
        if ( p1.y == p2.y ) // p1p2 与 y=p.y平行 
            continue;
        if ( p.y < min(p1.y, p2.y) ) // 交点在p1p2延长线上 
            continue; 
        if ( p.y >= max(p1.y, p2.y) ) // 交点在p1p2延长线上 
            continue;
        // 求交点的 X 坐标 -------------------------------------------------------------- 
        x = (double)(p.y - p1.y) * (double)(p2.x - p1.x) / (double)(p2.y - p1.y) + p1.x;
        if ( x > p.x ) 
        {
            nCross++; // 只统计单边交点 
        }
    }
    // 单边交点为偶数，点在多边形之外 --- 
    return (nCross % 2 == 1); 
}
 
// 注意：在有些情况下x值会计算错误，可把double类型改为long类型即可解决。


```

