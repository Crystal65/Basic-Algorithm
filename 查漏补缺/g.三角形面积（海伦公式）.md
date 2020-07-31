# g.三角形面积（海伦公式）

## 题目

海伦公式： 利用三角形的三条边的边长直接求三角形面积的公式 。

## 思路

海伦公式：![](http://wenwen.soso.com/p/20110721/20110721122135-124871828.jpg)

## 代码

```C
#include<stdio.h> 
#include<math.h> 
int main()
{
	float a,b,c,p,area; 
	printf("请输入三角形的三边长\n"); 
	scanf("%f %f %f",&a,&b,&c); 
	p=1.0/2*(a+b+c);  
	if(a+b>c&&b+c>a&&a+c>b){   
	area=sqrt(p*(p-a)*(p-b)*(p-c)); 
	printf("三角形的面积为：%7.2f\n",area); 
} 
	else 
	printf("不能构成三角形\n");
	return 0;
}
```

