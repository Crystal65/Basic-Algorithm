# [螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

## 题目

给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

- 示例:

  输入: 3
  输出:
  [
   [ 1, 2, 3 ],
   [ 8, 9, 4 ],
   [ 7, 6, 5 ]
  ]

## 思路

- 生成一个 n×n 空矩阵 mat，随后模拟整个向内环绕的填入过程：

  - 定义当前左右上下边界 l,r,t,b，初始值 num = 1，迭代终止值 tar = n * n；

  - 当 num <= tar 时，始终按照 从左到右 从上到下 从右到左 从下到上 填入顺序循环，每次填入后：

    - 执行 num += 1：得到下一个需要填入的数字；

    - 更新边界：例如从左到右填完后，上边界 t += 1，相当于上边界向内缩 1。

  - 使用num <= tar而不是l < r || t < b作为迭代条件，是为了解决当n为奇数时，矩阵中心数字无法在迭代过程中被填充的问题。
- 最终返回 mat 即可。

![](https://pic.leetcode-cn.com/ccff416fa39887c938d36fec8e490e1861813d3bba7836eda941426f13420759-Picture1.png)

## 代码

### C

```C
int** generateMatrix(int n, int* returnSize, int** returnColumnSizes){
    int l = 0;
    int r = n - 1;
    int t = 0;
    int b = n - 1;
    int** mat = (int**)malloc(sizeof(int*) * n);
    *returnSize = n;
    *returnColumnSizes = (int*)malloc(sizeof(int) * n);
    for (int i = 0; i < n; i++) {
        mat[i] = (int*)malloc(sizeof(int) * n);
        (*returnColumnSizes)[i] = n;
    }
    int num = 1;
    int tar = n * n;
    
    while(num <= tar){
        for(int i = l; i <= r; i++) mat[t][i] = num++; // left to right.
        t++;
        for(int i = t; i <= b; i++) mat[i][r] = num++; // top to bottom.
        r--;
        for(int i = r; i >= l; i--) mat[b][i] = num++; // right to left.
        b--;
        for(int i = b; i >= t; i--) mat[i][l] = num++; // bottom to top.
        l++;
    }

    return mat;
}
```

### Java

```Java
class Solution {
    public int[][] generateMatrix(int n) {
        int l = 0, r = n - 1, t = 0, b = n - 1;
        int[][] mat = new int[n][n];
        int num = 1, tar = n * n;
        while(num <= tar){
            for(int i = l; i <= r; i++) mat[t][i] = num++; // left to right.
            t++;
            for(int i = t; i <= b; i++) mat[i][r] = num++; // top to bottom.
            r--;
            for(int i = r; i >= l; i--) mat[b][i] = num++; // right to left.
            b--;
            for(int i = b; i >= t; i--) mat[i][l] = num++; // bottom to top.
            l++;
        }
        return mat;
    }
}

```

