# [最小K个数](https://leetcode-cn.com/problems/smallest-k-lcci/)

## 题目

设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

- 示例：

  输入： arr = [1,3,5,7,2,4,6,8], k = 4
  输出： [1,2,3,4]

## 思路

建立最小二叉堆，然后依次输出即可。

## 代码

```C
void Adjust(int *nums,int k,int len)
 {
     int temp=nums[k];
     int i;
     for(i=2*(k+1);i<=len+1;i*=2)
     {
         if(i<len&&nums[i-1]>nums[i])
         i++;//我写这行代码的时候上帝和我知道，现在只有上帝知道了
         if(temp<=nums[i-1])break;
         else
         {
             nums[k]=nums[i-1];
             k=i-1;
         }
     }
     nums[k]=temp;
 }
 void CreateMax(int *nums,int length)//建立小顶堆
 {
     int i;
     for(i=(length-1)/2;i>=0;i--)
        Adjust(nums,i,length-1);
 }
int* smallestK(int* arr, int arrSize, int k, int* returnSize){
    *returnSize=arrSize;
    if(arrSize==0)
    return arr;
    CreateMax(arr,arrSize);//创建小顶堆
    //for number[]
    int *number=(int *)malloc(sizeof(int)*arrSize);
    int index=0;

    while(k>0)
    {
        number[index++]=arr[0];
        arr[0]=arr[arrSize-index];
        Adjust(arr,0,arrSize-index);
        k--;
    }
    *returnSize=index;
    return number;
}
```

