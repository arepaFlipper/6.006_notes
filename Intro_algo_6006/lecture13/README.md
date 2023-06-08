# Graphs I: Breadth First Search
- Applications of Graph Search.
- Graph Representations.
- Breadth-First Search

---
##### Recall:

Graph $G=(V,E)$
- $V$: set of vertices (arbitrary labels)
- $E$: set of edges i.e. vertex pairs $(v,w)$ 
  - <span style="color:rgb(121,202,66)">ordered pair $\implies$ <u>directed</u> edge of graph.</span>
  - <span style="color:rgb(255,154,223)">unordered pair $\implies$ <u>undirected</u> edge of graph.</span>

  ![Example to illustrate graph terminology](graph0.jpg)


## Graph Search
"Explore a graph", e.g.:
- <span style="color:rgb(121,202,66)">find a path from start vertex $s$ to a desired vertex</span>
- <span style="color:rgb(121,202,66)">visit all vertices or edges of graph, or only those 
reachable from $s$</span>

### Applications:
<span style="color:rgb(121,202,66)">There are many.</span>
- Web crawling (<span style="color:rgb(121,202,66)">how Google finds pages</span>)
- Social networking (<span style="color:rgb(121,202,66)">Facebook friend finder</span>)
- Network bradcast routing.
- Garbage collection.
- Model checking (<span style="color:rgb(121,202,66)">finite state machine</span>)
- checking mathematical conjectures
- solving puzzles and games.

#### Pocket Cube:
Consider a 2 x 2 x 2 Rubik's cube:

<img src="Rubik.png" alt="Rubik's cube" width="100" height="100" />

Configuration Graph:
- vertex for each possible state.
- Edge for each basic move (e.g., 90 degree turn) from one state to another.
- undirected: moves are reversible

Diameter ("God's Number")

<span style="color:cyan">11 for $2$ x $2$ x $2$, 20 for $3$ x $3$ x $3$, $\Theta\left(\frac{n^2}{log_2(n)}\right)$
for $n$ x $n$ x $n$</span> <span style="color:rgb(255,154,223)">[Demaine, Demaine, Eisenstat, Lubiw winslow 2011]</span>
