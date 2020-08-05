# b.判断线段相交

## 思路

方法：1. **快速排斥实验（矩形实验）**
	 	   2.**跨立实验**

知识点解析：

1. 矩形实验是为了将相距距离比较远的线段直接排除（因为这样，他们不能相交），具体做法也很简单：

   - 判断两条线段横坐标与纵坐标的区间不会有交集

   - 或判断两条线段在 x 以及 y 坐标的投影是否有重合

     - 判断下一个线段中 x 较大的端点是否小于另一个线段中 x 较小的段点

       ```C
        max(c.x,d.x)<min(a.x,b.x)||max(a.x,b.x)<min(c.x,d.x)||max(c.y,d.y)<min(a.y,b.y)||max(a.y,b.y)<min(c.y,d.y);
       ```

2. 跨立实验是为了判断，两线段坐标区间有交集的线段之间的关系。

   具体思路为：一条线段的两个点在另一条线段的两侧，则满足跨立实验，具体代码实现为，利用向量与向量之间的叉乘的正负性，来做出判断。

   `(A−D)×(C−D)∗(B−D)×(C−D)<0(A−D)×(C−D)∗(B−D)×(C−D)<=0`

   注：快速排斥实验必要性：

   ![](https://img-blog.csdn.net/20180820153302779?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3pob3V6aTIwMTg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

   若直接判断第二步，叉积都为 0, 可以通过跨立实验，但是两个线段并没有交点。不过还好，这种情况在第一步快速排斥已经被排除了。   

## 代码

```C
/*
(A1-B1) × (B2-B1) * (B2-B1) × (A2-A1) >= 0
(B1-A1) × (A2-A1) * (A2-A1) × (B2-A1) >= 0
*/
 
#include<stdio.h>
#define min(a,b) a<b?a:b
#define max(a,b) a>b?a:b
typedef struct {
double x,y;
}Point;
Point A1,A2,B1,B2;
Point  A1B1, B2B1, A2A1, B2A1;
double xx(Point &s,Point &t)
{
    return (s.x*t.y+s.y*t.x);
}
int kua()                           //跨立实验
{
    A1B1.x=A1.x-B1.x;  A1B1.y=A1.y-B1.y;
    B2B1.x=B2.x-B1.x;  B2B1.y=B2.y-B1.y;
    A2A1.x=A2.x-A1.x;  A2A1.y=A2.y-A1.y;
    B2A1.x=B2.x-A1.x;  B2A1.y=B2.y-A1.y;
    if(xx(A1B1,B2B1)*xx(B2B1,A2A1)>=0)
    {
        A1B1.y=-A1B1.y;A1B1.x=-A1B1.x;
        if(xx(A1B1,A2A1)*xx(A2A1,B2A1)>=0)
            return 1;
        else
            return 0;
    }
    else
        return 0;
}
int main()
{
    Point A1,A2,B1,B2;
    int flag=1,i,j,a,b,c,d,e,f;
    while(1)
    {
        scanf("%lf%lf%lf%lf",&A1.x,&A1.y,&A2.x,&A2.y);
        scanf("%lf%lf%lf%lf",&B1.x,&B1.y,&B2.x,&B2.y);
        if( min(A1.x,A2.x) <= max(B1.x,B2.x) &&
            min(B1.x,B2.x) <= max(A1.x,A2.x) &&
            min(A1.y,A2.y) <= max(B1.y,B2.y) &&
            min(B1.y,B2.y) <= max(A1.y,A2.y)   )   //快速排斥实验
        {
            if(kua())
                printf("线段相交\n");
            else
                printf("线段不相交\n");
        }
        else
            printf("线段不相交\n");
 
    }
    return 0;
}
```

