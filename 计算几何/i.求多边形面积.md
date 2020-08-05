# i.求多边形面积

## 思路

- 任意多边形的面积可由任意一点与多边形上依次两点连线构成的三角形矢量面积求和得出。

​    **矢量面积=三角形两边矢量的叉乘**

​    如下图：

![](https://images0.cnblogs.com/blog/317502/201303/28225014-8a8e835f0a52475189c9f6c7beb5963b.png)

按定理，多边形面积由P点与A-G的各顶点连接所构成的三角形矢量面积构成，假定多边形顶点坐标顺序为A-G，逆时针为正方向，则有如下结论：

- PAB,PBC,PCD均为顺时针，面积为负；

- PDE，PEF，PFG，PGA均为逆时针，面积为正；

但无论正负，均可通过P点与顶点连线的矢量叉乘完成，叉乘结果中已包含面积的正负。

## 代码

```C++
#include<stdio.h>
#include<iostream>
#include<string.h>
#include<math.h> 
using namespace std;
struct node
{
	double x;
	double y;
}rect[10];
double det(struct node a,struct node b,struct node c)
{
	return (b.x-a.x)*(c.y-a.y)-(b.y-a.y)*(c.x-a.x);
}
int main()
{
	int n;
	while(cin>>n)
	{
		for(int  i=0;i<n;i++)
		{
			cin>>rect[i].x>>rect[i].y;
		}
		double ans=0;
		for(int i=1;i<n-1;i++)
		{
			ans+=det(rect[0],rect[i],rect[i+1]);
		}
		int ans1;
		ans1=(int)(0.5*fabs(ans)+0.5);            //四舍五入
		cout<<ans1<<endl;
	}
	return 0;
}
```



 