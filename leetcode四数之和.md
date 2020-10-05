### leetcode四数之和（median）

题目：

给定一个包含 n 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

例子：

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

解题思路：迭代加双指针  
空间代价：使用4重循环进行枚举所需要的的时间复杂度为O($n^4$)，使用双指针可以将其中的一个双重循环变成单循环，也就是将O($n^2$)变成O($n$)，使用迭代加双指针所需要的时间复杂度为O($n^3$)。通过剪枝，我们可以让时间复杂度低于O($n^3$)。

这里我们的思路很简单，对数组进行排序后，首先确定第一个数，并对3种情况进行剪枝，这边我们需要注意，i的范围为[0,...,size-3]  
1. nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target （后面没有情况能够满足）  
2. nums[i] + nums[size-1] + nums[size-2] + nums[size-3] < target （加上最大的都无法满足，则直接往后面查找）  
3. nums[i] == nums[i-1] （若后面出现的等于前面已经使用过的，则直接跳过）

接着我们确定第二个数，并对3种情况进行剪枝，这边我们j的范围为[i+1,...,size-2]  
1. nums[i] + nums[j] + nums[j+1] + nums[j+2] > target
2. nums[i] + nums[j] + nums[size-1] + nums[size-2] < target
3. nums[j] == nums[j-1] 

确定好前面2个数之后，我们对最后的两个数进行双指针操作，令left = j+1, right = size-1
1. 若4数之和小于target， 则左指针向右移动
2. 若4数之后大于target， 则右指针向左移动
3. 若4数之后等于target， 则左右指针同时移动
4. 直到左指针大于等于右指针时，停止搜索

代码
```
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:

        if len(nums) < 4:
            return []

        #排序
        nums = sorted(nums)
        size = len(nums)
        ans = []
        temp = []
        #枚举
        
        for i in range(size-3):

            #先找第一个数，跳过重复的并剪枝
            if i >= 1 and nums[i] == nums[i-1]:
                continue
            if nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target:
                break
            if nums[i] + nums[size-1] + nums[size-2] + nums[size-3] < target:
                continue
            
            #找第二个数
            for j in range(i+1, size-2):

                if j >= i+2 and nums[j] == nums[j-1]:
                    continue
                if nums[i] + nums[j] + nums[j+1] + nums[j+2] > target:
                    break
                if nums[i] + nums[j] + nums[size-1] + nums[size-2] < target:
                    continue
                
                left = j+1
                right = size-1

                while left < right:
                    if nums[i] + nums[j] + nums[left] + nums[right] == target:
                        ans.append([nums[i],nums[j],nums[left],nums[right]])

                        while left <right and nums[left] == nums[left+1]:
                            left += 1
                        left += 1

                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        right -= 1
                    elif nums[i] + nums[j] + nums[left] + nums[right] < target:
                        left += 1
                    else:
                        right -= 1

        return ans
```
