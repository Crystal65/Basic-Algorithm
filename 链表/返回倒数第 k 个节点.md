# [返回倒数第 k 个节点](https://leetcode-cn.com/problems/kth-node-from-end-of-list-lcci/)

## 题目

实现一种算法，找出单向链表中倒数第 k 个节点。返回该节点的值。

注意：本题相对原题稍作改动

- 示例：

  输入： 1->2->3->4->5 和 k = 2
  输出： 4

说明：给定的 k 保证是有效的。

## 思路

1、先让t向前走K步；
2、head和t同步前进，t到结尾，head到目标。

**双指针**

反向思考，既然是寻找倒数第K个，那么计算机只能循环后移，不如我们先将位置确定，让其同步后移到链尾。
设置前后指针都先指向头结点，后指针先移动到第K个结点，那么前后指针此时相距K个位置。同步后移，当后指针指向链尾时，前指针就自然指向倒数第K个结点

## 代码

```C
int kthToLast(struct ListNode* head, int k){
    	struct ListNode*t = head;
	while (k--){
		t = t->next;
	}
	while (t){
		t = t->next;
		head = head->next;
	}
	return head->val;
}
```



```C++
class Solution {
public:
    int kthToLast(ListNode* head, int k) {
        if(head == NULL)            //常规判断
            return 0;
        ListNode *pre = head;       //前指针
        ListNode *p = head;         //后指针    
        int i = 0;              //i用来确保后指针移动到第K个位置
        while(i < k){
            if(p == NULL)       
                return 0;
            p = p->next;
            i++;
        }
        while(p != NULL){       //相距K个，同步移动，直到后指针到链表尾部。
            pre = pre->next;
            p = p->next;
        }
        return pre->val;
    }
};
```

