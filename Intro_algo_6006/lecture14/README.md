# Graphs II: Depth-First Search
- Depth-First Search (DFS)
- Edge Classification
- Cycle Testing
- Topological Sort

## Recall:
- <u style="color:rgb(237,93,177)">Graph search</u>: Explore a graph
  - e.g., find a path from start vertex $s$ to a desired vertex.
- <u style="color:rgb(237,93,7)">Adjacency list</u>: array 
$Adj$ of $|V|$ linked lists.
  - for each vertex $u \in V$, $Adj[u]$ stores $u$'s neighbors,
  i.e., 
  $\{v \in V | (u,v)\in E\}$ (<span style="color:orange">just outgoing
  edges if directed</span>)

For example:
![Adjacency Lists](graph0.jpg)

### Breadth-first Search (BFS):
Explore level-by-level from $s$ --- find shortest paths.

### Depth-First Search (DFS):
This is like exploring a maze:
<img src="graph2.jpg" style="width:200px;height:200px; display: block; margin-left: auto; margin-right: auto;" alt="maze Depth-Frist Search Frontier">

#### Depth-First Search Algorithm:
- Follow path until you get stuck.
- Backtrack along breadcrumbs until reach unexplored neighbor.
- Recursively explore.
- Careful not to repeat a vertex.

<img src="code.png" style="width:400px;height:200px; display: block; margin-left: auto; margin-right: auto;" alt="Depth-First Search Algorithm">

##### Example:
![Depth-First Traversal](graph3.jpg)

##### Edge Classification
![Edge Classification](graph4.jpg)

- To compute this classification (<span style="color:cyan">back or not</span>), mark nodes for duration they are
"on the stack".

- Only tree and back edges in undirected graph.

#### Analysis
- DFS-visit gets called with a vertex $s$ only once (because then $parent[s]$ set).
$\implies$ time in DFS-visit $= \sum_{s \in V} |Adj[s]| = O(E)$

- DFS outer loop adds just $O(V)$
$\implies O(V+E)$ time (<span style="color:orange">linear time</span>).

### Cycle Detection
Graph $G$ has a cycle $\iff$ DFS has a back edge.

### Proof:
![Proof](graph5.jpg)
![Proof](graph6.jpg)

- before visit to $v_i$ finishes,

  Will visit $v_{i+1}$ ($\&$ finish):

  Will consider edge $(v_i, v_{i+1})$

  $\implies$ visit $v_{i+1}$ now or already did

- $\implies$ before visit to $v_0$ finishes,
will visit $v_k$ ($\&$ didn't before)

- $\implies$ before visit to $v_k$ (or $v_0$) finishes,
will see (v_k, v_0) as back edge.

##### Job scheduling
Given Directed Acylic Graph ($DAG$), where vertices represent tasks $\&$ edges represent 
dependencies, order tasks without violationg dependencies.

![Dependence Graph:DFS Finishing Times](graph7.jpg)

###### Source:

<span style="display:block; margin-left:auto; margin-right:auto">
Source : vertex with no incoming edges : schedulable at beginning
(<span style="color:orange">A,G,I</span>)
</span>

### Attempt:
BFS from each source:
- from A finds A, BH, C,F
- from D finds D, BE, CF <span style="color:cyan">$\leftarrow slow ... wrong!$</span>
- from G finds G, H
- from I finds I

### Topological Sort

<div style="display:flex; align-items:center">

Reverse of DFS <u style="color:cyan">finishing times</u> (time at which DFS-Visit($v$) finishes)

$$
\begin{cases}
DFS-Visit(v) \\
\ \ \ \ \cdots \\
\ \ \ \ order.append(v)\\
order.reverse()
\end{cases}
$$

</div>


##### Correctness
For any edge $(u,v)$ --- $u$ ordered before $v$, i.e., $v$ finished before $u$.

<img src="graph8.jpg" style="width:auto;height:50px; display: block; margin-left: auto; margin-right: auto;" alt="Correctness">

- if $u$ visited before $v$:
  - before visit to $u$ finishes, will visit $v$ (<span style="color:rgb(237,93,177)">via ($u,v$) or otherwise</span>)
  - $\implies v$ finishes before $u$. 

- if $v$ visited before $u$:
  - graph is acyclic
  - $\implies$ $u$ cannot be reached from $v$.
  - $\implies$ visit to $v$ finishes before visiting $u$.
