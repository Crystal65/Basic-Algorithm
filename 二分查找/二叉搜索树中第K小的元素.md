# [二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

## 题目

给定一个二叉搜索树，编写一个函数 kthSmallest 来查找其中第 k 个最小的元素。

说明：
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

- 示例 1:

  输入: root = [3,1,4,null,2], k = 1
     3
    / \
   1   4
    \
     2
  输出: 1

- 示例 2:

  输入: root = [5,3,6,2,4,null,null,1], k = 3
         5
        / \
       3   6
      / \
     2   4
    /
   1
  输出: 3

## 思路

当前节点的左子树的所有节点cnt+1==k的话，说明第k小的元素即是当前节点root->val,而k>cnt+1的话说明，第K小的元素是在root节点的右儿子那边，则把k-cnt-1，继续二分即可。
即所谓的二分是指以当前节点左子树结点cnt与第k个结点的关系进行二分，小于则在左边，大于，则在右边。

## 代码

```C
int numofRoot(struct TreeNode* root)
{
    if(!root) return 0;
    return 1+numofRoot(root->left)+numofRoot(root->right);
}

int kthSmallest(struct TreeNode* root, int k){
    int cnt=numofRoot(root->left);

    if(k<=cnt) return kthSmallest(root->left,k);
    else if(k>cnt+1) return kthSmallest(root->right,k-cnt-1);
    return root->val;
}
```





