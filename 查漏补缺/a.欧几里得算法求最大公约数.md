# a.欧几里得算法求最大公约数

## 题目

欧几里德算法又称辗转相除法，用于计算两个正整数a，b的最大公约数。 

## 思路

![欧几里得算法图解](D:\Markdown\1.png)

## 代码

```C
#include <stdio.h>
int main()
{
	int a,b,m;		//m为余数
	printf("please enter a,b:");
	scanf("%d%d",&a,&b);
	m=a%b; 
	while(m!=0)		//若a<b,在下次计算会颠倒变成a>b
	{
		a=b;
		b=m;
		m=a%b;
	}
	printf("The greatest common divisor is %d\n",b);
	return 0; 
} 
```

