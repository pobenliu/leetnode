#  Topological Sorting

 Topological Sorting  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/topological-sorting/) )

```
Description
Given an directed graph, a topological order of the graph nodes is defined as follow:
- For each directed edge A -> B in graph, A must before B in the order list.
- The first node in the order can be any node in the graph with no nodes direct to it.
Find any topological order for the given graph.

Notice
You can assume that there is at least one topological order in the graph.

Clarification
Learn more about representation of graphs

Example
For graph as follow:
```

![](http://ww1.sinaimg.cn/mw690/600e6311gw1f889r0gd08j206204k0sm.jpg)

```
The topological order can be:

[0, 1, 2, 3, 4, 5]
[0, 2, 3, 1, 5, 4]
...

Challenge 
Can you do it in both BFS and DFS?
```

### 解题思路

#### 一、BFS

- 将有父结点的结点，及其父结点个数（入度）作为 `key` 和 `value` 存入 `HashMap` 中。
- 找到入度为零的结点，放入队列。
- 将队列中结点取出，将其子结点入度分别减一，更新后入度为零的结点放入队列。

**复杂度分析**

以 `V` 表示顶点数，`E` 表示有向图中边的数量。

- 时间复杂度：获得结点入度为 `O(V + E)` ，寻找入度为 0 的结点 `O(V)` ，BFS 遍历 `O(V + E)` ，故总的时间复杂度为 `O(V + E)` 。
- 空间复杂度：使用哈希表存储结点，故空间复杂度为 `O(V)` 。

Java 实现

```java
/**
 * Definition for Directed graph.
 * class DirectedGraphNode {
 *     int label;
 *     ArrayList<DirectedGraphNode> neighbors;
 *     DirectedGraphNode(int x) { 
 *          label = x; 
 *          neighbors = new ArrayList<DirectedGraphNode>(); 
 *     }
 * };
 */
public class Solution {
    /**
     * @param graph: A list of Directed graph node
     * @return: Any topological order for the given graph.
     */    
    public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
        ArrayList<DirectedGraphNode> results = new ArrayList<DirectedGraphNode>();
        if (graph == null) {
            return results;
        }
        
        // Map.Entry = <Node N, numbers of parents(in-degree) for N>
        HashMap<DirectedGraphNode, Integer> map = new HashMap<>();
        for (DirectedGraphNode node : graph) {
            for (DirectedGraphNode neighbor : node.neighbors) {
                if (map.containsKey(neighbor)) {
                    map.put(neighbor, map.get(neighbor) + 1);
                } else {
                    map.put(neighbor, 1);
                }
            }
        }
        
        // add nodes without parents to the queue
        Queue<DirectedGraphNode> queue = new LinkedList<DirectedGraphNode>();
        for (DirectedGraphNode node : graph) {
            if (!map.containsKey(node)) {
                queue.offer(node);
                results.add(node);
            }
        }
        
        // poll a node from the queue and update # parents (-1) for its children 
        while (!queue.isEmpty()) {
            DirectedGraphNode node = queue.poll();
            for (DirectedGraphNode n : node.neighbors) {
                map.put(n, map.get(n) - 1);
                //add nodes without parents to the queue
                if (map.get(n) == 0) {
                    results.add(n);
                    queue.offer(n);
                }
            }
        }
        return results;
    }
}
```



#### 二、DFS



### 参考

1. [Topological Sorting | 九章算法](http://www.jiuzhang.com/solutions/topological-sorting/)
2. [Topological Sorting | 数据结构与算法/leetcode/lintcode题解](https://algorithm.yuanbin.me/zh-hans/)