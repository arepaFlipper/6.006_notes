# Dynamic Programming I: Memoization, Fibonacci, Shortest Paths, Guessing

- Memoization and subproblems.
- Examples:
  - Fibonacci.
  - Shortest Paths.
- Guessing & DAG view.

## Dynamic Programming (DP)
<span style="color:rgb(90,255,100)">Big idea, hard, yet simple</span>

- Powerful algorithmic design technique.
- Large class of seemingly exponential problems have a polynomial solution
("only") via DP.
- Particularly for optimization problems (min/max) 
<span style="color:rgb(90,255,100)">(e.g. shortest paths)</span>.

<span style="color:red">*</span> $DP \approx$ "controlled brute force"

<span style="color:red">*</span> $DP \approx$ recursion + re-use

#### History 
##### Richard E. Bellman (1920 - 1984)
> Richard Bellman received the IEEE Medal of Honor, 1979. "Bellman ...
explained that he invented the name 'dynamic programming' to hide the
fact that he was doing mathematical research at RAND under a Secretary
of Defense who 'had a pathological fear and hatred of the term, research'.
He settled on the term 'dynamic programming' because it would be difficult
to give a 'pejorative meaning' and because 'it was something not even a 
Congressman could object to'" [John Rust 2006]

### Fibonacci Numbers
$$
F_1=F_2=1 \; \;\; \;\; \;\; \; Fn=F_{n-1}+F_{n-2}
$$

<span style="color:cyan">Goal:</span> compute $F_n$

### Naive Algorithm

<span style="color:blue">follow recursive definition</span>

```
fib(n):
  if n < 2:
    return n
  else:
    return fib(n-1) + fib(n-2)
```
<span style="font-size:0.75em">

$$
\implies T(n)=T(n-1)+T(n-2) + O(1) \geq F_n \approx \varphi^n
\geq 2T(n-2) +O(1) \leq 2^(n/2)
$$

</span>

  <u style="display:flex;color:red; justify-content: center">Exponential --- BAD!</u>
