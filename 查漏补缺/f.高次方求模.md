# f.高次方求模

## 题目

高次方求模： N mod m = [N1 mod m * N2 mod m * N3 mod m*…………*Nn mod m]mod m 

## 思路

 如2^101mod(3)时由于幂太高，无法直接运算，于是才用分割求 n^p=n^(p/2)* n^(p/2)递归分的过程后逐步求模。 

## 代码

```C
#include <stdio.h>
 
int powMOd(int n,int p)
{
    if (p == 1)
	{
		return n % M;
	}
	int temp = Pow(n, p/2);
	int result = (temp*temp) % M;		// 递归完到1经行余数相乘逐渐往回走。
	if (p % 2 == 1)				//当高次是奇数时。
	{
		result = (result*n) % M;
	}
	return result;
}
int main()
{
    int num,power,result;
    printf("数字，幂")；
    scanf("%d %d",&num,&power);
   	result = pwoMod(n,p);
	printf("模是%d",result)；
    return 0;
}
```

