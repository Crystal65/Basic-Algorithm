# [在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

## 题目

统计一个数字在排序数组中出现的次数。

- 示例 1:

  输入: nums = [5,7,7,8,8,10], target = 8
  输出: 2

- 示例 2:

  输入: nums = [5,7,7,8,8,10], target = 6
  输出: 0

## 思路

使用二分查找模板，找到排序数组中等于target的元素的索引。
然后基于这个索引，向左遍历得到左边等于target的元素个数，向右遍历得到右边等于target的元素个数。

## 代码

```C
int search(int* nums, int numsSize, int target){
    int left = 0, right = numsSize-1, mid, cnt = 0;
    while(left <= right){
        mid = (right + left)>>1;
        if(nums[mid] == target){
            for(int i=mid-1; i>=0 && nums[i]==target; i--){  // 向左查找target
                cnt++;
            }
            for(int i=mid; i<numsSize && nums[i]==target; i++){  // 向右查找target
                cnt++;
            }
            break;
        }else if(nums[mid] < target){
            left = mid + 1;
        }else{
            right = mid - 1;
        }
    }
    return cnt;
}
```

