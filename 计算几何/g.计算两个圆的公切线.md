# g.计算两个圆的公切线

## 思路

两圆的共切线，根据两圆的圆心距从小到大排列，一共有六种情况。

1）两圆完全重合，有无数条公切线，返回-1；

2）两圆内含，没有公共点，无公切线，返回0；

3）两圆内切，有一条外公切线；

4）两圆相交，有两条外公切线；

5）两圆外切，有两条外公切线，一条内公切线；

6）两圆相离，有两条外公切线，两条内公切线；

## 代码

```C++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cmath>
#include<vector>
#define FIR first
#define SEC second
using namespace std;
const double eps = 1e-8;
const double PI = acos(-1.0);
int dcmp(double x)
{
    if(fabs(x)<eps)
        return 0;
    return x < 0? -1: 1;
}
struct pnode
{
    double x,y;
    pnode(double x=0.0,double y=0.0):x(x),y(y){}
    pnode operator - (const pnode &b)const{ return pnode(x-b.x,y-b.y);}
    pnode operator + (const pnode &b)const{ return pnode(x+b.x,y+b.y);}
    bool operator < (const pnode &b)const
    {
        return dcmp(x - b.x)<0 || ( dcmp(x-b.x)==0 && dcmp(y-b.y)<0);
    }
};
struct circle
{
    double x,y,r;
    circle(double x=0.0,double y=0.0,double r=0.0):x(x),y(y),r(r){}
    pnode point(double a)
    {
        return pnode( x+r*cos(a),y+r*sin(a));
    }
    int pread()
    {
       return scanf("%lf %lf %lf",&x,&y,&r);
    }
};
int get_tangents(circle A, circle B, pnode *a, pnode *b){
    int cnt = 0;        //存切点用
    if(dcmp(A.r - B.r) < 0)
    {
        swap(A, B);
        swap(a, b);
    }
    double d = sqrt((A.x - B.x) * (A.x - B.x) + (A.y - B.y) * (A.y - B.y));     //圆心距
    double rdiff = A.r - B.r;      //两圆半径差
    double rsum  = A.r + B.r;       //两圆半径和
    if(dcmp(d - rdiff) < 0)
        return 0;        //1.内含
    double base = atan2(B.y - A.y, B.x - A.x);      //向量AB的极角
    if(dcmp(d) == 0) return -1;        //2.重合
    if(dcmp(d - rdiff) == 0)
    {       //3.内切
        a[cnt] = b[cnt] = A.point(base);
        cnt++;
        return 1;
    }
    double ang = acos((A.r - B.r) / d);
    a[cnt] = A.point(base + ang); b[cnt] = B.point(base + ang); cnt++;      //4.相交（外切、外离的外公切线也在此求出）
    a[cnt] = A.point(base - ang); b[cnt] = B.point(base - ang); cnt++;      //两条外公切线的切点
    if(dcmp(d - rsum) == 0)
    {        //5.外切
        a[cnt] = b[cnt] = A.point(base);
        cnt++;
    }
    else if(dcmp(d - rsum) > 0)
    {      //6.外离
        double ang = acos((A.r + B.r) / d);
        a[cnt] = A.point(base + ang); b[cnt] = B.point(PI + base + ang); cnt++;
        a[cnt] = A.point(base - ang); b[cnt] = B.point(PI + base - ang); cnt++;
    }
    return cnt;
}
bool check(circle A)
{
    return dcmp(A.x)==0&&dcmp(A.y)==0&&dcmp(A.r)==0;
}
typedef pnode myvec;
double length( myvec a)
{
    return sqrt( a.x * a.x + a.y * a.y );
}
int main()
{
    circle A,B;
    pnode a[10],b[10];
    while( A.pread()==3 &&B.pread()==3 )
    {
        if( check(A)&&check(B) )break;
        int ans = get_tangents(A,B,a,b);
        if( ans==-1 || ans ==0 )
        {
            printf("%d\n",ans);
            continue;
        }
        printf("%d\n",ans);
        vector<pair<pnode,pnode> > group;
        for( int i = 0; i < ans;++i)
            group.push_back( make_pair(a[i],b[i]));
        sort( group.begin(),group.end() );
        for( vector<pair<pnode,pnode> > ::iterator i = group.begin();i!=group.end();i++ )
            printf("%.5lf %.5lf %.5lf %.5lf %.5lf\n",
                   i->FIR.x,i->FIR.y,i->SEC.x,i->SEC.y,length(i->FIR - i->SEC));
    }
 
    return 0;
}
```

