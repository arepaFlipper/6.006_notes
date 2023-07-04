# Topics in Algorithm Research

<h2 style="color:red">Processor Architecture</h2>

Computer architecture has evolved:
- Intel 8086 (1981): 5MHz (used in first IBM PC)
- Intel 80486 (1989): 25MHz (became i486 because of a court ruling that 
prohibits the trademarking of numbers).

- Pentium (1993): 66MHz
- Pentium 4 (2000): 1.5GHz (deep $\approx$ 30-stage pipeline)
- Pentium D (2005): 3.2GHz (and then the clock speed stopped increasing)
- Quadcore Xeon (2008): 3GHz (increasing number of cores on chip is key 
to performance scaling)
<h4 style="color:red">Processors need data to compute on:</h4>

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

<span style="color:red">Problem</span>: SRAM cannot support more than
$\approx$ 4 memory requests in parallel.



<span style="color:red">$: cache P: processor</span> 

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

Most of the time program running on the processor accesses local or "cache" memory

Every once in a while, it accesses remote memory:

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

<span style="color:red">Round-trip required to obtain data</span>

<h2 style="color:red">Research Idea: Execution Migration</h2>

When program running on a processor needs to access cache memory of another processor,
it migrates its "context" to the remote processor and executes there:

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

<span style="color:red">One-way trip for data access</span>

<span style="color:green">

  Context $= \underbrace{\text{ProgramCounter} + \text{RegisterFile}+\dots}_{\text{few K bits}}$
  (can be larger than data to be accessed)

</span>

Assume we know or can predict the access pattern of a program

|  |    | 
|-------------- | -------------- | 
| $m_1, m_2, \dots, m_{{}_{N}}$    | (memory addresses)     |
| $p(m_{{}_1}), p(m_{{}_2}), \dots, p(m_{{}_N})$    | (processor caches for each $m_{{}_i}$)     |

<h3 style="color:red">Example</h3>

p1 p2 p2 p1 p1 p3 p2

${cost}_{mig}(s,d)=distance(s,d)+L$ 
<m style="color:red">$\leftarrow$ load latency L is a function of context size</m>

${cost}_{access}(s,d)=2*distance(s,d)$ 

if $s==d$, costs are defined to be 0

<h3 style="color:red">Problem</h3>

Decide when to migrate to minimize total memory cost of trace For example:

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

<h4 style="color:red">What can we use to solve this problem?</h4>
<h4 style="color:green">Dynamic programming!</h4>

### Dynamic Programming Solution
Program at $p$, initially, number of processors = $Q$

<h4 style="color:red;fotn-weight:bold">Sub-problems?</h4>

Define <span style="color:green">$DP(k,p_{{}_i})$</span> as cost of 
optimal solution for the prefix $m_{{}_1}, m_{{}_2}, \dots, m_{{}_k}$ of memory
accesses when program starts at $p_{{}_1}$ and ends up at $p_{{}_i}$.

<span style="color:green">

  $DP(k+1,p_{{}_j})= \begin{cases}
    DP(k,p_{{}_j})+cost_{access}(p_{{}_j},p(m_{{}_{k+1}}))  & \text{ if } p{{}_j} \neq p(m_{{}_{k+1}})\\
    MIN_{{}_{i=1}}^Q(DP(k,p_{{}_i})+cost_{mig}(p_{{}_i},p_{{}_j})) & \text{ if } p{{}_j} = p(m_{{}_{k+1}})
  \end{cases}$

</span>

<h4 style="color:red">Complexity?</h4>

<span style="color:green">

$O(\underbrace{N \cdot Q}_{\text{number of sub-problems}}\cdot \underbrace{Q}_{\text{cost per sub-problem}})
= O(N\cdot Q^2)$

</span>

The Demaine's research group is building a 128-processor Execution Migration Machine that
uses a migration predictor based on this analysis.

# Research Areas and Beyond 6.006

#### Erik's Main Research Areas
- computational geometry <m style="color:pink">[6.850]</m>
  - geometric folding algorithms <m style="color:pink">[6.849]</m>
  - self-assembly
- data structures <m style="color:pink">[6.851]</m>
- graph algorithms <m style="color:pink">[6.899]</m>
- recreational algorithms <m style="color:pink">[SP.268]</m>
- algorithmic sculpture

#### Geometric Folding Algorithms: <m style="color:pink">[6.849], Videos Online</m>
Two aspects: dedign and foldability
- <u>design</u>: algorithms to fold any polyhedral surface from a square of paper
<span style="color:pink">[Demaine, Mitchell(2000); Demaine & Tachi(2011)]</span>

  - bicolor paper $\implies$ can 2-color faces
  - <u>OPEN</u>: how to best optimize "scale-factor".
  - e.g. best $n \times n$ checkerboard folding --- recently improved from
  $\approx n/2 \rightarrow n/4$

