# Shortest Paths II - Dijkstra
- Review
- Shortest paths in DAGs.
- Shortest paths in graphs without negative edges.
- Dijsktra's Algorithm.

## Readings
CLRS, Sections 24.2-24.3

## Review

$d[v]$ is the length of the current shortest path from starting vertex $s$.
Through a process of relaxation, $d[v]$ should eventually become
$\delta(s,v)$, which is the length of the shortest path from $s$ to $v$.
$\prod[v]$ is the predecessor of $v$ in the shortest path from $s$ to $v$.

&nbsp;  Basic operation in shortest-path computation is the relaxation 
operation:
```
RELAX(u,v,w):
  if d[v] > d[u] + w(u,v):
    d[v] = d[u] + w(u,v)
    pred[v] = u
```

$RELAX(u,v,w)$<br>
&nbsp;&nbsp;&nbsp;if $d[v] > d[u] + w(u,v)$<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;then $d[v] = d[u] + w(u,v)$<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\prod[v]\leftarrow u$

## Relaxation is Safe
<span style="color:cyan; font-weight:bold">Lemma:</span> The relaxation 
algorithm maintains the invariant that $d[v] \geq \delta(s,v)$ for all
vertices in the graph ($v \in V$).

<span style="color:cyan; font-weight:bold">Proof:</span> By induction on the 
of steps:

&nbsp; Consider $RELAX(u,v,w)$. By induction $d[u] \geq \delta(s,u)$. By the 
Triangle Inequality, $\delta(s,v) \leq \delta(s,u) + \delta(u,v)$. This means
that $\delta(s,v) \leq d[u] + w(u,v)$, since $d[u] \geq \delta(s,u)$ and 
$w(u,v) \geq \delta(u,v)$. So setting $d[v] = d[u] + w(u,v)$ is safe.

### Directed Acyclic Graphs (DAGs):
Can't have negative cycles because there are no cycles!

  1. Topologically sort the DAG, Path from $u$ to $v$ implies that 
  $u$ is before $v$ in the linear ordering.
  2. One pass over vertices in topologically sorted order relaxing each edge 
  that leaves each vertex:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$\theta(V+E)$ time
