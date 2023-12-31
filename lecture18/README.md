# Shortest Paths IV - Speeding up Dijkstra

- single-source single-target Dijkstra
- Bidirectional search
- Goal directed search - potentials and landmarks

#### Readings
<span style="color:rgb(90,255,100)">Wagner, Dorothea, and Thomas Willhalm.
"Speed-up Techniques for Shortest for Shortest-Path Computations."
Proceedings of the 24th International Symposium on Theoretical Aspects
of Computer Science (STACS 2007): 23-36. Read up to Section 3.2</span>

## DIJKSTRA single-source, single target
```
Initialize()
Q <- V[G]
while Q is not empty:
  do u <- Extract_min_priority_queue(Q)(stop if u = t!)
  for each vertex v in Adj[u]:
    do RELAX(u,v,w)
```

__Observation__: If only shortest path from $s$ to $t$ is required, stop when $t$ is removed
from $Q$, $i.e.$, when $u=t$

![Bi-directional Search Idea](graph0.jpg)

## Bi-Directional Search
<span style="color:rgb(90,255,100)">NOTE:</span>
Speed-up techniques covered here do not change worst-case behavior, but 
reduce the number of visited vertices in practice.

<span style="color:cyan;font-weight:bold">Bi-D Search</span>

Alternate forward search from $s$ <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;backward search from $t$<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(follow edges backward)<br/>
$d_f(u)$ distances for forward search<br/>
$d_b(u)$ distances for backward search<br/>

<span style="color:green">Q: What is the termination condition?</span>

<span style="color:green">Answer:</span>
Algorithm terminates when some vertex $w$ has been processed, i.e.,
deleted from the queue of both searches, $Q_f$ and $Q_b$.

Subtlety: After search terminates, find node $x$ with minimum value
of $d_f(x)+d_b(x)$. $x$ may not be the vertex $w$ that caused termination
as in example to the left!.
Find shortest path from $s$ to $x$ using $\prod_f$ and shortest path backwards
from $t$ to $x$ using $\prod_b$.
<span style="color:yellow">Note:</span> x will have been deleted from 
either $Q_f$ or $Q_b$ or both.

![Bi-directional Search Example](graph1.jpg)

$\prod_f[u]=s$ and $\prod_b[u']=t$


$\prod_f[w]=s$ and $\prod_b[w]=t$

$\prod_f[u']=u$ 

$\prod_b[u]=u'$ 


<span style="color:green">Q: How do we find the shortest path from $s$ to $t$?</span>

<img src="graph1.jpg" style="display:block;margin:0 auto" width="300" alt="Bi-directional Search" />

<span style="color:cyan">Claim:</span> If $w$ was processed first from both $Q_f$, $Q_b$ 

find the shortest path, using $\prod_f$ from $s$ to $w$: $\prod_f\prod_f\dots \prod_f[w]\rightarrow s$

find the shortest path, using $\prod_b$ from $t$ to $w$. (backwards): $\prod_b\prod_b\dots \prod_b[w]\rightarrow t$

<span style="color:red">But:</span> This is not quite correct because $w$ may not be on the shortest path.


Original:
<img src="graph2.jpg" style="display:block;margin:0 auto" width="300" alt="Bi-directional Search" />
<span style="color:rgb(90,255,100)">Forward Search</span>
<img src="graph3.jpg" style="display:block;margin:0 auto; border:1px solid rgb(90,255,100)" width="300" alt="Bi-directional Search" />
<span style="color:rgb(255,179,82)">Backward Search</span>
<img src="graph4.jpg" style="display:block;margin:0 auto; border: 1px solid rgb(255,179,82)" width="300" alt="Bi-directional Search" />
<span style="color:rgb(90,255,100)">Forward Search</span>
<img src="graph5.jpg" style="display:block;margin:0 auto; border: 1px solid rgb(90,255,100)" width="300" alt="Bi-directional Search" />
<span style="color:rgb(255,179,82)">Backward Search</span>
<img src="graph6.jpg" style="display:block;margin:0 auto; border: 1px solid rgb(255,179,82)" width="300" alt="Bi-directional Search" />
<span style="color:rgb(90,255,100)">Forward Search</span>
<img src="graph7.jpg" style="display:block;margin:0 auto; border: 1px solid rgb(90,255,100)" width="300" alt="Bi-directional Search" />
<span style="color:rgb(255,179,82)">Backward Search</span>
<img src="graph8.jpg" style="display:block;margin:0 auto; border: 1px solid rgb(255,179,82)" width="300" alt="Bi-directional Search" />

In this example the searches collide on the shortest length path, but not the lightest path.

<span style="color:cyan">To fix this:</span> find an $x$ (possible different from w). minimum value of $d_f(x)+d_b(x)$

Minimum value for $d_f(x)+d_b(x)$ over all vertices that have been processed
in at least one search (see figure):
$$
d_f(u)+d_b(u) = 3 + 6 = 9\\
d_f(u')+d_b(u') = 6 + 3 = 9\\
d_f(w)+d_b(w) = 5 + 5 = 10\\
$$

### Goal-Directed Search or A*
Modify edge weights with potential function over vertices.
$$
\bar{w}(p)=w(p) - \lambda_t(s) + \lambda_t(t)
$$
So Shortest paths are maintained in modified graph with $\bar{w}$ weights
(see Figure).

&nbsp;&nbsp;&nbsp;To apply Dijkstra, we need $\bar{w}(u,v) \geq 0$ for all (u,v).
&nbsp;&nbsp;&nbsp;Choose potential function approriately, to be feasible.

### Landmarks
Small set of landmarks $LCV$. For all $u \in V, l \in L$, pre-compute
$\delta(u,l)$.
&nbsp;&nbsp;&nbsp;Potential $\lambda_t^{(l)}(u)=\delta(u,l)- \delta(t,l)$
for each l.

<span style="color:rgb(90,255,100)">CLAIM:</span>$\lambda_t^{(l)}$
is feasible.

### Feasibility

$\bar{w}(u,v) = w(u,v) - \lambda_t^{(l)}(u)+\lambda_t^{(l)}{v}$

$\;\;\;\;\;\;\;\;\;\;\;\;= w(u,v) - \delta(u,l)+\delta(t,l)+\delta(v,l)-\delta(t,l)$

$\;\;\;\;\;\;\;\;\;\;\;\;= w(u,v) - \delta(u,l)+\delta(v,l)\geq 0$ by the $\Delta$-inequality

  $\lambda_t(u)= \text{max}_{{}_{(l\; \in\; L)}} (\lambda_t^{(l)}(u))$


Targeted Search:
<img src="graph9.jpg" style="display:block;margin:0 auto" width="300" alt="Targeted Search" />

Modifying Edge Weights:
<img src="graph10.jpg" style="display:block;margin:0 auto" width="300" alt="Modifying Edge Weights" />
