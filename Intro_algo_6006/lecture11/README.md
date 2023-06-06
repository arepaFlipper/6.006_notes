# Numerics I: Integer Arithmetic, Karatsuba Multiplication

- Irrationals
- Newton's method $(\sqrt(a), \frac{1}{b})$
- High precision multiply.

## Irrationals
Pythagoras discovered that a square's diagonal and its side
are incommensurable, i.e., could not be expressed as a ratio -
he called the ratio "speecheless"!

<img src="pic0.jpg" style="width: 300px;display: block;margin-left: auto;margin-right: auto" alt="Ratio of a Square's Diagonal to its Sides">

> Pythagoras worshipped numbers "_All is a number_". Irrationals
were a threat!

#### Motivating Question:
Are there hidden patterns in irrationals?

$\sqrt(2)= 1.$ $414$ $213$ $562$ $373$ $095$ $048$ $801$ $688$ $724$ $209$ $698$ $078$ $569$ $671$ $875$ 

Can you see a pattern?

#### Digression
##### Catalan numbers:
Set P of <u>balanced</u> parentheses strings are recursively defined as:

- $\lambda \in P$ ($\lambda$ is empty string)
- If $\alpha$, $\beta \in P$, then ($\alpha \cdot \beta) \in P$ 

Every noneempty balanced parent string can be obtained via Rule 2 
from a unique $\alpha,\beta$ pair.

For Example: <span style="color:cyan">(()) () ()</span> obtained by :
<span style="color:cyan">$(\underbrace{()}_{\alpha})$   $\underbrace{()()}_{\beta}$</span>

### Enumeration
- $C_{n}:$ number of balanced parentheses strings with exactly $n$ pairs of parentheses 
$C_{0}=1$ empty string.

- $C_{n+1}?:$ Every string with n+1 pairs of parentheses can be obtained in a unique 
way via rule 2.

One paren pair comes explicitly from the rule.
$k$ pairs from $\alpha$, $n - k$ pairs from $\beta$.

$C_{n+1} =\displaystyle \sum_{k=0}^{n} C_k \cdot C_{n-k}$ 

where:
- $n\geq 0$
- C_0 = 1
- $C_1 = {C_{0}}^2 = 1$
- $C_2 = C_{0} \cdot C_1 + C_1 \cdot C_{0} = 2$
- $C_3 = \dots = 5$

$1,1,2,5,14,42,132, 429,1430, 4862, 16796, 58786, 208012, 742900, 2674440, 9694845,$ 
$35357670, 129644790, 477638700, 1767263190, 6564120420, 24466267020, 91492563640,$ 
$343059613650, 1289904147324, 4861946401452, 18367353072152, 69533550916004,$
$263747951750360, 1002242216651368$

### Newton's Method
Find root of $f(x)=0$ through successive approximation e.g., $f(x)=x^2-a$

<img src="pic2.jpg" style="width: 300px;display: block;margin-left: auto;margin-right: auto" alt="Newton's Method">

Tangent at $x_i,f(x_i)$ is line $y=f(x_i)+$ 
<u>$f'(x_i)$</u>
$\cdot(x-x_i)$

where $f'(x_i)$ is the derivative.

$x_{i+1}:$ intercept on x-axis


<div style="display:flex; justify-content:center">

