# leetcode重排链表(median)

题目：

给定一个单链表 L：L0→L1→…→Ln-1→Ln ，  
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…  
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。  

示例：
```
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```

解题思路：首先将链表按中点分成前后两个部分，然后将后续链表倒序后，与前链表进行交替连接

链表分割：使用快慢指针，慢指针每次走一格，快指针每次走两格，当快指针走到终点（走下去为null时），慢指针刚好走到中点

链表倒序：建立以下pre指针，建立一个curr指针指向原列表的头结点，然后在循环中，每次将curr指针指向pre，令curr指针向后移一位，令pre指针向后移一位，直到curr走到null

代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head or not head.next:
            return head
        mid = self.middleNode(head)
        second = mid.next
        mid.next = None
        second = self.reverseNode(second)
        return self.mergeNode(head, second)
    
    def middleNode(self, head):

        slow = fast = head
        while fast.next and fast.next.next:
            slow, fast = slow.next, fast.next.next

        return slow
    
    def reverseNode(self, head):

        pre = None
        while head:
            curr = head
            head = head.next
            curr.next = pre
            pre = curr
        return pre
    
    def mergeNode(self, first, second):

        head = first

        while second:
            firsttemp = first.next
            secondtemp = second.next

            first.next = second
            first = firsttemp

            second.next = first
            second = secondtemp

        if first:
            first.next = second
        


```
