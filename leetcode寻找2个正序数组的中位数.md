### 寻找2个正序数组的中位数 （hard）

题目如下

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的中位数。要求解决此问题的时间复杂度为O(log(n+m))

示例

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

使用对数的时间复杂度可以用二分法。解题思路为：首先我们可以知道中位数所在位置，若整体数组的大小设为size，若size为奇数，则中位数在size//2+1,若size为偶数，则中位数为（size//2，size//2+1）。设中位数所在位置为k，即我们需要找到第k个值，那么我们每一次都只需从nums1和nums2中找k//2个值，找到较小的k//2个值之后，可以记录index，也可以将这k//2个值从list中给pop掉，将k减去已经找到的值之后，剩下所需要的找的k个值再进行上述的操作。

这里需要注意几个边界条件：

0. 当k//2大于nums1的长度或者nums2的长度，则使用nums1的长度或者nums2的长度
1. 当nums1为空集的时候，则nums2[k-1]为中位数  
2. 当nums2为空集的时候，则nums1[k-1]为中位数  
3. 当nums1和nums2都不为空集，并且k=1的时候，中位数为经过处理后的nums1[0]与nums2[0]中较小的一位

#### Code
递归
```
class Solution:
    def findMedianSortedArrays(self, nums1, nums2) -> float:

        def findKthSmallest(nums1, nums2, k):
            
            if not nums1:
                self.ans = nums2[k-1]
                return

            if not nums2:
                self.ans = nums1[k-1]
                return
            
            if k == 1:
                self.ans = min(nums1[0], nums2[0])
                return
            
            m = len(nums1)
            n = len(nums2)

            newindex1 = min(k//2-1, m-1)
            newindex2 = min(k//2-1, n-1)
            
            if nums1[newindex1] > nums2[newindex2]:
                [nums2.pop(0) for _ in range(newindex2+1)]
                findKthSmallest(nums1, nums2, k-newindex2-1)
            else:
                [nums1.pop(0) for _ in range(newindex1+1)]
                findKthSmallest(nums1, nums2, k-newindex1-1)
        
        size = len(nums1) + len(nums2)
        num1 = nums1.copy()
        num2 = nums2.copy()
        if size % 2 == 0:
            self.ans = 0
            findKthSmallest(num1, num2, size//2)
            a = self.ans
            num1 = nums1.copy()
            num2 = nums2.copy()
            self.ans = 0
            findKthSmallest(num1, num2, size//2+1)
            b = self.ans
            return (a+b)/2
        else:
            self.ans = 0
            findKthSmallest(nums1, nums2, size//2+1)
            return self.ans
```

迭代
```
class Solution:
    def findMedianSortedArrays(self, nums1, nums2) -> float:

        def findKthSmallest(k):

            m = len(nums1)
            n = len(nums2)
            index1 = 0
            index2 = 0
            while True:

                if index1 > m-1:
                    return nums2[index2 + k -1]
                if index2 > n-1:
                    return nums1[index1 + k -1]
                
                if k == 1:
                    return min(nums1[index1], nums2[index2])

                newindex1 = min(index1 + k//2-1, m-1)
                newindex2 = min(index2 + k//2-1, n-1)

                if nums1[newindex1] > nums2[newindex2]:
                    k -= newindex2 - index2 + 1
                    index2 += newindex2 - index2 + 1
                else:
                    k -= newindex1 -index1 + 1
                    index1 += newindex1 - index1 + 1

        size = len(nums1) + len(nums2)
        if size % 2 == 0:
            return (findKthSmallest(size // 2) + findKthSmallest(size//2+1)) / 2
        else:
            return findKthSmallest(size//2+1)
```
