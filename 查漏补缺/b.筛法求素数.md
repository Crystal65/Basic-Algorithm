# b.筛法求素数

## 题目

 用筛法求整数n内的所有素数 

## 思路

用筛法求素数的基本思想是:把从1开始的、某一范围内的正整数从小到大顺序排列， 1不是素数，首先把它筛掉。剩下的数中选择最小的数是素数，然后去掉它的倍数。依次类推，直到筛子为空时结束。 

## 代码

### C

```C
#include<stdio.h>
#include<math.h>

int judge(int N)
{   
 int i,flag;    //设置标志
 for(i=2;i<=sqrt(N);i++)
 {
  if(N%i==0 )   //从2~N的开方之间如果有数能整除，则说明不是素数
  {
   flag=0;
   break;
  }
  else        //否则是素数
   flag=1;
 }
 return flag;
}

int main()
{  
 int i,N;
 scanf("%d",&N);	//判断 N以内的素数 
 for(i=2;i<=N;i++)   //从2开始
 {  
  if(judge(i))
   printf("%d\n",i);
 }
 return 0;
}
```

### C++

```C++
#include<iostream>
#include<cmath>

using namespace std;
int main()
{
    int N; 
	  cin>>N;
    int *Location=new int[N+1];
    for(int i=0;i!=N+1;++i)
        Location[i]=i;
    Location[1]=0; //筛除部分 
    int p,q,end;
    end=sqrt((double)N)+1;
    for(p=2;p!=end;++p)
    {
        if(Location[p])
        {
            for(q=p;p*q<=N;++q)
            {
                for(int k=p*q;k<=N;k*=p)
                    Location[k]=0;
            }
        }
    }

    int m=0;
    for(int i=1;i!=N+1;++i)
    {
        if(Location[i]!=0)
        {
            cout<<Location[i]<<"	";
            ++m;
        }
        if(m%10==0) cout<<endl;
    }
    cout<<endl<<"一共有"<<m<<"个素数"<<endl;
    return 0;
}
```





