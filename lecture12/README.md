# Numerics II: Square Roots, Newtown's Method
- Review:
  - high precision arithmetic.
  - multiplication.
- Division:
  - Algorithm.
  - Error Analysis.
-Termination.

## Review
Want millionth  digit of $\sqrt{2}$
<div style="display:flex; justify-content:space-around">

  $\lfloor \sqrt{2\cdot 10^{2\cdot d}} \rfloor$

  $d=10^6$
</div>

Compute $\lfloor \sqrt{a} \rfloor$ via Newton's Method:

$X_0=1$ (initial guess)

$X_{i+1}=X_i-\frac{f(X_i)}{f'(X_i)}= X_i-\frac{({X_i}^2-a)}{2X_i}= \frac{X_i+\frac{a}{X_i}}{2} \leftarrow division!$

### Error Analysis of Newtown's Method
Suppose $X_n = \sqrt{a}\cdot(1+ \epsilon_{n})$

where $\epsilon_{n}$ may be + or -, then:

$$
\begin{align*}
X_{n+1}&=\frac{X_n+\frac{a}{X_n}}{2} \\[12px]
       &=a\cdot \frac{X_n+\frac{a}{X_n}}{2} \\[12px]
       &=\frac{\sqrt{a}\cdot(1+ \epsilon_{n})+\frac{a}{\sqrt{a}\cdot(1+ \epsilon_{n})}}{2} \\[12px]
       &= \frac{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n}) \cdot \left(\sqrt{a}\cdot(1+ \epsilon_{n})+\frac{a}{\sqrt{a}\cdot(1+ \epsilon_{n})}\right)}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]
       &= \frac{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n}) \cdot \sqrt{a} \cdot (1 + \epsilon_{n}) + 2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n}) \cdot \frac{a}{\sqrt{a}\cdot(1+ \epsilon_{n})}}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]
       &= \frac{2 \cdot \sqrt{a} \cdot \sqrt{a} \cdot (1 + \epsilon_{n}) \cdot (1 + \epsilon_{n}) + 2 \cdot a}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]
       &= \frac{2 \cdot a \cdot (1 + \epsilon_{n})^2 + 2 \cdot a}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]

       &= \frac{2 \cdot a \cdot \left((1 + \epsilon_{n})^2 + 1\right)}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]

       &= \frac{2 \cdot a \cdot (1 + 2 \cdot \epsilon_{n} + \epsilon_{n}^2 + 1)}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]

&= \frac{2 \cdot a \cdot (2 + 2 \cdot \epsilon_{n} + \epsilon_{n}^2)}{2 \cdot \sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]

&= \frac{a \cdot (1 + \epsilon_{n} + \frac{\epsilon_{n}^2}{2})}{\sqrt{a} \cdot (1 + \epsilon_{n})} \\[12px]

&= \sqrt{a} \cdot \left(1 + \frac{{\epsilon_{n}}^2}{2 \cdot (1+\epsilon_{n})}\right) \\[12px]
X{n+1}&= \sqrt{a} \cdot \left(1 + \frac{{\epsilon_{n}}^2}{2 \cdot (1+\epsilon_{n})}\right) \\[12px]
\end{align*}
$$

Therefore:
$$
\epsilon_{n+1}=\frac{{\epsilon_{n}}^2}{2\cdot(1+\epsilon_{n})}
$$

Quadratic convergence, as the number of correct digits doubles each step.
  Newton's method requires high-precision division. We covered multiplication in Lecture 11.

