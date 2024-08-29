Definition of Abelian group
1. Group is a set of elements that has a binary relation \* that satisfies three properties :
	1. Associativity : (a \* b) \* c = a \* (b \* c)
	2. Existence of identity element e s.t. : a \* e = e\* a = a
	3. Existence of inverse element $a^{-1}$ s.t. : $a * a^{-1} = a^{-1} * a = e$
2. Abelian means it is commutative
	1. a \* b = b \* a

Axioms of the reals: A non-empty set $\mathbb{R}$ together with operations +, $\cdot$ and ordering $\leq$ is called the real numbers if it satisfies:

1. $\quad (\mathbb{R}, +, 0) \quad$is an abelian group
2. $\quad (\mathbb{R} \setminus \{0\}, \cdot, 1) \quad \text{is an abelian group} \quad (1 \neq 0)$
3. $\quad \text{distributive law} \quad x \cdot (y + z) = x \cdot y + x \cdot z$
4. $\quad \leq \text{ is a total order, compatible with } + \text{ and } \cdot, \text{ Archimedean property}$
5. $$\quad \text{Every Cauchy sequence is a convergent sequence.} \quad |x| := \begin{cases} 
      x & \text{if } x \geq 0 \\
      -x & \text{if } x < 0 
    \end{cases}$$

The sequence $a_n$ convergences to a if for ALL given positive epsilon neighborhood around a, there exists and we can find a natural number N such that beyond this natural number N all the $a_{n>N}$ will lie within the epsilon neighborhood :

A sequence $(a_n)_{n \in \mathbb{N}}$ is called convergent to  $a \in \mathbb{R}$ \ if $\forall \varepsilon > 0, \; \exists N \in \mathbb{N}, \; \forall n \geq N : |a_n - a| < \varepsilon$

E.g. 1 Prove that $(a_n = 1/n)_{n->\infty} = 0$

Step 1 : Rewrite the problem based on the definition of convergence

Let $\varepsilon > 0$

Then for $n\geq N$ (Missing the definition of N), we need to show 

(Missing calculation to reach the below result)

$|a_n - 0| < \varepsilon$

Step 2 : Fill in the calculation 

Let $\varepsilon > 0$

Then for $n\geq N$ (Missing the definition of N), we need to show 

$|a_n - 0| = |1/n| \leq |1/N| < \varepsilon$

Step 3 : Choose N so large that $N * \varepsilon > 1$
This is valid due to Archimedean property (No matter how small the epsilon is we can exceed any number we want by finitely adding the number many times)

E.g. 2 Prove that $(a_n = -1^n)_{n->\infty}$ is divergent (Prove by contradiction)

Step 1 : Assume that the sequence is convergent and rewrite the problem based on the definition of convergence

Let $\varepsilon > 0$

Then for $n\geq N$, 

$|a_n - a| < \varepsilon$

Step 2 : We have to choose the $\varepsilon$ to be so small that the neighbourhood around a will not have -1 and 1 at the same time, so we need $\varepsilon$ to be smaller than or equals to 1 (we choose 1)

Let $\varepsilon = 1$

Then for $n\geq N$, 

$|a_N - a| < \varepsilon$
$|a_{N+1} - a| < \varepsilon$

Hence,

$|1 - a| < \varepsilon$
$|(-1) - a| < \varepsilon$

$2 = |1 - (-1)| = |1-a+a-(-1)| \leq |1-a| + |a-(-1)| = |1-a| + |(-1)-a| < 2$
which is a contradiction
