# c.判断矩形是否包含点

## 题目

判断该点的横坐标和纵坐标是否夹在矩形的左右边和上下边之间。

## 思路

判断一个点是否在两条线段之间夹着就转化成，判断一个点是否在某条线段的一边上，就可以利用叉乘的方向性，来判断夹角是否超过了180度

![](https://images2018.cnblogs.com/blog/449809/201807/449809-20180713175810432-796730532.png)

只要判断`(AB X AE ) * (CDX CE)  >= 0 `就说明E在AB,CD中间夹着，同理计算另两边DA和BC就可以了。

最后就是只需要判断

`(AB X AE ) * (CD X CE)  >= 0 && (DA X DE ) * (BC X BE) >= 0 `。

## 代码

```C++
//判断一个点是否在矩形内部
#include<cstdio>
#include<iostream>
 
struct Point
{
	float x;
	float y;
	Point(float x,float y)
	{
		this->x = x;
		this->y = y;
	}
};
// 计算 |p1 p2| X |p1 p|
float GetCross(Point& p1, Point& p2,Point& p)
{
	return (p2.x - p1.x) * (p.y - p1.y) -(p.x - p1.x) * (p2.y - p1.y);
}
//判断点是否在5X5的以原点为左下角的正方形内（便于测试）
bool IsPointInMatrix(Point& p)
{
	Point p1(0,5);
	Point p2(0,0);
	Point p3(5,0);
	Point p4(5,5);
 
	return GetCross(p1,p2,p) * GetCross(p3,p4,p) >= 0 && GetCross(p2,p3,p) * GetCross(p4,p1,p) >= 0;
	//return false;
}
using namespace std;
int main()
{
 
	while(true)
	{
		Point testPoint(0,0);
		cout << "enter the point :" << endl;
 
		cin >> testPoint.x >> testPoint.y;
 
		cout << "the point is  : "<< testPoint.x << " "<< testPoint.y << endl;
 
		cout << "the point is " << (IsPointInMatrix(testPoint)? "in the Matrix .": "not in the matrix ." )<< endl;
	}
	 
	return 0;
}
```

