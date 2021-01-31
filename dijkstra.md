迪杰斯特拉算法是用来寻找单元最短路径，应用在给定初始点和终点，每个路径的cost都非负，找最短路径，初始化一个列表用来存放各个点得连接状态，最开始全部元素得值都是None代表无法到达。从给定源点开始，初始化列表，把源点位置得值改为0，代表这个位置为源点，然后依次查找与之相连得结点，更新列表的值，代表从源点到该点需要多少cost。直至遍历到终点。

[例题.778水位上升的游泳池中游泳](https://leetcode-cn.com/problems/swim-in-rising-water/comments/)

````python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        n = len(grid)
        que = deque([(0, 0)])
        ans = [[float('inf')]*n for _ in range(n)]
        ans[0][0] = grid[0][0]
        while que:
            x, y = que.popleft()
            for i, j in ((1, 0), (0, 1), (-1, 0), (0, -1)):
                pox = x + i 
                poy = y + j 
                if 0 <= pox < n and 0 <= poy < n:
                    d = max(grid[pox][poy], ans[x][y])
                    if d < ans[pox][poy]:
                        ans[pox][poy] = d
                        que.append((pox, poy))
        return ans[-1][-1]
````
这个程序中，从矩阵的左上角开始，进行深度优先遍历，使用栈来存放需要遍历的结点，每新遍历一个结点，就将其子结点加入栈，并更新列表中各位置所需的cost值，该结点遍历完成后，移除该结点，当栈空了，也就代表全部结点遍历完成，所得的最终结点在列表中对应的值就是最小的cost。

完整算法思路
````python
V = 7
# 标记数组：used[v]值为False说明改顶点还没有访问过
used = [False for _ in range(V)]
# 距离数组：distance[i]表示从源点s到ｉ的最短距离，distance[s]=0
distance = [float('inf') for _ in range(V)]
# cost[u][v]表示边e=(u,v)的权值，不存在时设为INF
cost = [[float('inf') for _ in range(V)] for _ in range(V)]


def dijkstra(s):
    distance[s] = 0
    while True:
        # v在这里相当于是一个哨兵，有顶点没有被维护，则不为-1，全部维护过后，则为-1
        v = -1
        # 从未使用过的顶点中选择一个距离最小的顶点
        for u in range(V):
            if not used[u] and (v == -1 or distance[u] < distance[v]):
                v = u
        if v == -1:
            # 说明所有顶点都维护到S中了
            break

        # 将选定的顶点加入到S中, 同时进行距离更新
        used[v] = True
        # 更新U中各个顶点到起点s的距离。之所以更新U中顶点的距离，是由于上一步中确定了k是求出最短路径的顶点，从而可以利用k来更新其它顶点的距离；例如，(s,v)的距离可能大于(s,k)+(k,v)的距离。
        for u in range(V):
            distance[u] = min(distance[u], distance[v] + cost[v][u])


if __name__ == '__main__':
    for _ in range(12):
        v, u, w = list(map(int, input().split()))
        cost[v][u] = w
    s = int(input('请输入一个起始点：'))
    dijkstra(s)
    print(distance)
````