### Multiplication Algorithms:
1. Naive Divide & Conquer method: $\Theta(d^2)$ time.
2. Karatsuba method: $\Theta(n^{\log_2(3)})=\Theta(n^{1.584\dots})$ time.
$$
T(n)=3\cdot T\left(\frac{n}{2}\right)+\Theta(n) 
$$
3. Toom-Cook generalizes Karatsuba (break into $k \geq 2$ parts)
$$
T(n)=5\cdot T\left(\frac{n}{3}\right)+\Theta(n) = \Theta(n^{\log_3(5)}) = \Theta(n^{1.465\dots})
$$
4. Schönhage–Strassen - almost linear! $\Theta(d \cdot log_2(d) \cdot log_2(log_2 (d)))$ using FFT(Fast Fourier Transform).
All of these are in [gmpy](https://pypi.org/project/gmpy/) package in Python.
5. Furer (2007): $\Theta(n \cdot log_2(n) \cdot 2^{O(log^* n)})$ where $log^*$ is iterated logarithm. 
The number of times $\log$ needs to be applied to get a number that is less than or equal to 1.

### High Precision Division
We want high precision rep of $\frac{a}{b}$
- Compute high-precision rep of $\frac{1}{b}$ first.
- High-precision rep of $\frac{1}{b}$ means $\lfloor \frac{R}{b} \rfloor$ where $R$ is large value
s.t. it is easy to divide by $R$.

  Ex: $R=2^k$ for binary representations.

### Division Algorithm
Newton's Method for computing $\frac{R}{b}$:

<div style="display:flex; justify-content:space-around">

<span>$f(x)=\frac{x}{b}$</span>

<span>$f'(x)=\frac{-1}{x^2}$</span>

<span>(zero at $x=\frac{R}{b}$)</span>

</div>

$$
\begin{align*}
X_{i+1}&= X_i - \frac{f(X_i)}{f'(X_i)} = X_i - \frac{(\frac{1}{X_i} - \frac{b}{R})}{-\frac{1}{{X_i}^2}}\\[12px]
X_{i+1}&= X_i + {X_i}^2 \cdot \left(\frac{1}{X_i} - \frac{b}{R}\right) = 2X_i + \frac{b\cdot {X_i}^2}{R}\\[12px]
X_{i+1} &= 2X_i +\frac{b\cdot {X_i}^2}{R}
\end{align*}
$$

##### Example

Want $\frac{R}{b}=\frac{2^{16}}{5}=\frac{65536}{5}= 13107.2$

Try initial guess $\frac{2^{16}}{4}=2^{14}$

$$
X_0 = 2^{14}=16384 \\[12px]
X_1 = 2^{16384} - \frac{5\cdot(16384)^2}{65536}=12288\\[12px]
X_2 = 2^{12288} - \frac{5\cdot(12288)^2}{65536}=13056\\[12px]
X_3 = 2^{13056} - \frac{5\cdot(13056)^2}{65536}=13107\\
$$

###### Error Analysis

$$
\begin{align*}
X_{i+1}&= 2\cdot X_i - \frac{b\cdot {X_i}^2}{R} \text{ Assume } X_i = \frac{R}{b} \cdot{(1+\epsilon_i)} \\[12px]
&= 2 \cdot \frac{R}{b} \cdot{(1+\epsilon_i)} - \frac{b}{R} \cdot \left(\frac{R}{b}\right)^2 \cdot (1+\epsilon_i)^2 \\[12px]
&= \frac{R}{b} \left((2+2\cdot\epsilon_i) - (1 + 2\epsilon_i+ \epsilon_i^2) \right) \\[12px]
&= \frac{R}{b} \left(1 -{\epsilon_i}^2\right) \\[12px]
&= \frac{R}{b} \left(1 + \epsilon_{i+1}\right) \text{, Where } \epsilon_{i+1} = -{\epsilon_i}^2
\end{align*}
$$

Quadratic convergence: The number of digits doubles each step.

One might think that the complexity of division is $\log_2(d)$ times the complexity of
multiplication given that we will have $\log_2(d)$ multiplications in the $\log_2(d)$ 
iterations required to reach precision d. However, the complexity of dicision equals the 
complexity of multiplication.

To understand this, assume that the complexity of multiplication is $\Theta(n^\alpha)$ for
$n-digit$ numbers, with $\alpha \geq 1$. Division requires multiplication of different-sized
numbers at each iteration. Initially the numbers are small, and then they grow to d-digits.
The number of operations in division are:


$$
c\cdot 1^\alpha + c\cdot 2^\alpha + c\cdot 4^\alpha + \cdots + c\cdot\left(\frac{d}{4}\right)^\alpha
+ c \cdot \left(\frac{d}{2}\right)^\alpha + c \cdot {d}^\alpha
$$

Constants times $d^{\alpha}$:

$$
c\cdot 1^\alpha + c\cdot 2^\alpha + c\cdot 4^\alpha + \cdots + c\cdot\left(\frac{d}{4}\right)^\alpha
+ c \cdot \left(\frac{d}{2}\right)^\alpha + c \cdot {d}^\alpha < 2c \cdot {d}^\alpha
$$

Complexity of division $\cong$ Complexity of multiplication

### Complexity of Computing Square Roots
We apply a first level of Newton's method to solve $f(x)=x^2-a$. Each iteration of this first level requires
a division. If we set the precision to d-digits right from the beginning, then convergence at 
the first level will require $\log_2(d)$ iterations. This means the complexity of computing a 
square root will be $\Theta(d^{\alpha}\cdot log_2(d)$ if the complexity of multiplication is 
$\Theta(d^{\alpha})$, given that we have shown that the complexity of division is the same as the complexity 
of multiplication.

However, we can do better, if we recognize that the number of digits of precision
we need at beginning of the first level of Newton's nethod starts out small and then
grows. If the complexity of a d-digit division is $\Theta(d^{\alpha})$, then a similar summation to 
the one above tells us that the complexity of computing square roots is $\Theta(d^{\alpha})$.

### Termination
Iteration: $X_{i+1} = \lfloor \frac{ X_i+ \lfloor \frac{a}{X_i}\rfloor}{2} \rfloor$

Do floors hurt? Does program terminate? ($\alpha$ and $\beta$ are the fractional parts below.)
Iteration is:

$$
\begin{align*}
X_{i+1}&=\frac{X_i+\frac{a}{X_i}-\alpha}{2}-\beta \\[12px]
&=\frac{X_i+\frac{a}{X_i}}{2}- \gamma \text{, where } \gamma = \frac{\alpha}{2}+ \beta \text{and } 0 \leq \gamma \leq 1
\end{align*}
$$

Since $\frac{a+b}{2}\geq \sqrt{a\cdot b}, \frac{X_i + \frac{a}{X_i}}{2} \geq \sqrt{a}$, so 
substracting $\gamma$ always leaves us $\geq \lfloor \sqrt{a} \rfloor$. This won't stay stuck
above if $\epsilon_i \leq 1$ (good initial guess).
