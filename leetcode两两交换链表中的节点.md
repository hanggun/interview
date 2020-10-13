# leetcode两两交换链表中的节点(median)

题目：

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。  
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

例子:

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

解题方法：递归

解题思路：将链表划分成子问题进行解决，在传输的一整个链表中，第一个节点为head,用newHead表示以第二个节点为头结点的链表，将第三个及之后交换好的节点连接到head.next，最后将head放置于newHead后面，即newHead.next=head则完成了两两交换。按照这个策略，从最后的节点开始进行交换，直到交换到开始节点结束。开始的过程如下图所示：

<img src="https://github.com/hanggun/interview/blob/master/leetcode%E5%9B%BE%E7%89%87/leetcode%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8.jpg" width = "400" height = "500" alt="" align=center />

代码：
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        newHead = head.next
        head.next = self.swapPairs(newHead.next)
        newHead.next = head

        return newHead
```