<span style="width:auto; padding: 10px;color:cyan;font-weight:bold;display:flex;justify-content:center;border:1px solid cyan"> $x_{i+1}=x_i-\frac{f(x_i)}{f'(x_i)}$ </span>

</div>

### Square Roots

<div style="display:flex; justify-content:center">

$f(x)=x^2 -a$

</div>
<div style="display:flex; justify-content:center">

$x_{i+1}=x_i-\frac{f(x_i)}{f'(x_i)}=x_i-\frac{({x_i}^2-a)}{2x_i}=\frac{x_i+\frac{a}{x_i}}{2}$

</div>

Example:

$a=2$

|  iteration   | value   |
| ------------ | --------- |
| $x_0$    | 1.000000000     |
| $x_1$    | 1.500000000     |
| $x_2$    | 1.416666665     |
| $x_3$    | 1.414215686     |
| $x_3$    | 1.414213562     |


Quadratic convergence, # digits doubles. Of course, in order to use Newton's method,
we need high-precision division. We'll start with multiplication and cover division in 
Lecture 12.

### High Precision Computation

$\sqrt{2}$ to d-digit precision: $\underbrace{1.414213562372}_{d-digits} \dots$

Want integer $\left[10^d\cdot \sqrt{2}\right]=\left[\sqrt{2 \cdot 10^{2d}}\right]$ --- 
integral part of square root Can still use Newton's Method.

### High Precision Multiplication

Multiplying two n-digit numbers (radix r=2,10) $0\leq x,y< r^n$

$x_1:$ high half

$x_0:$ low half

$x= x\cdot r^{\frac{n}{2}}+x_0$ 

$y= y\cdot r^{\frac{n}{2}}+y_0$

$0 \leq x_0,x_1 < r^{n/2}$

$0 \leq y_0,y_1 < r^{n/2}$

$z = x\cdot y
= x_1\cdot y_1\cdot r^n+(x_0\cdot y_1 +x_1\cdot y_0)\cdot r^{\frac{n}{2}}+ x_0 \cdot y_0$

4 multiplications of half-sized numbers $\implies$ quadratic algorithm $\Theta(n^2)$ time.

## karatsuba's Method

![branching factors 4T](pic3.jpg)
<span style="color:rgb(135,69,69);display:flex;justify-content:center">$4T\cdot(\frac{n}{2})$</span>
<span style="color:rgb(135,69,69);display:flex;justify-content:center">$4^{\log_2 n}=n^{log_2 4}=n^{2}$</span>

![branching factors 3T](pic4.jpg)
<span style="color:rgb(40,135,88);display:flex;justify-content:center">$3T\cdot(\frac{n}{2})$</span>
<span style="color:rgb(40,135,88);display:flex;justify-content:center">$3^{\log_2 n}=n^{log_{2} 3}$</span>


Let 

<span style="color:rgb(250,127,177)">

$z_0=x_0 \cdot y_0$

</span>

<span style="color:rgb(193,243,0)">

$z_2=x_1 \cdot y_1$

</span>

<span style="color:rgb(243,186,56)">

$z_1= (x_0 + x_1) \cdot (y_0 + y_1) - z_2 - z_0= x_0\cdot y_1 + x_1 \cdot y_0$

$z_1 = (x_0 \cdot y_0 + x_0 \cdot y_1 + x_1 \cdot y_0 + x_1 \cdot y_1) - z_2 - z_0$

$z_1 = x_0 \cdot y_0 + x_0 \cdot y_1 + x_1 \cdot y_0 + x_1 \cdot y_1 - z_2 - z_0$

$z_1 = x_0 \cdot y_1 + x_1 \cdot y_0$

</span>

$z=$ 
<span style="color:rgb(193,243,0)">
$z_2$ 
</span>
$\cdot r^{n} +$ 
<span style="color:rgb(243,186,56)">
$z_1$ 
</span>
$\cdot r^{n/2}+$ 
<span style="color:rgb(250,127,177)">
$z_0$
</span>

There are three multiplies in the above calculations.

<span style="color:rgb(40,135,88);display:flex;justify-content:center">

$T(n):$ time to multiply two n-digit numbers

</span>
<span style="color:rgb(40,135,88);display:flex;justify-content:center">

$T(n)= 3T(\frac{n}{2}+ \Theta(n))$

</span>
<span style="color:rgb(40,135,88);display:flex;justify-content:center">

$T(n)= \Theta(n^{log_2{3}})$

</span>
<span style="color:rgb(40,135,88);display:flex;justify-content:center">

$T(n)= \Theta\left(n^{1.5849625\dots}\right)$

</span>

This is better than $\Theta(n^{2})$. Python does this, and more (see Lecture 12).

## Fun Geometry Problem
