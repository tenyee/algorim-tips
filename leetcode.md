# 第一章 最短路径问题

## 1.1 Dijkstra算法

​	算法思想：从源点start到end，如果存在中间节点k达到end，那么从源点到end的可选路径为从start到达k，再从k到达end。

如果从start到达k是最小的，从k到达end也是最小的，那么就可以找到最短路径，贪心算法。

​	算法步骤：

```c
初始化：
	dis[i]:表示从start到i节点的最小距离
	visit[i]:表示i节点已被添加到遍历集合中，下次不需要再遍历
	
（1）查找dis[i]且i未被添加到visit中最小的值的下标（就是节点）作为一个扩散点k；
（2）把k添加到visit中，以k为间接点，计算从源点到i的最短距离，更新dis[i] = min{dis[i], dis[k]+grid[k][i]}
（3）重复上面两步，直到所有节点都加入到visit中
    
代码：
/*
	grid[i][j] = m ,有向带权图，无负权值
	N，节点数量
	start，源点
*/
void Dijkstra(vector<vector<int>> &grid, int N, int start)
{
    vector<bool> visit(N, false);
    vector<int> dist(N, INT_MAX);
    
    // 初始化从源点到其它点的距离表dist
    for (int i = 0; i < N; ++i) {
        dist[i] = grid[start][i];
    }
    dist[start] = 0;
    visit[start] = true;
    
    // 遍历所有节点
    for (int i = 0; i < N; ++i) {
        // 找到当前不在visit中，且dist为最小的点，作为一个扩散点
        int minVal = INT_MAX;
        int minIndex = 0;
        for (int i = 0; i < N; ++i) {
            if (visit[i] == false && dist[i] > minVal) {
                minVal = dist[i];
                minIndex = i;
            }
        }
        // 标志节点已遍历
        visit[minIndex] = true;
        // 以点minIndex为中间点，更新dist
        for (int j = 0; j < N; ++j) {
            dist[j] = min(dist[j], dist[minIndex] + grid[minIndex][j]);
        }
    }
    // 到此，dist就是从start到其它节点的最小距离集
}
```



