# d.逆康托展开

## 题目

 逆康托展开：求在一个全排列中，从小到大的第n个全排列是多少 

## 思路

一开始已经提过了，康托展开是一个全排列到一个自然数的**双射**，因此是可逆的。

举个例子，4132是4的全排列中编号为20的数
20得到4132的过程如下：

- 用 19/3!=3余1，说明比首位小的数有3个，所以首位为4，余数继续辗转。

- 用 1/2!=0余1，说明在第二位之后小于第二位的数有0个，所以第二位为1，余数继续辗转。

- 用 1/1!=1余0，说明在第三位之后小于第三位的数有1个，所以第三位为3，余数继续辗转。

- 用 0/0!=0余0，说明在第四位之后小于第四位的数有0个，所以第四位为2，结束n次循环。

## 代码

```C
#include<stdio.h>
#define Max 4
const int fact[10] = { 1,1,2,6,24,120,720,5040,40320,362880 };//给出10以内的阶乘 

void revContor(int a[] , int rank) 
{
	int value, i,flag = 0;
	int b[Max+1];
	rank -= 1;
	for (i = 1; i<Max; i++) 
	{
		value = rank / fact[Max - i];
		rank = rank % fact[Max - i];
		flag = value + 1;
		for (value = 1; value <= Max; value++) 
		{
			if (a[value] != 0)
				flag--;
			if (flag == 0)
				break;
		}
		b[i] = a[value] ;
		a[value] = 0;
	}
	for (value = 1; value <= Max; value++) 
	{
		if (a[value] != 0) 
		{
			b[i] = a[value];
			for (i = 1; i <= Max; i++) 
			{
				printf("%5d", b[i]);
			}
			return ;
		}
	}
}


int main() 
{
	int i;
	int  arrange[Max + 1]; //先定义一个四以内排列,并且忽略0位 
	int  rank = 0;  //排列的序号；

	for (i = 0; i <= Max; i++) 
	{
		arrange[i] = i;
	}
	scanf("%d", &rank);
	revContor(arrange, rank);
	
	return 0;
}
```






