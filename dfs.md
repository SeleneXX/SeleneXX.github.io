深度优先遍历，在图中和数中经常用到。尤其是在搜索图中是否存在一连串元素时，非常有用，附带的往往还有剪枝和回溯。回溯和dfs通常使用递归就可以解决，而剪枝只需要在每一次递归中加入一个判断条件。

[剑指offer12.矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]
但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

这题就是一道典型的查找连续元素的题目，找到需求的队列的第一个元素后，对这个元素的四个方向进行遍历，并进行递归查找。
````python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def dfs(i, j, k):   #dfs函数
            if not 0 <= i < len(board) or not 0 <= j < len(board[0]) or board[i][j] != word[k]: return False    #剪枝，当出界或者当前位置元素不是所需求得元素时，剪枝
            if k == len(word) - 1:  #找到了最后一个元素，后续就不需要继续查找，剪枝
                return True
            board[i][j] = ''    #标记当前位置为空，代表已被查找过
            res = dfs(i + 1, j, k + 1) or dfs(i - 1, j, k + 1) or dfs(i, j + 1, k + 1) or dfs(i, j - 1, k + 1)    #递归调用，遍历四个方向，如果有一个方向存在那就返回true，否则返回false
            board[i][j] = word[k]   #递归结束前要恢复当前位置，方便下一次遍历
            return res

        for i in range(len(board)):
            for j in range(len(board[0])):
                if dfs(i, j, 0): return True    #找到对应得第一个元素开始进行dfs
        return False
````
