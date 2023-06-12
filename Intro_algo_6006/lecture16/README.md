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
