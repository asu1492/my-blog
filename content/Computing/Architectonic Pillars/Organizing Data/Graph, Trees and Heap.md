
## Graphs — 7 patterns
1. **DFS / BFS Traversal**  
    Connectivity, components.
2. **Cycle Detection**  
    Directed (DFS + recursion stack), undirected (parent tracking).
3. **Topological Sort**  
    Dependency resolution (Kahn / DFS).
4. **Shortest Path**  
    BFS (unweighted), Dijkstra, Bellman-Ford.
5. **Union-Find (Disjoint Set)**  
    Dynamic connectivity, Kruskal.
6. **Flood Fill / Grid Graph**  
    Islands, regions, boundaries.
7. **State Graph / BFS with State**  
    Bitmask, multi-dimensional visited.
## Trees — 6 patterns
1. **DFS Traversal** (Pre/In/Post order)  
2. **BFS / Level Order Traversal**  
    Shortest path, level-wise computation.
3. **Bottom-Up (Postorder DP on Tree)**  
    Heights, diameters, subtree aggregation.
4. **Top-Down (Preorder with State)**  
    Carry constraints from parent to child.
5. **Binary Search Tree Properties**  
    Inorder sortedness, range queries.
6. **Path-Based Problems**  
    Root-to-leaf, any-to-any path sums.

## Heaps — 2 patterns
1. **Top-K / K-th Element**  
    Maintain size-K heap.
2. **Two Heaps (Median Pattern)**  
    Max-heap + min-heap for balancing.

------

Resources:
1. [Striver Lectures on Graphs](https://www.youtube.com/playlist?list=PLgUwDviBIf0rGEWe64KWas0Nryn7SCRWw)

---
## Questions
1. [ ] 994: Multi-source BFS from all rotten oranges.
2. 207: Topological sort with Kahn’s algorithm (indegree).

---

What is Graph 
1. Non linear DS
2. Collection of nodes/vertices connected through edges
3. Node is one fundamental unit/entity of which graph is formed
4. Edge — defined by two end points


Connections
1. Directed vs Undirected 
	1. Two Way : undirected edge (e.g. FB Friends)
	2. One Way : directed edge (eg follow features in networking sites)
2. Outdegre - no of edges going out of the node
3. Indegree - no of edges coming into node


Application

1. Social network
    
2. Maps
    
3. Website back links
    
4. City Routes
    
5. Web Crawl for a website or for entire WWW 
    

  

  



  

Link

1. DAG (directed acyclic graph) used in Apache Kafka
    

  

  

Note

1. Think like pipes connected through nodes
2. Or C60 structure

-----
Binary Tree
1. At most two children 
2. one edge b/w two nodes 
3. leaf -
4. Type of BT
	1. Full BT - every node either 0 or 2 children - No node has one child
	2. Perfect BT - all nodes have 2 children & all leaves are at the same level. (Thus PBT is both FBT and CBT)
	3. Complete BT - All levels filled except last level filled left to right 
		1. Min Heap - Each node smaller than children
	4. Balanced BT - 
		1. Balanced enough to ensure 0(log n) insertion and finding the element 
		2. Common BBT 
			1. AVL Trees
			2. Red Black Trees 
	5. BST - left < Parent < right 
5. Traversal 
	1. In Order - ascending order - left -> root -> right 
	2. Pre Order - root -> left -> right  **`root is first`**
	3. Post Order - left -> right -> root  **`root is last`**

Binary Search Tree
- To **organize data** so we can **search quickly**
- To avoid scanning every item in a list
- To exploit this "divide and conquer" idea in storage and retrieval
- Time Complexity - O(log n)
- BST was popularized by Donald Knuth in his legendary work "The Art of Computer Programming(1968)"