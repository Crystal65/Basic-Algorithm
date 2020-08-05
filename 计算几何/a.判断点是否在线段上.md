# a.判断点是否在线段上

## 题目

给定一点Q(a,b),和线段M的首尾两个端点P1(X1,Y1),P2(X2,Y2),要求判断点Q否在线段M上。

## 思路

**向量法**

判断`Q-P1 == k * (P1-P2) ?Yes : No;`即可

**叉乘法**

判断` (Q-P1)×(P2-P1)==0?Yes:No;`即可

ps:**a**×**b**=|**a**||**b**|sin<**a**,**b**>

## 代码

**思路一**

```C
#include<stdio.h>
int main()
{
	double a,b,x1,x2,y1,y2;
	scanf("%lf%lf",&a,&b);          //Q(a,b)
	scanf("%lf%lf",&x1,&y1);        //P1(x1,y1)
	scanf("%lf%lf",&x2,&y2);        //P2(x2,y2)
	double s1=a-x1, t1=b-y1;        //Q-P1(s1,t1)
	double s2=x1-x2,t2=y1-y2;       //P1-P2(s2,t2)
	printf("%s\n",s1/s2==t1/t2?"Yes":"No");
	return 0;
}
```

**思路二**

```C
#include<stdio.h>
int main()
{
	double a,b,x1,x2,y1,y2;
	scanf("%lf%lf",&a,&b);          //Q(a,b)
	scanf("%lf%lf",&x1,&y1);        //P1(x1,y1)
	scanf("%lf%lf",&x2,&y2);        //P2(x2,y2)
	double s1=a-x1, t1=b-y1;        //Q-P1(s1,t1)
	double s2=x1-x2,t2=y1-y2;       //P1-P2(s2,t2)
	puts((s1*t2-t1*s2)==0?"Yes":"No");        //二维点的×乘x1*y2-x2*y1
	return 0;
}
```



