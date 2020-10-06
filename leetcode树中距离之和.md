### leetcode树中距离之和 （hard）

题目：

给定一个无向、连通的树。树中有 N 个标记为 0...N-1 的节点以及 N-1 条边 。  
第 i 条边连接节点 edges[i][0] 和 edges[i][1] 。  
返回一个表示节点 i 与其他所有节点距离之和的列表 ans。

例子：
```
输入: N = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
输出: [8,12,6,10,10,10]
解释: 
如下为给定的树的示意图：
  0
 / \
1   2
   /|\
  3 4 5

我们可以计算出 dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5) 
也就是 1 + 1 + 2 + 2 + 2 = 8。 因此，answer[0] = 8，以此类推。
```

解题方法：建立图表+后序遍历+前序遍历

解题思路：  
将树分成2个子树[0,1]和[2,3,4,5],用`0`代表0子树的节点数量，用`#2`代表2子树的节点数量  
ans(0) = dist(0) + dist(2) + #2  
ans(2) = dist(0) + dist(2) + #0  
我们可以发现 ans(2) = ans(0) + #0 - #2, 而#0 = 节点总数N - #2， 因此ans(2) = ans(0) + N - 2*#2，而2代表的是neighbor，1代表的是root  

因此我们可以使用一个后序遍历的递归函数得到每一个节点代表的子树的距离以及每一个子树的节点数量  
每一个子树的节点数量等于它的子树的节点之和，距离等于子树的距离之和加节点  
>Node[root] = Sum(Node[child]) + 1  
>distance[root] = Sum(distance[child]) + Sum(Node[child])

再通过第二个前序遍历的递归函数，更新所有节点的最终距离  
>distance[child] = distance[root] + N - 2*Node[child]

代码
```python
class Solution:
    def sumOfDistancesInTree(self, N: int, edges):
        
        self.N = N
        tree = {}
        count = {}
        size = len(edges)
        distance = {}

        for i in range(N):
            tree[i] = []
            count[i] = 1
            distance[i] = 0

        for i in range(size):
            tree[edges[i][0]].append(edges[i][1])
            tree[edges[i][1]].append(edges[i][0])

        def dfs(root, parent):
            neighbors = tree[root]

            for i in neighbors:

                if i == parent:
                    continue

                dfs(i, root)
                count[root] += count[i]
                distance[root] += distance[i] + count[i]
                
        def dfs2(root, parent):
            
            neighbors = tree[root]
            
            for i in neighbors:
                if i == parent:
                    continue
                
                distance[i] = distance[root] + self.N - 2*count[i]
                dfs2(i, root)
                
        dfs(0, -1)
        dfs2(0, -1)
        
        ans = list(distance.values())
        
        return ans
```
