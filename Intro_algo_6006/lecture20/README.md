# Dynamic Programming II: Text Justification, Blackjack

- 5 easy steps.
- Text justification.
- Perfect-information Blackjack.
- Patent pointers.

### Summary

<span style="color:red">*</span> $DP \approx$ "careful brute force"

<span style="color:red">*</span> $DP \approx$ guessing + recursion + memoization

<span style="color:red">*</span> $DP \approx$ <span style="color:green">
dividing into reasonable # subproblems whose solutions relate --- acyclicly ---
usually via guessing parts of solution.
</span>

<span style="color:red">*</span> time = (# of subproblems) $\cdot$ 
$\underbrace{(time/subproblem)}_{\text{treating recursive calls as }O(1)}$

- usually mainly guessing
- essentially an amortization.
- count each subproblem only once; after first time, costs $O(1)$ via memoization.

<span style="color:red">*</span> $DP \approx$ shortest paths in some DAG
(directed acyclic graph).

## 5 Easy Steps to Dynamic Programming

| step  | description   | time   |
|-------------- | -------------- | -------------- |
| 1. | define subproblems     | count # subproblems |
| 2. | guess (part of solution) | count # choices |
| 3. | relate subproblem solutions | compute time/subproblem | 
| 4. | recursive + memoize <br/><br/> OR build DP table bottom-up check subproblems acyclic/topological order| time = (time/subproblem) $\cdot$ (# subproblems)  | 
| 5. | solve original problem: = a subproblem OR by combining subproblems solutions | $\implies$ extra time|

| Example  | Fibonacci   | Shortest Paths   |
|-------------- | -------------- | -------------- |
| <u>subprops | $F_k$ for $1\leq k\leq n$ | $\delta_{k}(s,v)$ for $v \in V, 0 \leq k \leq \|V\|=$ min $s \rightarrow v$ path using $\leq k$ edges |
|<span style="color:rgb(255,179,82)">#subprobs</span> |<span style="color:rgb(255,179,82)"> $n$|<span style="color:rgb(255,179,82)"> $V^2$ |
| <u>guess: |nothing| edge into $v$ (if any)|
|<span style="color:rgb(255,179,82)"># choices|<span style="color:rgb(255,179,82)"> $1$|<span style="color:rgb(255,179,82)"> indegree$(v)+1$ |
| <u>recurrence: |$F_{{}_k}= F_{{}_{k-1}} + F_{{}_{k-2}}$|$\delta_{{}_k}(s,v)=$min $\{\delta_{{}_{k-1}}(s,v), \delta_{{}_{k-2}}(s,v) \| (u,v) \in E\}$|
|<span style="color:rgb(255,179,82)">(time/subproblem)|<span style="color:rgb(255,179,82)"> $\Theta(1)$|<span style="color:rgb(255,179,82)"> $\Theta(1+ \text{indegree}(v))$ |
|<u> topo. order:|for $k=1,\dots ,n$|for $k=0,1,\dots \|V\|-1$ for $v \in V$|
|<span style="color:rgb(255,179,82)">total time:|<span style="color:rgb(255,179,82)"> $\Theta(n)$|<span style="color:rgb(255,179,82)"> $\Theta(V\cdot E)$</span> + <span style="color:pink">$\Theta(V^2)$ unless efficient about indeg. 0</span>|
|<u>original problem:|$F_n$|$\delta_{{}_{\|V\|-1}}(s,v)$ for $v \in V$|
|<span style="color:rgb(255,179,82)">extra time:|<span style="color:rgb(255,179,82)"> $\Theta(1)$|<span style="color:rgb(255,179,82)"> $\Theta(V)$|
