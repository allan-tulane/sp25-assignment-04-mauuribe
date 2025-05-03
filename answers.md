# CMPS 2200 Assignment 5
## Answers

**Name:**Mauricio Uribe






- **1a.**
In a d-ary heap, each internal node may have up to d children, forming a complete d-ary tree rather than a binary one. As a result, the height of the tree containing n elements becomes log base d of n, or log (d) (n). Since increasing the number of children per node reduces the number of levels, the tree becomes shallower as d increases.




- **1b.**
For a d-ary heap, the delete-min operation removes the root and restores the heap structure by relocating a leaf node to the top and pushing it downward until the heap property is satisfied. During this "sift down" process, each level requires comparing the current node to up to d children to find the minimum, and since the heap's height is O(log₍d₎n), the total work becomes O(d·log₍d₎n). In contrast, inserting a new element involves placing it at the next available leaf position and percolating it upward, which requires only one comparison per level. The total work is therefore O(log(d)n).


- **1c.**
When using a d-ary heap to implement Dijkstra’s algorithm, performance depends on the cost of delete-min and insert. Since each vertex undergoes one delete-min and each edge may trigger an insert or decrease-key, the total time becomes:
O(|V|·d·log(d)|V|) for delete-min operations
O(|E|·log(d)|V|) for insert/decrease-key operations
So, the full running time is O(|V|·d·log(d)|V| + |E|·log(d)|V|).

- **1d.**
To optimize Dijkstra’s algorithm to run in O(|E|) time, we need to ensure the delete-min cost is bounded by |E|. This is achievable by selecting d = Θ(|V|^ε) for some ε > 0. When d is this large, log(d)|V| approaches a constant, making d·log(d)|V| = O(1). Hence, the delete-min term simplifies to O(|V|), and the overall runtime becomes O(|E|).


- **2a.**
The shortest path weights for APSP at each stage reflect the inclusion of more intermediate vertices. At k = 0, the paths only use direct edges, so many entries are ∞. At k = 1, we allow vertex 1 as an intermediate, which reveals new paths like from 0 to 2 becoming 2 → –1 later. By k = 2, the results further improve — for instance, APSP(0,2,2) becomes –1 due to a shorter route through vertex 2. This shows how the distances get refined as we increase the set of allowed intermediates.

- **2b.**
We notice that the values of APSP(i,j,k) either stay the same or get smaller as k increases. This is because each step considers whether using vertex k as an intermediate leads to a shorter path. The update rule is: APSP(i,j,k) = min(APSP(i,j,k–1), APSP(i,k,k–1) + APSP(k,j,k–1)), which compares the existing shortest path with a new path that goes through node k. This ensures we always keep the best known option at each stage.

- **2c.**
The dynamic programming solution for APSP relies on the principle of optimal substructure. The idea is that the shortest path from i to j using the first k vertices is either: the shortest path that avoids vertex k altogether, or a new, potentially shorter path that passes through k. This gives rise to the recursive formula: APSP(i,j,k) = min(APSP(i,j,k–1), APSP(i,k,k–1) + APSP(k,j,k–1)), capturing the logic of building up shortest paths by successively including more nodes as intermediates.

- **2d.**
Each of the indices i, j, and k ranges over all vertices, meaning the total number of subproblems is V × V × V = V³. With memoization or tabulation, we compute each entry only once, giving a total time complexity of O(V³). While cubic, this method avoids redundant recalculations and is practical for graphs with moderate size.

- **2e.**
Johnson’s algorithm has a runtime of O(|V||E| log|V|), making it efficient for sparse graphs. However, when the graph is dense, meaning |E| approaches |V|², the dynamic programming method with O(V³) time can be faster or equally efficient. Additionally, dynamic programming is simpler to implement and works well when negative edge weights are present, provided there are no negative cycles.


- **3a.**
A Minimum Spanning Tree (MST) does not necessarily solve the Minimum Maximum Edge Tree (MMET) problem. While MSTs aim to minimize the total edge weight, MMET seeks to minimize the single largest edge used in the tree. In some cases, an MST might include a high-weight edge if it helps reduce the total, even though a different tree could have a lower maximum edge. Therefore, the two are not guaranteed to be the same.

- **3b.**
To compute the second-best spanning tree, you start by finding the original MST. Then, for each edge in that MST, remove it and recompute an MST from the remaining graph (excluding that edge). Among all such recomputed trees, the one with the smallest total weight becomes your next-best spanning tree.

- **3c.**
This process involves computing one MST initially, and then computing a new MST for each of the n–1 edges in the original tree. Each MST takes O(E·logV) using Prim’s or Kruskal’s algorithm. Thus, the total time becomes O(n·E·logV).
