# [合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

## 题目

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

- 示例:

  输入:
  [
    1->4->5,
    1->3->4,
    2->6
  ]
  输出: 1->1->2->3->4->4->5->6

## 思路

**前置知识：合并两个有序链表**

- 首先我们需要一个变量 head 来保存合并之后链表的头部，你可以把 head 设置为一个虚拟的头（也就是 head 的 val 属性不保存任何值），这是为了方便代码的书写，在整个链表合并完之后，返回它的下一位置即可。
- 我们需要一个指针 tail 来记录下一个插入位置的前一个位置，以及两个指针 aPtr 和 bPtr 来记录 a 和 b 未合并部分的第一位。注意这里的描述，tail 不是下一个插入的位置，aPtr 和 bPtr 所指向的元素处于「待合并」的状态，也就是说它们还没有合并入最终的链表。 当然你也可以给他们赋予其他的定义，但是定义不同实现就会不同。
- 当 aPtr 和 bPtr 都不为空的时候，取 val 熟悉较小的合并；如果 aPtr 为空，则把整个 bPtr 以及后面的元素全部合并；bPtr 为空时同理。
- 在合并的时候，应该先调整 tail 的 next 属性，再后移 tail 和 *Ptr（aPtr 或者 bPtr）。那么这里 tail 和 *Ptr 是否存在先后顺序呢？它们谁先动谁后动都是一样的，不会改变任何元素的 next 指针。

```C++
ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
    if ((!a) || (!b)) return a ? a : b;
    ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
    while (aPtr && bPtr) {
        if (aPtr->val < bPtr->val) {
            tail->next = aPtr; aPtr = aPtr->next;
        } else {
            tail->next = bPtr; bPtr = bPtr->next;
        }
        tail = tail->next;
    }
    tail->next = (aPtr ? aPtr : bPtr);
    return head.next;
}
```

**分治合并**

- 将 k 个链表配对并将同一对中的链表合并；
- 第一轮合并以后， k 个链表被合并成了 k/2个链表，平均长度为 2n/k，然后是k/4个链表，k/8个链表等等；
- 重复这一过程，直到我们得到了最终的有序链表。

![](https://pic.leetcode-cn.com/6f70a6649d2192cf32af68500915d84b476aa34ec899f98766c038fc9cc54662-image.png)

## 代码

```C
void swap(struct ListNode **tree, int i, int j) {
    struct ListNode *tmp = tree[i];
    tree[i] = tree[j];
    tree[j] = tmp;
}

void heapify(struct ListNode **tree, int n, int i) {
    /* 本函数从上到下依次将最大值下沉，即最小堆实现 */
    if (i >= n) return;  // 叶子节点超界
    int minIdx = i;  // 最小节点索引
    int left = 2 * i + 1, right = 2 * i + 2;  // 左右孩子节点索引
    if (left < n && tree[left]->val < tree[minIdx]->val) {
        minIdx = left;
    }
    if (right < n && tree[right]->val < tree[minIdx]->val) {
        minIdx = right;
    }
    if (minIdx != i) {
        /* 如果当前节点不是最小堆，交换然后依次递归下去构建最小堆 */
        swap(tree, i, minIdx);
        heapify(tree, n, minIdx);  // 注意传参是minIdx继续整理堆
    }
}

struct ListNode* mergeKLists(struct ListNode** lists, int listsSize){
    // 1.特判空列表
    if (listsSize < 1 || lists == NULL) return NULL;  // [[]]不能判断
    // 2.构建最小堆实现
    struct ListNode **tree = (struct ListNode**)malloc(sizeof(struct ListNode*) * listsSize);
    for (int i = 0, j = listsSize, k = 0; i < j; i++) {
        if (!lists[i]) listsSize--;  // NULL
        else tree[k++] = lists[i];
    }
    if (listsSize <= 0) return NULL;
    int idx = ((listsSize - 1) - 1) / 2;  // 获取最后一个叶子节点下标
    for (; idx >= 0; idx--) heapify(tree, listsSize, idx);
    /* 3.堆进行维护，每次“替换”最小值
    * 如果空表，移到最后对剩下元素维护堆，直到最后堆的大小为0结束。
    */
    struct ListNode *dummy = (struct ListNode*)malloc(sizeof(struct ListNode));
    struct ListNode *prev = dummy, *curr;
    while (listsSize > 0) {
        /* 每次从堆顶取最小元素的节点出来，然后构建链表后整理堆
        * 1.如果当前堆顶元素无后续节点，那么将其与最后idx交换，下次整理堆数量-1
        * 2.如果当前堆顶元素有后继节点，那么继续构建表后整理堆。
        * 整理堆永远是从0号位置开始，只是整理数量listsSize下降而已。
        */
        curr = tree[0];
        if (curr->next == NULL) {
            swap(tree, listsSize - 1, 0);  // 与最后一位交换
            listsSize--;  // 下次整理堆数量减少1
        } else tree[0] = tree[0]->next;
        prev->next = curr;
        prev = curr;
        heapify(tree, listsSize, 0);
    }
    return dummy->next;
}
```



```C++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* merge(vector <ListNode*> &lists, int l, int r) {
        if (l == r) return lists[l];
        if (l > r) return nullptr;
        int mid = (l + r) >> 1;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return merge(lists, 0, lists.size() - 1);
    }
};
```

