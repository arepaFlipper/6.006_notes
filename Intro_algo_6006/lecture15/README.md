# Shortest Paths I: Intro
- Weighted Graphs
- General Approach
- Negative Edges
- Optimal Substructure

## Readings
CLRS Sections 24 (intro).

## Motivations
Shortest way to drive from A to B Google maps "get directions"

  Formulation: Problem on a weighted graph $G(V,E)$ $W:E\implies \mathbb{R}$

  Two algorithms: Dijkstra $O(( V \cdot log_2 V ) + E )$, assumes non-negative
  edge weights Bellman Ford $O(V\cdot E)$ is a general algorithm.

## Application
- Find shortest path from CalTech to MIT
  - See "CalTech Cannon Hack" photos web.mit.edu
  - See Google Maps from CalTech to MIT.
- Model as `a` weghted fraph $G(V,E),W:E\implies \mathbb{R}$
  - $V:$ vertices (street intersections)
  - $E:$ edges (street, roads) directed edges (one way roads)
  - $W(U,V):$ weight of edge from $u$ to $v$ (distance, toll)

  $$
  \text{path }p=<v_0,v_1,\dots,v_k>\\
  (v_i, v_{i+1}) \in E for 0 \geq i \leq k \\
  w(p) = \displaystyle \sum_{i=0}^{k-1}  w(v_i, v_{i+1})
  $$

## Weighted Graphs:
#### Notation:
$u \xrightarrow[]{\text{p}} v$ means $p$ is a path from $v_0$ to $v_k$.
where v_0 is a path from $v_0$ to $v_0$  of weight $0$.

#### Definition:
Shortest path weight from $u$ to $v$ as:

$$
\delta(u,v) =
\begin{cases}
\text{min } \;    
\{w(p): u \xrightarrow[]{\text{p}} v \} & \text{if } \exists \; \text{any such path}\\
\\ 
\infty & \text{otherwise ($v$ unreachable from $u$) } 
\end{cases}
$$

