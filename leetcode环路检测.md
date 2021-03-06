# leetcode环路检测（median）

参考自https://leetcode-cn.com/problems/linked-list-cycle-lcci/solution/kuai-man-zhi-zhen-zheng-ming-bi-jiao-yan-jin-by-ch/

题目：

给定一个链表，如果它是有环链表，实现一个算法返回环路的开头节点。  
有环链表的定义：在链表中某个节点的next元素指向在它前面出现过的节点，则表明该链表存在环路。

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

解题方法：快慢双指针

解题思路：慢指针每次走一步，快指针每次走2步，若有环，则必定会相遇，若无环，则快指针会先走到终点。通过判断快指针可以判断输入是否有环。当有环时，相遇的点不一定在环路的开头节点，计算环的开头
节点，首先我们知道快指针的行走距离是慢指针的两倍，将链表起点与环起点之间的距离设为m，环起点到相遇点的距离设为y，环的长度设为n，则我们可以知道快指针的行走路径为`(y+m)*2`，同时我们还可以
用另一种表达方式进行描述`m+y+xn`，x这里指快指针走过的圈数，并且x$\geq$1，于是我们可以得到：  
$(y+m)\*2 = m+y+x\*n$  
即 y+m = x\*n  
我们可以发现，当行走m距离时等于行走n-y的距离，也就是从链表的起点走到环起点等于环的相遇点走到环的起始点，通过这个思维，我们可以找到环的起始点，只需要在两点相遇时，将快指针移动到链表起点并每次行走与慢指针一样的步数，最终相遇的点就是环的起点

代码：
```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:

        if not head or not head.next:
            return 

        slow = head
        fast = head

        while fast and fast.next:

            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                break

        if not fast or not fast.next:
            return

        fast = head
        while slow != fast:
            slow = slow.next
            fast = fast.next
        
        return slow
```
