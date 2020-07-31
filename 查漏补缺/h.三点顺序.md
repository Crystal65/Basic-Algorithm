# h.三点顺序

## 题目

现在给你不共线的三个点A,B,C的坐标，它们一定能组成一个三角形，现在让你判断A，B，C是顺时针给出的还是逆时针给出的？ 

**输入**

每行是一组测试数据，有6个整数x1,y1,x2,y2,x3,y3分别表示A,B,C三个点的横纵坐标。（坐标值都在0到10000之间）

输入0 0 0 0 0 0表示输入结束

**输出**

如果这三个点是顺时针给出的，输出1，逆时针给出则输出0

## 思路

- 利用矢量叉积判断是逆时针还是顺时针：
   设A(x1,y1),B(x2,y2),C(x3,y3),则三角形两边的矢量分别是：
   AB=(x2-x1,y2-y1), AC=(x3-x1,y3-y1)
   则AB和AC的叉积为：(2*2的行列式)
   |x2-x1, y2-y1|
   |x3-x1, y3-y1|*

  *值为：(x2-x1)*(y3-y1) - (y2-y1)*(x3-x1)

-  利用右手法则进行判断：
   如果ABxAC>0，则三角形ABC是逆时针的；
   如果ABxAC<0，则三角形ABC是顺时针的；
   如果ABxAC=0，则说明三点共线。

## 代码

### C

```C
#include<stdio.h>  
int main()  
{  
    int x1,y1,x2,y2,x3,y3;  
    while(~scanf("%d%d%d%d%d%d",&x1,&y1,&x2,&y2,&x3,&y3)&&(x1+y1+x2+y2+x3+y3))  
    {  
        if((x2-x1)*(y3-y1)-(x3-x1)*(y2-y1)>0)  
            printf("0\n"); //逆时针  
        else   
            printf("1\n");  //顺时针  
    }  
    return 0;  
}  
```

### C++

```C++
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<iostream>
#include<algorithm>
#include<cmath>
#include<ctime>
#include<queue>
#include<stack>
#include<vector>
using namespace std;
//#include<windows.h>
//#include<conio.h>
#define max(a,b) a>b?a:b
#define min(a,b) a>b?b:a
#define mem(a,b) memset(a,b,sizeof(a))
#define malloc(sb) (sb *)malloc(sizeof(sb))
//int dir[4][2]= {{0,1},{0,-1},{1,0},{-1,0}};
int main()
{
 
    int x1,y1,x2,y2,x3,y3;
    int s1,s11,s2,s22,s3,s33,Sum;
    int cos,sum;
    while(~scanf("%d%d%d%d%d%d",&x1,&y1,&x2,&y2,&x3,&y3),x1+y1+x2+y2+x3+y3)
    {
        s1=x2-x1;
        s11=y2-y1;
        s2=x3-x1;
        s22=y3-y1;
       cos=(s1*s22)-(s11*s2);
    printf("%d\n",cos<0?1:0);
    }
}
```



