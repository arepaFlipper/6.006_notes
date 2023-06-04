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

