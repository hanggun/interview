### leetcode最长回文子串（median）

题目：

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

例子：

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

解题思路1：使用动态规划，这里我们可以发现，当我们从第一个开始往后搜索的时候，当前位置dp[i][j] = dp[i+1][j-1] and s[i] == s[j]，也就是说，要使字符串s[i:j+1]为回文子串，那么s[i+1:j]
得是回文子串并且两边的字符得是相同的。根据这个思想，我们可以将大问题转化为一个一个字符子串的小问题来解决。

在初始化的时候，我们可以设置  
1. dp[i][i] = True 当只有一个字符串的时候，是回文子串
2. dp[i][i+1] = s[i] == s[i+1] 当只有2个字符串的时候，当这2个字符相同时，是回文子串

在动态规划的搜索顺序上，采用了从下往上搜索的方法

代码：
```
class Solution:
    def longestPalindrome(self, s: str) -> str:

        n = len(s)

        if not s:
            return 
            
        dp = [[False for i in range(n)] for j in range(n)]
        ans = s[0]
        for i in range(n):
            dp[i][i] = True
            if i != n-1:

                if s[i] == s[i+1]:
                    dp[i][i+1] = True
                    ans = s[i:i+2]

        for i in range(n-3, -1, -1):
            for j in range(i+2, n):
                if dp[i+1][j-1] and s[i] == s[j]:
                    dp[i][j] = True

                    if j-i+1 > len(ans):
                        ans = s[i:j+1]

        return ans
```

解题思路2：中心扩散思路  
由于我们知道dp[i][j] = dp[i+1][j-1] and s[i] == s[j] = ... 我们可以从中心开始搜索，直到外围s[i] != s[j]为止停止

执行思路  
1. 以当前位置(i,i)为中心位置，若满足s[left] = s[right]并且序号不超过边界，则向左右搜索
2. 中心子回串有2种可能，一种是单个字符，一种是2个字符，因此，将(i,i+1)也作为中心位置，进行1的操作
3. 最后只需要比较(i,i)与(i,i+1)2个中心中，记录长度最长的回文子串即可

代码
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        start = 0
        end = 0

        def dfs(start, end):
            while start > -1 and end < len(s) and s[start] == s[end]:
                start -= 1
                end += 1
            
            return start+1, end-1

        for i in range(len(s)):
            left1, right1 = dfs(i,i)
            left2, right2 = dfs(i,i+1)

            if right1 - left1 > end - start:
                start = left1
                end = right1

            if right2 - left2 > end - start:
                start = left2
                end = right2
            
        return s[start:end+1]
```
