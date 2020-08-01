# [和可被 K 整除的子数组](https://leetcode-cn.com/problems/subarray-sums-divisible-by-k/)

## 题目

给定一个整数数组 A，返回其中元素之和可被 K 整除的（连续、非空）子数组的数目。

示例：

- 输入：A = [4,5,0,-2,-3,1], K = 5
- 输出：7
- 解释：
  有 7 个子数组满足其元素之和可被 K = 5 整除：
  [4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]

## 思路

**哈希表+逐一统计**

通常，涉及连续子数组问题的时候，我们使用前缀和来解决。

我们令P[i]= A[0]+ A[1]+...+ A[i]。那么每个连续子数组的和sum(i, j)就可以写成P[j]- P[i- 1] (其中0< i< j)的形式。此时，判断子数组的和能否被K整除就等价于判断(P[j]- P[i- 1]) mod K ==0, 根据同余定理，只要P[j] mod K == P[i- 1] mod K,就可以保证上面的等式成立。

因此我们可以考虑对数组进行遍历，在遍历同时统计答案。当我们遍历到第i个元素时,我们统计以i结尾的符门合条件的子数组个数。我们可以维护一个以前缀和模K的值为键，出现次数为值的哈希表record,在遍历的同时进行更新。这样在计算以i结尾的符合条件的子数组个数时，根据上面的分析,答案即为[0..i - 1] 中前缀和模K也为P[i] mod K的位置个数，即record[P[i] mod K]。

最后的答案即为以每一个位置为数尾的符合条件的子数组个数之和。要注意的一个边界条件是，我们需要对哈希表初始化，记录record[0]= 1,这样就考虑了前缀和本身被K整除的情况。

注意:不同的语言负数取模的值不一定相同,有的语言焕数,对于这种情况需要特殊处理。

## 代码

### C

```C
int subarraysDivByK(int* A, int ASize, int K)
{
    int hash[K];
    memset(hash,0,sizeof(hash));
    for(int i=1;i<ASize;i++)//前缀和
    {
        A[i]=A[i-1]+A[i]; 
    } 
    for(int i=0;i<ASize;i++)//余数为键的哈希表
    {
       hash[(A[i]%K+K)%K]++;//消除负数
    }
    int sum=0;
    for(int i=0;i<K;i++)//统计相同余数满足要求的数目
    {
        sum+=hash[i]*(hash[i]-1)/2;
    }
    return sum+hash[0];//加上单个满足余数为零的数目
}
```

### C++

```C++
class Solution 
{
public:
    int subarraysDivByK(vector<int>& A, int K) 
    {
        unordered_map<int, int> record = {{0, 1}};
        int sum = 0, ans = 0;
        for (int elem: A) 
        {
            sum += elem;
            // 注意 C++ 取模的特殊性，当被除数为负数时取模结果为负数，需要纠正
            int modulus = (sum % K + K) % K;
            if (record.count(modulus)) 
            {
                ans += record[modulus];
            }
            ++record[modulus];
        }
        return ans;
    }
};
```



