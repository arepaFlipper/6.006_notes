# Shortest Paths III: Bellman-Ford

- Review Notation
- Generic S.P. Algorithm
- Bellman-Ford Algorithm
  - Analysis
  - Correctness

### Recall:
$$
\text{path } p = \langle v_1, v_2, \ldots, v_k \rangle
$$
$$
(v_1, v_2, \ldots, v_k) \in E \;\; 0 \leq i \leq k
$$
$$
w(p) = \displaystyle \sum_{i=1}^{k-1} w(v_i, v_{i+1})
$$

Shortest path weight from $u$ to $v$ is $\delta(u,v)$.
$\delta(u,v)$ is $\infty$ if $v$ is unreachable from $u$.
"undefined" if there is a negative cycle on some path from $u$ to $v$. 

### Generic S.P. Algorithm
