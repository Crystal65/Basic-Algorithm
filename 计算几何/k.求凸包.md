# k.求凸包

## 题目

凸包指的是一个点集中的最小凸多边形，且其包含了所有点集内的点；简单地说，就是点集最外侧的点构成的凸多边形。

![](https://p1.ssl.qhimg.com/t0146eaa77650fca8cc.png)

<center>外围的蓝色线即为凸包

## 思路

1、**卷包裹法**：

每次找出极角最小的边，通过它到达下一点，重复这一过程直到回到原点；具体过程如下：

（1）找出左下角的点（以横纵坐标为第一第二关键字，可以找x最小的，x相同则找y最小的），显然这个点一定为凸包上的一个顶点；

（2）以（1）中找出的点为原点，选出任意一条边（起点为原点），与所有其它边进行对比直至找到极角最小的边（通过叉乘比较，详见计算几何板块）；

（3）通过该边找到下一个点，重复（2）中过程，直到回到原点，这个序列就是凸包序列。

效率分析：每找到一个点，我们都要枚举一遍其他所有点，因此效率是![O(n^{2})](https://private.codecogs.com/gif.latex?O%28n%5E%7B2%7D%29)。

2、**Graham-Scan算法**：

以左下角的点为原点，按逆时针将边排序，用栈存凸包；过程如下：

（1）找出左下角的点；

（2）以（1）中找出的点作为原点，将其余点逆时针排序（即极角按从小到大排序）；

（3）将原点以及排序后第一个点压入栈内；

（4）设栈顶为top，按排序后的顺序逆时针枚举其他所有点；在考虑某个点时，要用类似单调队列的方法，不断比较该点与栈中top-1连的边和栈顶的点与栈中top-1连的边，若其更靠外侧（即叉乘小于0），则将栈顶元素弹出栈（top--），直到其更靠内侧，然后把该点压入栈。

效率分析：排序的时间复杂度为![O(nlogn)](https://private.codecogs.com/gif.latex?O%28nlogn%29)，因为每个点最多入栈一次，所以效率为![O(n+nlogn)](https://private.codecogs.com/gif.latex?O%28n&plus;nlogn%29)。

## 代码

**卷包裹法**

```C++
int dist(int x1,int y1,int x2,int y2) //求距离
{
    return int(pow(double(x2-x1),2)+pow(double(y2-y1),2));
}
 
int mul_cross(int x1,int y1,int x2,int y2) //求叉乘
 
{
    return(x1*y2-x2*y1);
}
void find_1() //找左下角的点
{
    x[0]=2147483647;
    y[0]=2147483647;
    root=0;
    for (int i=1;i<=n;i++)
        if (x[i]<x[root]||(x[i]==x[root]&&y[i]<y[root]))
            root=i;
}
void find_2() //找凸包
{
    int last=root;
    while (true)
    {
        a[++tot]=last;
        int now=1;
        if (now==last) now++;
        for (int i=1;i<=n;i++)
            if (last!=i&&mul_cross(x[now]-x[last],y[now]-y[last],x[i]-x[last],y[i]-y[last])<0||(mul_cross(x[now]-x[last],y[now]-y[last],x[i]-x[last],y[i]-y[last])==0&&dist(x[i],y[i],x[last],y[last])<dist(x[now],y[now],x[last],y[last]))
                now=i;
        last=now;
        if (last==root) break;
    }
}
```

**Graham-Scan算法**

```C++
int multiplication_cross(int x1,int y1,int x2,int y2) //求叉乘
{
    return (x1*y2-x2*y1);
}
void swap(int i,int j) //交换，把左下角的点放在数组第一位
{
    int k=x[i];
    x[i]=x[j];
    x[j]=k;
    k=y[i];
    y[i]=y[j];
    y[j]=k;
}
double distance_self(int i,int j) //求两点间距离
{
    return sqrt(pow(double(x[i]-x[j]),2)+pow(double(y[i]-y[j]),2));
}
void qs(int l,int r) //排序
{
    int i=l;
    int j=r;
    int mx=x[(l+r)/2];
    int my=y[(l+r)/2];
    while (i<=j)
    {
        while ((multiplication_cross(x[i]-x[1],y[i]-y[1],mx-x[1],my-y[1])>0||(multiplication_cross(x[i]-x[1],y[i]-y[1],mx-x[1],my-y[1])==0&&distance_self(1,i)<sqrt(double((mx-x[1])*(mx-x[1])+(my-y[1])*(my-y[1])))))&&(i<n)) i++;
        while ((multiplication_cross(x[j]-x[1],y[j]-y[1],mx-x[1],my-y[1])<0||(multiplication_cross(x[j]-x[1],y[j]-y[1],mx-x[1],my-y[1])==0&&sqrt(double((mx-x[1])*(mx-x[1])+(my-y[1])*(my-y[1])))<distance_self(1,j)))&&(j>1)) j--;
        if (i<=j)
        {
            swap(i,j);
            i++;
            j--;
        }
    }
    if (i<r) qs(i,r);
    if (j>l) qs(l,j);
}
void find_1() //找左下角的点
{
    root=0;
    x[0]=2147483647;
    y[0]=2147483647;
    for (int i=1;i<=n;i++)
        if (x[i]<x[root]||(x[i]==x[root]&&y[i]<y[root])) root=i;
    swap(1,root);
}
void find_2() //求凸包
{
    for (int i=3;i<=n;i++)
    {
        while (multiplication_cross(x[stack[top]]-x[stack[top-1]],y[stack[top]]-y[stack[top-1]],x[i]-x[stack[top-1]],y[i]-y[stack[top-1]])<0) top--;
        stack[++top]=i;
    }
}
```

