# Clone Graph

 Clone Graph  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/clone-graph/) )

```
Description
Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.

How we serialize an undirected graph:
Nodes are labeled uniquely.
We use # as a separator for each node, and , 
as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph {0,1,2#1,2#2,2}.
The graph has a total of three nodes, and therefore contains three parts as separated by #.
- First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
- Second node is labeled as 1. Connect node 1 to node 2.
- Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.

Visually, the graph looks like the following:
   1
  / \
 /   \
0 --- 2
     / \
     \_/

Example
return a deep copied graph.
```

### 解题思路

首先复习一下宽度优先搜索（BFS, Breadth-first Search）的原理：从树或图的某个结点开始，先遍历该结点的所有相邻结点，然后再遍历下一层相邻结点。BFS 的实现通常需要一个辅助队列 queue 保存已遍历结点。

伪代码如下：

```java
procedure BFS(G,v) is
      create a queue Q
      create a set V
      add v to V
      enqueue v onto Q
      while Q is not empty loop
         t ← Q.dequeue()
         if t is what we are looking for then
            return t
        end if
        for all edges e in G.adjacentEdges(t) loop
           u ← G.adjacentVertex(t,e)
           if u is not in V then
               add u to V
               enqueue u onto Q
           end if
        end loop
     end loop
     return none
 end BFS
```



#### 一、BFS

由于题目只给出了一个结点，所以要使用 BFS 遍历无向图找到所有结点，和遍历树不同的是，需要使用 `HashSet` 去除重复结点。

- 找到无向图的所有结点。
- 依次拷贝结点（主要是结点值），使用 `HashMap<oldNode, newNode>` 。
- 依次拷贝每个结点的邻居结点。

其实分解后每个步骤都比较容易实现，需要对 `HashSet` 、 `HashMap` 、 `Queue` 的操作比较熟悉。

Java 实现

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     ArrayList<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { 
 *          label = x; 
 *          neighbors = new ArrayList<UndirectedGraphNode>(); 
 *      }
 * };
 */
public class Solution {
    /**
     * @param node: A undirected graph node
     * @return: A undirected graph node
     */
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) {
            return node;
        }
        
        // use bfs algorithm to traverse the graph and get all nodes.
        ArrayList<UndirectedGraphNode> nodes = getNodes(node);
        
        // copy nodes, store the old -> new mapping information in a hash map
        HashMap<UndirectedGraphNode, UndirectedGraphNode> mapping = new HashMap<>();
        for (UndirectedGraphNode oldNode : nodes) {
            mapping.put(oldNode, new UndirectedGraphNode(oldNode.label));
        }
        
        // copy neighbors (edges)
        for (UndirectedGraphNode oldNode : nodes) {
            UndirectedGraphNode newNode = mapping.get(oldNode);
            for (UndirectedGraphNode neighbor : oldNode.neighbors) {
                UndirectedGraphNode newNeighbor = mapping.get(neighbor);
                newNode.neighbors.add(newNeighbor);
            }
        }
        
        return mapping.get(node);
    }
    
    private ArrayList<UndirectedGraphNode> getNodes(UndirectedGraphNode node) {
        Queue<UndirectedGraphNode> queue = new LinkedList<UndirectedGraphNode>();
        HashSet<UndirectedGraphNode> set = new HashSet<UndirectedGraphNode>();
        
        queue.offer(node);
        set.add(node);
        while (!queue.isEmpty()) {
            UndirectedGraphNode head = queue.poll();
            for (UndirectedGraphNode neighbor : head.neighbors) {
                if (!set.contains(neighbor)) {
                    set.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
        
        return new ArrayList<UndirectedGraphNode>(set);
    }
}
```



#### 二、BFS II

可以在 BFS 遍历所有结点时完成拷贝，这样可以将方法一中的三个步骤简化为两个步骤，具体参考以下程序。

Java 实现

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     ArrayList<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { 
 *          label = x; 
 *          neighbors = new ArrayList<UndirectedGraphNode>(); 
 *     }
 * };
 */
public class Solution {
    /**
     * @param node: A undirected graph node
     * @return: A undirected graph node
     */
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) {
            return node;
        }
        
        ArrayList<UndirectedGraphNode> nodes = new ArrayList<>();
        HashMap<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
        
        // clone nodes
        nodes.add(node);
        map.put(node, new UndirectedGraphNode(node.label));
        
        int start = 0;
        while (start < nodes.size()) {
            UndirectedGraphNode head = nodes.get(start++);
            for (UndirectedGraphNode neigh : head.neighbors) {
                if (!map.containsKey(neigh)) {
                    map.put(neigh, new UndirectedGraphNode(neigh.label));
                    nodes.add(neigh);
                }
            }
        }
        
        // clone neighbors
        for (UndirectedGraphNode oldNode : nodes) {
            UndirectedGraphNode newNode = map.get(oldNode);
            for (UndirectedGraphNode oldNeigh : oldNode.neighbors) {
                newNode.neighbors.add(map.get(oldNeigh));
            }
        }
        
        return map.get(node);
    }
}
```



#### 三、BFS III

相较于方法二，可以在使用 BFS 遍历结点时，同时拷贝结点及结点关系。

Java 实现

```java
/**
 * Definition for undirected graph.
 * class UndirectedGraphNode {
 *     int label;
 *     ArrayList<UndirectedGraphNode> neighbors;
 *     UndirectedGraphNode(int x) { 
 *          label = x; 
 *          neighbors = new ArrayList<UndirectedGraphNode>(); 
 *     }
 * };
 */
public class Solution {
    /**
     * @param node: A undirected graph node
     * @return: A undirected graph node
     */
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        if (node == null) {
            return node;
        }
        Queue<UndirectedGraphNode> q = new LinkedList<UndirectedGraphNode>();
        HashMap<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
        
        map.put(node, new UndirectedGraphNode(node.label));
        q.add(node);
        
        while (!q.isEmpty()) {
            UndirectedGraphNode curt = q.poll();
            for (UndirectedGraphNode neighbor : curt.neighbors) {
                if (!map.containsKey(neighbor)) {
                    // clone nodes
                    map.put(neighbor, new UndirectedGraphNode(neighbor.label));
                    q.add(neighbor);
                }
                // clone neighbors
                map.get(curt).neighbors.add(map.get(neighbor));
            }
        }
        return map.get(node);
    }
    
}
```



### 参考

1. [Clone Graph | 九章算法](http://www.jiuzhang.com/solutions/clone-graph/)
2. [Clone Graph leetcode java（DFS and BFS 基础） | 爱做饭的小莹子](http://www.cnblogs.com/springfor/p/3874591.html)