**Axioms of the Reals:**

A non-empty set $\mathbb{R}$ together with operations $+$, $\cdot$, and ordering $\leq$ is called the real numbers if it satisfies:

1. $(\mathbb{R}, +, 0)$ is an abelian group.
2. $(\mathbb{R} \setminus \{0\}, \cdot, 1)$ is an abelian group, with $1 \neq 0$.
3. Distributive law: $x \cdot (y + z) = x \cdot y + x \cdot z$.
4. $\leq$ is a total order, compatible with $+$ and $\cdot$, satisfying the Archimedean property.
5. Every Cauchy sequence is a convergent sequence, where:
   $$|x| = \begin{cases} x & \text{if } x \geq 0 \\ -x & \text{if } x < 0 \end{cases}$$

**Example 1: Prove that $a_n = \frac{1}{n}$ converges to 0**

*Step 1*: Rewrite the problem based on the definition of convergence.

Let $\varepsilon > 0$. Then for $n \geq N$, we need to show:
$$|a_n - 0| < \varepsilon$$

*Step 2*: Fill in the calculation.

$$|a_n - 0| = \left|\frac{1}{n}\right| \leq \left|\frac{1}{N}\right| < \varepsilon$$

*Step 3*: Choose $N$ so large that $N \cdot \varepsilon > 1$. This is valid due to the Archimedean property.

**Example 2: Prove that $a_n = (-1)^n$ is divergent**

*Step 1*: Assume the sequence is convergent and rewrite the problem based on the definition of convergence.

Let $\varepsilon > 0$. Then for $n \geq N$:
$$|a_n - a| < \varepsilon$$

*Step 2*: Choose $\varepsilon$ so small that the neighborhood around $a$ will not contain both $-1$ and $1$. We choose $\varepsilon = 1$.

Then for $n \geq N$:
$$|a_N - a| < \varepsilon$$
$$|a_{N+1} - a| < \varepsilon$$

Hence,
$$|1 - a| < \varepsilon$$
$$|(-1) - a| < \varepsilon$$

This leads to a contradiction:
$$2 = |1 - (-1)| = |1-a+a-(-1)| \leq |1-a| + |a-(-1)| < 2$$