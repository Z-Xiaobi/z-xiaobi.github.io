---
title: "Is Bipartite Graph CN"
date: 2021-02-27T22:52:45+08:00
description: ""
tags: ["bipartite graph", "leetcode"]
series: ["data structure"]
categories: ["Algorithms"]
draft: true
---

# LeetCode 785. 判断二分图
存在一个 **无向图** ，图中有 n 个节点。其中每个节点都有一个介于 0 到 n - 1 之间的唯一编号。给你一个二维数组 graph ，其中 graph[u] 是一个节点数组，由节点 u 的邻接节点组成。形式上，对于 graph[u] 中的每个 v ，都存在一条位于节点 u 和节点 v 之间的无向边。

该无向图同时具有以下属性：
- 不存在自环（`graph[u]` 不包含 `u`）。
- 不存在平行边（`graph[u]` 不包含重复值）。
- 如果 `v` 在 `graph[u]` 内，那么 `u` 也应该在 `graph[v]` 内（该图是无向图）
- 这个图可能不是连通图，也就是说两个节点 `u` 和 `v` 之间可能不存在一条连通彼此的路径。

**二分图** 定义：如果能将一个图的节点集合分割成两个独立的子集 `A` 和 `B` ，并使图中的每一条边的两个节点一个来自 `A` 集合，一个来自 `B` 集合，就将这个图称为 二分图 。

如果图是二分图，返回 `true` ；否则，返回 `false` 。

 
```
示例 1：
输入：graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
输出：false
解释：不能将节点分割成两个独立的子集，以使每条边都连通一个子集中的一个节点与另一个子集中的一个节点。
0----1
| \  |
|  \ |
3----2

示例 2：
输入：graph = [[1,3],[0,2],[1,3],[0,2]]
输出：true
解释：可以将节点分成两组: {0, 2} 和 {1, 3} 。
0----1
|    |
|    |
3----2
```

提示：
- `graph.length == n`
- `1 <= n <= 100`
- `0 <= graph[u].length < n`
- `0 <= graph[u][i] <= n - 1`
- `graph[u]` 不会包含 `u`
- `graph[u]` 的所有值 互不相同
- 如果 `graph[u]` 包含 `v`，那么 `graph[v]` 也会包含 `u`

每个边的两个顶点分别属于不同的唯二的两个组里，可以将问题转化为给节点涂两种颜色的问题，也就是，一个边的两个顶点必然不是同一颜色（组）。

Java
方法一：并查集
遍历每一个顶点的邻接点集合，将其在并查集中合并
并且在遍历过程中检查，如果当前顶点的某一邻接点已和当前顶点连通，那么就说明不是二分图
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        Union uf = new Union(n);
        for(int i=0;i<n;i++){
            int vertexs[] = graph[i];
            //将一个顶点对应的所有邻接点合并
            for(int vertex:vertexs){
                //如果邻接点和当前顶点连通，则不符合二分图定义
                if(uf.connected(i,vertex)){
                    return false;
                }
                uf.union(vertex,vertexs[0]);
            }
        }
        return true;
    }
}
//并查集模板
class Union {
    int parent[];
    int size[];
    public Union (int n){
        parent = new int[n];
        size = new int[n];
        for(int i=0;i<n;i++){
            parent[i] = i;
            size[i] = 1;
        }
    }
    public void union(int index1,int index2){
        int root1 = find(index1);
        int root2 = find(index2);
        if(root1==root2){
            return;
        }
        if(size[root1]>=size[root2]){
            parent[root2] = root1;
            size[root1] += size[root2];
        } else {
            parent[root1] = root2;
            size[root2] += size[root1];
        }
    }
    public int find(int index){
        while(index!=parent[index]){
            parent[index] = parent[parent[index]];
            index = parent[index];
        }
        return index;
    }
    public boolean connected(int index1,int index2){
        int root1 = find(index1);
        int root2 = find(index2);
        return root1==root2;
    }
}
```

方法二：深度优先搜索
染色法
对当前顶点赋值1的话，则对其所有邻接点赋值-1。
对当前顶点赋值-1的话，则对其所有邻接点赋值1。
在DFS过程中不断反转赋值
如果在搜索过程中某点已赋值，则检查该点值是否和应赋的值一致，如果不一致则不符合二分图定义

```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        //visited数组默认值为0，1表示一类，-1表示一类
        int visited[] = new int[n];
        for(int i=0;i<n;i++){
            if(visited[i]==0){
                //若返回false，即该点值和应赋的值不一致，则不符合二分图定义
                if(!dfs(graph,visited,i,1)){
                    return false;
                }
            }
        }
        return true;
    }
    public boolean dfs(int graph[][],int visited[],int v,int type){
        //若该点已被赋值，则判断
        if(visited[v]!=0){
            return visited[v]==type;
        }
        visited[v] = type;
        int vertexs[] = graph[v];
        for(int vertex:vertexs){
            //对该点的邻接点进行反转赋值
            if(!dfs(graph,visited,vertex,-type)){
                return false;
            }
        }
        return true;
    }
}

作者：二货磁铁
链接：https://leetcode-cn.com/leetbook/read/algorithm-and-interview-skills/xtek1g/?discussion=PUOQyY
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```

方法三：广度优先遍搜索
和DFS一样使用染色法就行
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        //visited数组默认值为0，1表示一类，-1表示一类
        int visited[] = new int[n];
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i=0;i<n;i++){
            if(visited[i]==0){
                queue.offer(i);
                visited[i] = 1;
                while(!queue.isEmpty()){
                    int v = queue.poll();
                    int vertexs[] = graph[v];
                    //遍历当前顶点的邻接点
                    for(int vertex:vertexs){
                        //如果当前顶点的某个邻接点赋值等于该顶点赋值，则不符合二分图定义
                        if(visited[vertex]==visited[v]){
                            return false;
                        }
                        //反转赋值并入队
                        if(visited[vertex]==0){
                            visited[vertex] = -visited[v];
                            queue.offer(vertex);
                        }
                    }
                }
            }
        }
        return true;
    }
}
```
```java
// solution from video
// 0ms 39.2MB
class Solution {
    public boolean isBipartite(int[][] graph) {
        int[] colors = new int[graph.length];
        for (int i=0; i < colors.length; i++) { //这个循环是为了保证我们遍历了所有的点
            if (colors[i] == 0 &!dfs(graph, i, colors, 1)) return false;
        }
        return true;
    }
    boolean dfs(int[][] graph, int cur, int[]colors, int color) {
        //着色
        colors[cur] = color;
        for (int next: graph[cur]) {
            //如果遇到相邻节点颜色相同，返回false;
            if (colors[next] == color)
                return false;
            //如果下一个节点没有着色，进行递归着色查找
            if (colors[next] == 0 && !dfs(graph, next, colors, -color))
                return false;
        }
        return true;
    }
}
```

# Reference
[力扣-算法与面试技巧精讲](https://leetcode-cn.com/leetbook/read/algorithm-and-interview-skills/xtek1g/)
