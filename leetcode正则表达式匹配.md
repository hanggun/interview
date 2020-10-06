### leetcode正则表达式匹配 (hard)

题目：

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```

示例：

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```

解题方法：动态规划

解题思路：  
建立dp[i][j]，dp[i][j]代表s的前i个字符能否与p的前j个字符匹配，这里我们以p[j]是否为'*'，将问题分为2种情况

状态转移方程：

当p[j] != '*' (即p[j]为数字时)  
>dp[i][j] = dp[i-1][j-1] if i != 0 and (s[i] = p[j] or p[j-1] = '.') (i != 0 意味着任何字符与空字符匹配都为False)

当p[j] = '*'时  
>dp[i][j] = dp[i][j-2] （匹配0次）  
>dp[i][j] = dp[i-1][j] if i != 0 and (s[i] = p[j-1] or p[j-1] = '.') （匹配1次以上）

代码：
```
class Solution:
    def isMatch(self, s: str, p: str) -> bool:

        m = len(s)
        n = len(p)
        
        dp = [[False for i in range(n+1)] for j in range(m+1)]
        
        dp[0][0] = True
        
        for i in range(m+1):
            for j in range(1,n+1):

                if p[j-1] != '*':
                    if i != 0:
                        if (s[i-1] == p[j-1] or p[j-1] == '.'):
                            dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = dp[i][j-2]
                    if i != 0:
                        if s[i-1] == p[j-2] or p[j-2] == '.':
                            dp[i][j] |= dp[i-1][j]

        return dp[m][n]
```