- <u>foldability</u>: given a crease pattern, can you fold it flat?
  - NP-complete in general <d style="color:pink">Bern & Hayes(1996)</d>
  - <u>OPEN</u>: $m \times n$ <u>map</u> 
  with creases specified as mountain/valley
  <d style="color:pink">[Edmonds (1997)]</d>
  - just solved: $2 \times n$ <d style="color:pink">Demaine,Liu,Morgan(2011)</d>
  - hyperbolic paraboloid <d style="color:pink">[Bauhans (1929)]</d>
  doesn't exist <d style="color:pink">[Demaine, Demaine, Hart, Price, Tachi (2009)]</d>

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

  - understanding circular creases.
  - any straight-line graph can be made by folding flat & one straight cut 
  <d style="color:pink">[Demaine, Demaine, Lubiw (1998), Bern, Demaine, Eppstein, Hayes (1999)]</d>

### Self-Assembly
Geometric model of computation:
- <u>glue</u> e.g. DNA strands, each pair has <u>strength</u>
- <u>square tiles</u> with glue on each side
- <u>Brownian motion</u>: tiles/constructions --- stick together if $\sum glue$
$strengths \geq temperature$
- can build $n \times n$ square using $O\left(\frac{\log_2 n}{ \log_2 (\log_2 n)}\right)$
tiles <d style="color:pink">[Rothemund & Winfree 2000]</d> or using $O(1)$ tiles & $O(\log_2 n)$ "stages"
<d style="color:blue">algorithmic steps by the bioengineer</d>
[<d style="color:pink">Demaine, Demaine, Fekete, Ishaque, Rafalin, Schweller, Souvaine (2007)</d>]

### Data Structures [6.851], Videos Next Semester
There are 2 main categories of data structures
- Integer data structures: store $n$ integers in ${0,1,2,\dots,n-1}$
subject to insert, delete, predecessor, successor (<d style="color:rgb(0,255,255)">on word RAM</d>)
  - <d style="color:green">hashing does exact search in $O(1)$ </d>
  - <d style="color:green">AVL trees do all in $O(\log_2 n)$</d>
  - $O\left(\log_2 (\log_2 n)\right)/op$ <d style="color:pink">van Emde Boas</d>
  - $O\left(\frac{\log_2 n}{\log_2 ({\log_2 n})}\right)/op$ 
  <d style="color:pink">fusion trees: Fredman & Willard</d>
  - $O\left(\sqrt{\frac{\log_2 n}{\log_2 ({\log_2 n})}}\right)$ 
  <d style="color:pink">min of above</d>
- Cache-efficient data structures
  - <u>memory transfers</u> happen in blocks (from cache to disk/main memory)
  - searching takes $\Theta(\log_B N)$ transfers (<d styles="color:rgb(0,255,255)">vs. $\log_2 n$</d>)
  - sorting takes $\Theta(\frac{N}{B} \cdot \log_C \frac{N}{B})$ transfers
  - possible even if you don't know $B$ & $C$!

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

#### (Almost) Planar Graphs: [6.899], Videos Online
- Dijsktra in $O(n)$ time <d style="color:pink">[Henzinger, Klein, Rao, Subramanian, (1997)]</d>
- Bellman-Ford in $O\left(\frac{n\cdot lg^2 n}{\log_2 (\log_2 n)}\right)$ time 
<d style="color:pink">[Mozes & Wolff-Nilson (2010)]</d>
- Many problems NP-hard, even on planar graphs. But can find a solution within $1+\epsilon$

-<h1 style="color:yellow">⚠️⚠️[You owe me a figure here]⚠️⚠️</h1>

factor of optimal, for any $\in$ [Baker 1994 & Others]:

- run BFS from any root vertex $r$
- delete every k layers
- for many problems, solution messed up by only 
$1+\frac{1}{k} factor$ $\left(\implies k = \frac{1}{\epsilon}\right)$
- connected components of remaining graph have $< k$ layers. Can solve via 
$DP$ typically in $\approx 2^k \cdot n$ time.

### Recreational Algorithms
- many algorithms and complexities of games
<m style="color:pink">[some in SP.268 and out book Games, Puzzles & Computation(2009)]</m>

- $n\times n \times n$ Rubik's Cube diameter is $\Theta(\frac{n^2}{\log_2 n})$
<m style="color:pink">[Demaine, Demaine, Eisenstat, Lubiw, Winslow (2011)]</m>

- Tetris is NP-complete 
<m style="color:pink">[Breukelaar, Demaine, Hohenberger, Hoogeboom, Kosters, Liben-Nowell (2004)]</m>

- ballon twisting any polyhedron
<m style="color:pink">[Demaine, Demaine, Hart (2008)]</m>

- algorithmic magic tricks
