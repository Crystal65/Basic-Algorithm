# 选择排序（Selection sort）

## 简介

选择排序是一种简单直观的排序算法。它的工作原理是每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，直到全部待排序的数据元素排完。 选择排序是不稳定的排序方法（比如序列[5， 5， 3]第一次就将第一个[5]与[3]交换，导致第一个5挪动到第二个5后面）。 
排序算法即解决以下问题的算法：

##### 输入

n个数的序列⟨a1,a2,a3,...,an⟩。

##### 输出

原序列的一个重排⟨a1∗,a2∗,a3∗,...,an∗⟩，使得a1∗<=a2∗<=a3∗<=...<=an∗a1∗<=a2∗<=a3∗<=...<=an∗ 

## 原理

n个记录的文件的直接选择排序可经过n−1趟直接选择排序得到有序结果： 
①初始状态：无序区为R[1..n]，有序区为空。 
②第1趟排序 
在无序区R[1..n]中选出关键字最小的记录R[k]，将它与无序区的第1个记录R[1]、交换，使R[1..1]和R[2..n]分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区。 
…… 
③第i趟排序 
第i趟排序开始时，当前有序区和无序区分别为R[1..i−1]和R(i..n）。该趟排序从当前无序区中选出关键字最小的记录 R[k]，将它与无序区的第1个记录R交换，使R[1..i]和R分别变为记录个数增加1个的新有序区和记录个数减少1个的新无序区。

##### 通俗的解释

对比数组中前一个元素跟后一个元素的大小，如果后面的元素比前面的元素小则用一个变量k来记住他的位置，接着第二次比较，前面“后一个元素”现变成了“前一个元素”，继续跟他的“后一个元素”进行比较如果后面的元素比他要小则用变量k记住它在数组中的位置(下标)，等到循环结束的时候，我们应该找到了最小的那个数的下标了，然后进行判断，如果这个元素的下标不是第一个元素的下标，就让第一个元素跟他交换一下值，这样就找到整个数组中最小的数了。然后找到数组中第二小的数，让他跟数组中第二个元素交换一下值，以此类推。

## 代码

```C
//选择排序(从小到大排)
#include<stdio.h>
int number[100000000];    //在主函数外定义数组可以更长多了 
void select_sort(int R[],int n)    //定义选择排序函数"select_sort" 
{
    int i,j,k,index;    //定义变量 
    for(i=0;i<n-1;i++)   //遍历 
    {
        k=i;
        for(j=i+1;j<n;j++)    //j初始不为0，冒泡初始为0，所以选排比冒泡快，但不稳定 
        {
            if(R[j]<R[k])   //顺序从这里改顺序 小到大"<",大到小">" ！！！
                k=j;      //这里是区分冒泡排序与选择排序的地方，冒泡没这句 
        }
        if(k!=j)    //为了严谨，去掉也行 
        {
            index=R[i];   //交换R[i]与R[k]中的数 
            R[i]=R[k];    //简单的交换c=a,a=b,b=c 
            R[k]=index;
        }
    }
} 

int main()     //主程序 
{
    int i,n;
    printf("输入数字个数：\n");    
    scanf("%d",&n);     //输入要排序的数字的个数 
    printf("输入%d个数:\n",n);
    for(int j=0;j<n;j++)     //将所有数全放入number数组中 
        scanf("%d",&number[j]) ;
    select_sort(number,n);   //引用选择排序select_sort的函数 
    for (i=0;i<n-1;i++)    //用for循环输出排完排完序的数组 
        printf("%d ",number[i]);   //这样写是为了格式（最后一个数后面不能有空格）                                  
    printf("%d\n",number[i]);
    return 0;   //好习惯 
}
```

