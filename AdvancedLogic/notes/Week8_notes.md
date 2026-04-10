# Week 8: Łukasiewicz Logic and McNaughton's Theorem

## Learning Objectives

By the end of this week, you should be able to:

- [ ] Explain why Łukasiewicz logic $\mathbb{F}^3$ is the preferred fuzzy logic among $\mathbb{F}^1$, $\mathbb{F}^2$, $\mathbb{F}^3$, citing the Law of Excluded Middle and the expressibility of $\min$/$\max$ as defined operators
- [ ] State McNaughton's Theorem (1 variable) precisely, including the characterisation of representable functions as continuous piecewise-linear functions with integer coefficients
- [ ] Prove (by induction on formula size) that every one-variable propositional formula in $\mathbb{F}^3$ has a truth function that is a continuous piecewise-linear function with integer coefficients (Lemma 8.2)
- [ ] State and apply Lemma 8.3: for any integers $n, m \in \mathbb{Z}$, the function $f_{n,m}(x) = [nx+m]^1_0$ is the truth function of some formula $F_{n,m}$
- [ ] Apply the tree algorithm to construct a propositional formula whose truth function is any given $[nx+m]^1_0$
- [ ] Explain (at the level of Lemma 8.4, optional) how wedge-shaped functions allow the converse direction of McNaughton's Theorem
- [ ] Verify identities in Łukasiewicz semantics $\mathbb{F}^3$, including De Morgan's laws and the implication-disjunction equivalence
- [ ] Plot and identify inflexion points of truth functions of propositional formulas under $\mathbb{F}^3$
- [ ] Construct formulas for piecewise linear functions by combining sub-formulas via $\vee$ (max) and $\wedge$ (min)
- [ ] Prove that the intersection of two partitions of $[0,1]$ is again a partition

---

## 1. Argument for Łukasiewicz Logic

### 1.1 The Three Fuzzy Semantics

Recall the three fuzzy logics under consideration:

| Logic | $F_\neg(x)$ | $F_\wedge(x,y)$ | $F_\vee(x,y)$ | $F_\to(x,y)$ |
|-------|------------|-----------------|----------------|---------------|
| $\mathbb{F}^1$ | $1-x$ | $\min(x,y)$ | $\max(x,y)$ | $\max(1-x,y)$ |
| $\mathbb{F}^2$ | $1-x$ | $xy$ | $x+y-xy$ | $1-x+xy$ |
| $\mathbb{F}^3$ | $1-x$ | $\max(0, x+y-1)$ | $\min(1, x+y)$ | $\min(1, 1-x+y)$ |

### 1.2 The Law of Excluded Middle (LEM)

**(LEM):** $x \vee \neg x = 1$, for all $x \in [0,1]$.

**Checking $\mathbb{F}^3$:**
$$F^3_\vee(x, 1-x) = \min(1, x + (1-x)) = \min(1,1) = 1 \quad \checkmark$$

**Showing $\mathbb{F}^1$ fails LEM:** Under $\mathbb{F}^1$, $x \vee \neg x = \max(x, 1-x)$. For $x = \frac{1}{2}$: $\max(\frac{1}{2}, \frac{1}{2}) = \frac{1}{2} \neq 1$. So $\mathbb{F}^1$ fails LEM.

**Showing $\mathbb{F}^2$ fails LEM:** Under $\mathbb{F}^2$, $x \vee \neg x = x + (1-x) - x(1-x) = 1 - x(1-x)$. For $x = \frac{1}{2}$: $1 - \frac{1}{4} = \frac{3}{4} \neq 1$. So $\mathbb{F}^2$ fails LEM.

Therefore, if we require (LEM) as a desirable property, we must use $\mathbb{F}^3$.

### 1.3 Łukasiewicz Logic Extends Godel Logic

In $\mathbb{F}^3$, we can introduce two **additional binary operators** $\underline{\wedge}$ and $\underline{\vee}$ as **abbreviations** (defined operators):

$$x \underline{\wedge} y = (x \to y) \wedge x \qquad \text{and} \qquad x \underline{\vee} y = (x \to y) \to y$$

These satisfy:
$$\underline{\wedge}(x,y) = \min(x,y) \qquad \text{and} \qquad \underline{\vee}(x,y) = \max(x,y)$$

for all $x, y \in [0,1]$ under $\mathbb{F}^3$.

This means **$\mathbb{F}^3$ is an extension of $\mathbb{F}^1$**: any reasoning done in $\mathbb{F}^1$ can be replicated in $\mathbb{F}^3$ by replacing $\wedge$ with $\underline{\wedge}$ and $\vee$ with $\underline{\vee}$ (formally, there is a translation $t : \text{Fml} \to \text{Fml}$ such that $\models^{\mathbb{F}^1} F$ iff $\models^{\mathbb{F}^3} t(F)$).

However, no such translation exists for $\mathbb{F}^2$ inside $\mathbb{F}^3$: no finite combination of $\mathbb{F}^3$ connectives can exactly mirror the behaviour of the $\mathbb{F}^2$ t-norm and t-conorm. This motivates the need for a **characterisation result** — McNaughton's Theorem.

---

## 2. McNaughton's Theorem (Part 1) — The Characterisation

### 2.1 Classical Analogy

In classical propositional logic, every Boolean function $f : \{0,1\}^n \to \{0,1\}$ is the truth function of some formula (constructible via DNF):
$$F := \bigvee_{f(x_1,\ldots,x_n)=1} \left(\bigwedge_{i=1}^n p_i^{x_i}\right)$$
where $p_i^1 = p_i$ and $p_i^0 = \neg p_i$.

For arbitrary real-valued functions $f : [0,1]^n \to [0,1]$, the same is **not** true under Łukasiewicz semantics (there are $2^{2^{\aleph_0}}$ such functions but only $\aleph_0$ formulas). So we ask: **which** functions can be represented?

### 2.2 The Clamping Notation

Define:
$$[z]^1_0 = \min(1, \max(0, z))$$

This **clamps** (restricts) the value of $z$ to the interval $[0,1]$. It is the unique function satisfying:
$$[z]^1_0 = \begin{cases} 0 & \text{if } z \leq 0 \\ z & \text{if } 0 \leq z \leq 1 \\ 1 & \text{if } z \geq 1 \end{cases}$$

### 2.3 McNaughton's Theorem (1 Variable)

**Theorem 8.1 (McNaughton, 1951):** A one-variable real-valued function $f : [0,1] \to [0,1]$ can be represented by a propositional formula under Łukasiewicz semantics **if and only if** there exists an increasing sequence

$$0 = \gamma_0 < \gamma_1 < \gamma_2 < \cdots < \gamma_n = 1$$

and a family of **linear functions** $f_i(x) = a_i x + b_i$ with $a_i, b_i \in \mathbb{Z}$ (integer coefficients), for $i < n$, such that:

$$f(x) = f_i(x) = a_i x + b_i \quad \text{for all } x \in [\gamma_i, \gamma_{i+1}]$$

and the pieces join continuously: $f_i(\gamma_{i+1}) = f_{i+1}(\gamma_{i+1})$ for all $i < n$.

In other words, a function is representable in $\mathbb{F}^3$ **if and only if** it is a **continuous piecewise-linear function with integer coefficients**.

---

## 3. McNaughton's Theorem (Part 2) — Proof of Lemma 8.2

### 3.1 Statement of Lemma 8.2

**Lemma 8.2:** The truth function of every one-variable propositional formula is a **continuous piecewise-linear function with integer coefficients**.

This establishes the "only if" direction of McNaughton's Theorem (every representable function has integer-coefficient piecewise-linear truth function).

### 3.2 Proof by Induction on Formula Size

**Base case ($n = 1$):** If $F = p$ (a single propositional variable), its truth function is $\overline{F}(x) = x$. Take $0 = \gamma_0 < \gamma_1 = 1$ with $f_0(x) = 1 \cdot x + 0$, giving integer coefficients $a_0 = 1, b_0 = 0$. ✓

**Induction hypothesis:** Suppose the result holds for all formulas of length $< n$. Let $\varphi$ have length $n$.

**Case 1: $F = \neg G$.**

By the induction hypothesis, $G$ has a continuous piecewise-linear truth function: there exist $0 = \gamma_0 < \gamma_1 < \cdots < \gamma_t = 1$ and $g_i(x) = a_i x + b_i$ (with $a_i, b_i \in \mathbb{Z}$) such that $\overline{G}(x) = g_i(x)$ for $x \in [\gamma_i, \gamma_{i+1}]$.

Define $f_i(x) = a'_i x + b'_i$ where $a'_i = -a_i \in \mathbb{Z}$ and $b'_i = (1 - b_i) \in \mathbb{Z}$. Then for $x \in [\gamma_i, \gamma_{i+1}]$:
$$\overline{F}(x) = 1 - \overline{G}(x) = 1 - g_i(x) = 1 - (a_i x + b_i) = (-a_i)x + (1-b_i) = f_i(x) \quad ✓$$

**Case 2: $F = G \wedge H$.**

By induction, $G$ has partition $\{\gamma_i\}$ and linear pieces $g_i(x) = a_i x + b_i$, while $H$ has partition $\{\delta_j\}$ and linear pieces $h_j(x) = c_j x + d_j$. WLOG assume both partitions are identical (refine if needed). Then:
$$\overline{F}(x) = \max(0, \overline{G}(x) + \overline{H}(x) - 1) = \max(0, (a_i + c_i)x + (b_i + d_i - 1))$$

So $f_i(x) = (a_i + c_i)x + (b_i + d_i - 1)$ has integer coefficients $a_i + c_i \in \mathbb{Z}$ and $b_i + d_i - 1 \in \mathbb{Z}$. ✓

**Case 3: $F = G \vee H$.**

Under $\mathbb{F}^3$: $F \equiv \neg(\neg G \wedge \neg H)$. So this follows immediately from Cases 1 and 2.

**Case 4: $F = G \to H$.**

Under $\mathbb{F}^3$: $F \equiv \neg G \vee H \equiv \neg(\neg(\neg G) \wedge \neg H) = \neg(G \wedge \neg H)$. Follows from the previous cases.

Since $F$ is inductively closed, the result holds for all formulas. $\square$

---

## 4. McNaughton's Theorem (Part 3) — Lemma 8.3 and the Tree Algorithm

### 4.1 Lemma 8.3

**Lemma 8.3:** Let $n, m \in \mathbb{Z}$ be integers and define $f_{n,m} : [0,1] \to [0,1]$ by:

$$f_{n,m}(x) = [nx + m]^1_0$$

Then there exists a one-variable propositional formula $F_{n,m}$ whose truth function is $f_{n,m}$.

This establishes the converse direction for the building blocks: any **single** clamped linear function with integer coefficients is representable.

### 4.2 Proof of Lemma 8.3 by Induction

**Base case ($n = 0$):**

$$f_{0,m}(x) = [m]^1_0 = \begin{cases} 0 & \text{if } m \leq 0 \\ 1 & \text{if } m \geq 1 \end{cases}$$

Take $F_{0,m} = \bot$ (contradiction, e.g. $p \wedge \neg p$) if $m \leq 0$, and $F_{0,m} = \top$ (tautology, e.g. $p \vee \neg p$) if $m \geq 1$.

**Induction hypothesis:** Suppose the result holds for all $n, m \in \mathbb{Z}$ with $|n| \leq k$.

**Induction step for $n = k+1 > 0$:**

Let $f_{k,m}(x) = [kx + m]^1_0$ and $f_{k,(m+1)}(x) = [kx + (m+1)]^1_0$.

By the IH, there are formulas $F_{k,m}$ and $F_{k,(m+1)}$ with these truth functions. Define:

$$F_{(k+1),m} = (F_{k,m} \vee p) \wedge F_{k,(m+1)}$$

**Verification** (by considering behaviour over key intervals):

The intervals $\left[0, \frac{-m}{k+1}\right]$, $\left[\frac{-m}{k+1}, \frac{-m}{k}\right]$, $\left[\frac{-m}{k}, \frac{-(m-1)}{k+1}\right]$, $\left[\frac{-(m-1)}{k+1}, 1\right]$ partition $[0,1]$.

- On $\left[0, \frac{-m}{k+1}\right]$: $f_{k,m}(x) + x = 0 + x = x$ (both components in "zero" region).
- On $\left[\frac{-(m-1)}{k+1}, 1\right]$: $f_{k,(m+1)}(x) = 1$ so $(F_{k,m} \vee p) \wedge F_{k,(m+1)} = F_{k,m} \vee p$.
- On $\left[\frac{-m}{k}, \frac{-(m-1)}{k+1}\right]$: $f_{k,m}(x) + x = (kx+m) + x = (k+1)x + m = f_{(k+1),m}(x)$ as required.

**Induction step for $n = -(k+1) < 0$:**

Use $f_{-k,m}$ and $f_{-k,(m-1)}$, and define:

$$F_{-(k+1),m} = (F_{-k,(m-1)} \vee \neg p) \wedge F_{-k,m}$$

**Hence it follows by induction** that for all $n, m \in \mathbb{Z}$ there is some formula $F_{n,m}$ whose truth function is $f_{n,m}(x) = [nx+m]^1_0$. $\square$

### 4.3 The Tree Algorithm (Step-by-Step)

The inductive proof of Lemma 8.3 can be turned into a **recursive tree algorithm** for generating the formula $F_{n,m}$ for any $[nx+m]^1_0$.

**Step 1:** Start at the root of a tree with a node labelled:

| Function | Formula |
|----------|---------|
| $nx + m$ | (to be filled) |

**Step 2:** From each node, if $n \neq 0$ create **two child nodes** as follows:

**If $n > 0$:**

| Left child | Right child |
|------------|-------------|
| Function: $(n-1)x + m$ | Function: $(n-1)x + (m+1)$ |
| Formula: $F_\text{left}$ | Formula: $F_\text{right}$ |

And the parent formula is: $(F_\text{left} \vee p) \wedge F_\text{right}$

**If $n < 0$:**

| Left child | Right child |
|------------|-------------|
| Function: $(n+1)x + (m-1)$ | Function: $(n+1)x + m$ |
| Formula: $F_\text{left}$ | Formula: $F_\text{right}$ |

And the parent formula is: $(F_\text{left} \vee \neg p) \wedge F_\text{right}$

**Step 3:** Once we reach a node with $n = 0$, fill in the formula cell:

| Case | Formula |
|------|---------|
| $m \leq 0$ | $\bot$ (falsum / contradiction) |
| $m \geq 1$ | $\top$ (verum / tautology) |

**Step 4:** Work **up** the tree to the root, using child formulas to construct the parent formula via the rule from Step 2.

**Simplification rules** (to reduce formula complexity):
$$(F \wedge \bot) \equiv (\bot \wedge F) \equiv \bot$$
$$(F \wedge \top) \equiv (\top \wedge F) \equiv F$$
$$(F \vee \bot) \equiv (\bot \vee F) \equiv F$$
$$(F \vee \top) \equiv (\top \vee F) \equiv \top$$

### 4.4 Example: $f(x) = [3x-2]^1_0$

**Root:** Function $3x - 2$, Formula $(F_L \vee p) \wedge F_R$

**Level 1 (left child):** Function $2x - 2$, Formula $(F_{LL} \vee p) \wedge F_{LR}$
**Level 1 (right child):** Function $2x - 1$, Formula $(F_{RL} \vee p) \wedge F_{RR}$

**Level 2 (left-left):** Function $x - 2$, Formula $(F \vee p) \wedge 0 = 0$
**Level 2 (left-right):** Function $x - 1$, Formula $(F \vee p) \wedge 0 = 0$
**Level 2 (right-left):** Function $x - 1$, Formula $(F \vee p) \wedge 0 = 0$
**Level 2 (right-right):** Function $x + 0$, Formula $(0 \vee p) \wedge 1 = p$

**Level 3 leaves ($n=0$, all constants):** $-2 \to 0$, $-1 \to 0$, $-1 \to 0$, $0 \to 0$, $-1 \to 0$, $0 \to 0$, $0 \to 0$, $1 \to 1$

**Working up:**
- Right-right parent: $(0 \vee p) \wedge 1 = p$
- Right-left parent: $(0 \vee p) \wedge 0 = 0$
- Right parent (function $2x-1$): $(0 \vee p) \wedge p = (p \wedge p)$
- Left parents: all reduce to $0$
- **Root formula:** $(0 \vee p) \wedge (p \wedge p) = (p \wedge p \wedge p)$

**Result:** $[3x-2]^1_0$ corresponds to the formula $F = (p \wedge p \wedge p)$.

**Verification:** Under $\mathbb{F}^3$, $p \wedge p = \max(0, x + x - 1) = \max(0, 2x-1) = [2x-1]^1_0$. Then $(p \wedge p) \wedge p = \max(0, [2x-1]^1_0 + x - 1) = \max(0, 3x - 2) = [3x-2]^1_0$. ✓

### 4.5 Example: $f(x) = [2-3x]^1_0 = [-3x+2]^1_0$

Since $n = -3 < 0$, we use $\neg p$ branches.

**Root:** Function $-3x + 2$, Formula $((F_L \vee \neg p) \wedge F_R)$

Working through the tree (with $n$ decreasing by 1 in absolute value at each level):

**Left child (level 1):** Function $-2x + 1$, Formula $(0 \vee \neg p) \wedge \neg p = (\neg p \wedge \neg p)$
**Right child (level 1):** Function $-2x + 2$, Formula $(\neg p \vee \neg p) \wedge 1 = (\neg p \vee \neg p)$

**Leaves (selected key values):**
- $-x + 0 \to (0 \vee \neg p) \wedge 0 = 0$
- $-x + 1 \to (0 \vee \neg p) \wedge 1 = \neg p$
- $-x + 2 \to (1 \vee \neg p) \wedge 1 = 1$

**Root formula:** $(((\neg p \wedge \neg p) \vee \neg p) \wedge (\neg p \vee \neg p))$

**Result:** $[-3x+2]^1_0$ corresponds to $F = ((\neg p \wedge \neg p) \vee \neg p) \wedge (\neg p \vee \neg p)$.

**Verification:** Under $\mathbb{F}^3$, $\neg p$ has truth function $1-x$. $\neg p \wedge \neg p = \max(0, (1-x)+(1-x)-1) = \max(0, 1-2x) = [1-2x]^1_0 = [-2x+1]^1_0$. Then $((\neg p \wedge \neg p) \vee \neg p) = \min(1, [1-2x]^1_0 + (1-x)) = [-3x+2]^1_0$. ✓

---

## 5. Combining Sub-Formulas — The Converse Direction

### 5.1 Simple Functions: Min of Two Linear Pieces

Given a piecewise-linear function $f$ with integer coefficients, Lemma 8.3 gives us formulas $F_i$ for each linear piece $f_i(x) = [a_i x + b_i]^1_0$. We combine them using $\underline{\wedge}$ (which realises $\min$) and $\underline{\vee}$ (which realises $\max$).

**Example:** Consider:
$$f(x) = \begin{cases} 0 & \text{if } x \leq \frac{1}{3} \\ 3x - 1 & \text{if } \frac{1}{2} < x \leq \frac{5}{8} \\ 4 - 5x & \text{if } \frac{5}{8} < x \leq \frac{4}{5} \\ 0 & \text{if } x > \frac{4}{5} \end{cases}$$

This is the minimum of:
- $f_{3,-1}(x) = [3x - 1]^1_0$ (blue)
- $f_{-5,4}(x) = [-5x + 4]^1_0$ (green/dashed)

Hence we can take:
$$F = F_{3,-1} \underline{\wedge} F_{-5,4}$$

where $F_{3,-1}$ and $F_{-5,4}$ are constructed by the tree algorithm.

### 5.2 Complex Functions: Wedge-Shaped Decomposition

For more complex piecewise-linear functions that cannot be expressed as a simple min or max of $f_{n,m}$ functions, decompose $f$ into **wedge-shaped** pieces. Each wedge is a function that:
- Rises linearly to a peak
- Descends linearly back to zero

Each wedge can be expressed as a $\min$ (i.e., $\underline{\wedge}$) of two clamped linear functions. Then $f$ is the maximum (i.e., $\underline{\vee}$) of all the wedges:

$$F = F_0 \underline{\vee} F_1 \underline{\vee} F_2 \underline{\vee} \cdots \underline{\vee} F_n$$

### 5.3 Lemma 8.4 — Sharp Approximation (Optional/Non-Examinable)

**[This section (8.4) is stated as non-examinable in the lecture notes]**

**Lemma 8.4:** Let $r, s, t \in \mathbb{N}$ with $\gcd(r,s) = 1$. Then for all $\varepsilon > 0$, there exist formulas $G_1$ and $G_2$ with truth-functions $g_1$ and $g_2$ such that:
1. $g_i\!\left(\frac{r}{s}\right) = \frac{t}{s}$ for $i = 1, 2$
2. $g_1(x) = 0$ and $g_2(x) = 1$ for all $x < \frac{r}{s} - \varepsilon$
3. $g_1(x) = 1$ and $g_2(x) = 0$ for all $x > \frac{r}{s} + \varepsilon$

**Proof sketch:** By **Bézout's Identity**, since $\gcd(r,s) = 1$, there are $a, b \in \mathbb{Z}$ with $ar + bs = 1$. Hence $art + bst = t$, so $art + bst + (krs - krs) = t$, giving $n = at + ks$ and $m = bt - kr$ for any $k \in \mathbb{Z}$. Then:
$$f_{n,m}\!\left(\frac{r}{s}\right) = n\cdot\frac{r}{s} + m = \frac{nr + ms}{s} = \frac{(at+ks)r + (bt-kr)s}{s} = \frac{t(ar+bs)}{s} = \frac{t}{s}$$

By choosing $k$ sufficiently large (positive or negative), we can make $f_{n,m}(x) = 0$ for all $x < \frac{r}{s} - \varepsilon$ (for $g_1$) or for all $x > \frac{r}{s} + \varepsilon$ (for $g_2$).

With Lemma 8.4, any wedge-shaped function (with rational breakpoints) can be represented by a formula, completing the proof of McNaughton's Theorem.

---

## 6. Łukasiewicz Semantics — Identities and Verification

### 6.1 Łukasiewicz Semantics $\mathbb{F}^3$ — Summary Table

Under $\mathbb{F}^3$, the connectives have the following truth functions for $x, y \in [0,1]$:

| Connective | Truth function |
|-----------|---------------|
| $\neg x$ | $1 - x$ |
| $x \wedge y$ | $\max(0, x+y-1)$ |
| $x \vee y$ | $\min(1, x+y)$ |
| $x \to y$ | $\min(1, 1-x+y)$ |

### 6.2 De Morgan's Laws in $\mathbb{F}^3$

**(i) $\neg(x \vee y) = (\neg x \wedge \neg y)$:**

$$\neg(x \vee y) = 1 - \min(1, x+y) = 1 + \max(-1, -(x+y)) = \max(0, 1-(x+y))$$
$$= \max(0, (1-x)+(1-y)-1) = \max(0, \neg x + \neg y - 1) = (\neg x \wedge \neg y) \quad \checkmark$$

**(ii) $\neg(x \wedge y) = (\neg x \vee \neg y)$:**

$$\neg(x \wedge y) = 1 - \max(0, x+y-1) = 1 + \min(0, -(x+y-1)) = \min(1, 1-(x+y-1))$$
$$= \min(1, (1-x)+(1-y)) = \min(1, \neg x + \neg y) = (\neg x \vee \neg y) \quad \checkmark$$

### 6.3 Implication as Disjunction in $\mathbb{F}^3$

**(iii) $(x \vee y) = (\neg x \vee y)$ [i.e., $(x \to y) = (\neg x \vee y)$]:**

$$(x \to y) = \min(1, 1-x+y) = \min(1, (1-x)+y) = \min(1, \neg x + y) = (\neg x \vee y) \quad \checkmark$$

This confirms that implication $x \to y$ in Łukasiewicz logic equals $\neg x \vee y$.

---

## 7. Partitions — The Intersection Lemma

### 7.1 Definition

A collection of intervals $P = \{I_1, \ldots, I_n\}$ is a **partition of $[0,1]$** if:
1. $I_i \cap I_j = \varnothing$ for all $i \neq j$ (pairwise disjoint)
2. $\bigcup_{i=1}^n I_i = [0,1]$ (covers the whole interval)

### 7.2 Lemma — Product of Two Partitions

**Lemma:** Let $P_1$ and $P_2$ be two partitions of $[0,1]$. Then:

$$P = \{A \cap B : A \in P_1 \text{ and } B \in P_2\}$$

is also a partition of $[0,1]$.

**Proof:**

**Disjointness:** Suppose $I \cap I' \neq \varnothing$ for some $I, I' \in P$. By definition, there are $A, A' \in P_1$ and $B, B' \in P_2$ such that $I = A \cap B$ and $I' = A' \cap B'$. Then:
$$I \cap I' = (A \cap B) \cap (A' \cap B') = (A \cap A') \cap (B \cap B') \neq \varnothing$$

So $A \cap A' \neq \varnothing$ and $B \cap B' \neq \varnothing$. Since $P_1$ is a partition, $A \cap A' \neq \varnothing$ forces $A = A'$; since $P_2$ is a partition, $B = B'$. Hence $I = A \cap B = A' \cap B' = I'$. So all distinct elements of $P$ are disjoint. ✓

**Coverage:** Let $x \in [0,1]$. Since $P_1$ covers $[0,1]$, there is some $A \in P_1$ with $x \in A$. Since $P_2$ covers $[0,1]$, there is some $B \in P_2$ with $x \in B$. Hence $x \in A \cap B \in P$. ✓

Therefore $P$ is a partition. $\square$

**Significance:** This lemma is used in the proof of McNaughton's Theorem — when combining the partition of $[0,1]$ from $\overline{G}$ and the partition from $\overline{H}$, we refine them to a common partition by taking all pairwise intersections.

---

## 8. Truth Functions of Łukasiewicz Formulas — Plotting and Inflexion Points

### 8.1 Key Formulas and Their Truth Functions

Under $\mathbb{F}^3$ with $x = $ truth value of $p$:

| Formula expression | Truth function |
|-------------------|----------------|
| $p$ | $x$ |
| $\neg p$ | $1 - x$ |
| $p \wedge p$ | $\max(0, 2x-1) = [2x-1]^1_0$ |
| $p \vee p$ | $\min(1, 2x) = [2x]^1_0$ |
| $p \wedge p \wedge p$ | $[3x-2]^1_0$ |
| $p \vee p \vee p$ | $[3x]^1_0$ |
| $p \wedge p \wedge p \wedge p$ | $[4x-3]^1_0$ |
| $p \vee p \vee p \vee p$ | $[4x]^1_0$ |

**General pattern:**
- $\underbrace{p \wedge \cdots \wedge p}_{n}$ has truth function $[nx - (n-1)]^1_0$: zero until $x = \frac{n-1}{n}$, then rises linearly to $1$ at $x = 1$.
- $\underbrace{p \vee \cdots \vee p}_{n}$ has truth function $[nx]^1_0$: rises linearly from $0$ to $1$ at $x = \frac{1}{n}$, then stays at $1$.

### 8.2 SGT Worked Examples — Plotting Truth Functions

#### (i) $F = (p \wedge p) \vee \neg p$

- $p \wedge p = [2x-1]^1_0$: is $0$ for $x \leq \frac{1}{2}$, then rises with slope 2.
- $\neg p = 1 - x$: decreases with slope $-1$.
- $F = \min(1, [2x-1]^1_0 + (1-x)) = \min(1, x) = x$... Wait — let us recompute.

Actually: $(p \wedge p) \vee \neg p = \min(1, \max(0, 2x-1) + (1-x))$.

For $x \leq \frac{1}{2}$: $\max(0,2x-1) = 0$, so $F = \min(1, 1-x) = 1 - x$ (since $1-x \leq 1$). Descends from 1 to $\frac{1}{2}$.

For $x > \frac{1}{2}$: $\max(0,2x-1) = 2x-1$, so $F = \min(1, (2x-1)+(1-x)) = \min(1, x) = x$ (since $x \leq 1$). Ascends from $\frac{1}{2}$ to 1.

**Shape:** V-shaped with minimum $\frac{1}{2}$ at $x = \frac{1}{2}$, values 1 at $x=0$ and $x=1$.
**Inflexion point:** $x = \frac{1}{2}$ (gradient changes from $-1$ to $+1$).

#### (ii) $F = (p \vee p) \wedge p$

- $p \vee p = \min(1, 2x) = [2x]^1_0$: rises with slope 2 until $x = \frac{1}{2}$, then constant 1.
- $F = \max(0, [2x]^1_0 + x - 1)$.

For $x \leq \frac{1}{2}$: $[2x]^1_0 = 2x$, so $F = \max(0, 2x + x - 1) = \max(0, 3x-1)$.
  - For $x \leq \frac{1}{3}$: $F = 0$.
  - For $\frac{1}{3} < x \leq \frac{1}{2}$: $F = 3x - 1$.

For $x > \frac{1}{2}$: $[2x]^1_0 = 1$, so $F = \max(0, 1 + x - 1) = x$.

**Shape:** Zero for $x \leq \frac{1}{3}$, rises with slope 3 from $\frac{1}{3}$ to $\frac{1}{2}$ (reaching $\frac{1}{2}$), then rises with slope 1 from $\frac{1}{2}$ to 1.
**Inflexion points:** $x = \frac{1}{3}$ (slope 0 to 3) and $x = \frac{1}{2}$ (slope 3 to 1).

#### (iii) $F = (p \vee p \vee p \vee p)$

Truth function: $[4x]^1_0$.

**Shape:** Rises linearly with slope 4 from $x=0$ to $x = \frac{1}{4}$, then constant 1.
**Inflexion point:** $x = \frac{1}{4}$ (slope 4 to 0).

#### (iv) $F = (p \wedge p \wedge p \wedge p)$

Truth function: $[4x-3]^1_0$.

**Shape:** Zero from $x=0$ to $x = \frac{3}{4}$, then rises linearly with slope 4 to reach 1 at $x = 1$.
**Inflexion point:** $x = \frac{3}{4}$ (slope 0 to 4).

---

## 9. SGT Questions and Full Solutions

### Question 8.1: Verify Łukasiewicz Identities

**(i) $\neg(x \wedge y) = (\neg x \vee \neg y)$**

$$\neg(x \wedge y) = 1 - \max(0, x+y-1) = 1 + \min(0, -(x+y-1))$$
$$= \min(1, 1-(x+y-1)) = \min(1, (1-x)+(1-y)) = \min(1, \neg x + \neg y) = (\neg x \vee \neg y)$$

**(ii) $\neg(x \vee y) = (\neg x \wedge \neg y)$**

$$\neg(x \vee y) = 1 - \min(1, x+y) = 1 + \max(-1, -(x+y)) = \max(0, 1-(x+y))$$
$$= \max(0, (1-x)+(1-y)-1) = \max(0, \neg x + \neg y - 1) = (\neg x \wedge \neg y)$$

**(iii) $(x \vee y) = (\neg x \vee y)$ [i.e., $(x \to y) = (\neg x \vee y)$]**

$$(x \to y) = \min(1, 1-x+y) = \min(1, (1-x)+y) = \min(1, \neg x + y) = (\neg x \vee y)$$

### Question 8.2: Product of Partitions

*(Full proof given in Section 7.2 above.)*

**Key steps:**
1. If $I \cap I' \neq \varnothing$ for $I = A \cap B$ and $I' = A' \cap B'$, then $(A \cap A') \neq \varnothing$ forces $A = A'$, and $(B \cap B') \neq \varnothing$ forces $B = B'$, so $I = I'$.
2. For any $x \in [0,1]$, pick $A \in P_1$ containing $x$ and $B \in P_2$ containing $x$; then $x \in A \cap B \in P$.

### Question 8.3: Plot Truth Functions

*(Full solutions given in Section 8.2 above.)*

Summary table of inflexion points:

| Formula | Inflexion points | Shape description |
|---------|-----------------|-------------------|
| $(p \wedge p) \vee \neg p$ | $x = \frac{1}{2}$ | V-shape: 1 down to $\frac{1}{2}$, back to 1 |
| $(p \vee p) \wedge p$ | $x = \frac{1}{3}$, $x = \frac{1}{2}$ | 0 to $x=\frac{1}{3}$, slope 3, then slope 1 |
| $p \vee p \vee p \vee p$ | $x = \frac{1}{4}$ | slope 4 up to $\frac{1}{4}$, flat at 1 |
| $p \wedge p \wedge p \wedge p$ | $x = \frac{3}{4}$ | flat 0 to $\frac{3}{4}$, slope 4 up to 1 |

### Question 8.4: Tree Algorithm for $f(x) = [3x-2]^1_0$

*(Full worked example given in Section 4.4 above.)*

**Answer:** The formula is $F = (p \wedge p \wedge p)$.

**Tree (top-down):**

```
Function: 3x - 2
Formula: (0 ∨ p) ∧ (p ∧ p) = (p ∧ p ∧ p)
        /                          \
  Fn: 2x-2                    Fn: 2x-1
  Fml: (0∨p)∧0 = 0           Fml: (0∨p)∧p = (p∧p)
   /       \                    /            \
Fn: x-2  Fn: x-1          Fn: x-1       Fn: x+0
Fml: 0   Fml: 0           Fml: 0        Fml: p
 / \      / \              / \           / \
-2  -1  -1   0           -1   0        0    1
 0   0   0   0            0   0        0    1
```

### Question 8.5: Tree Algorithm for $f(x) = [2-3x]^1_0 = [-3x+2]^1_0$

*(Full worked example given in Section 4.5 above.)*

**Answer:** The formula is $F = ((\neg p \wedge \neg p) \vee \neg p) \wedge (\neg p \vee \neg p)$.

**Tree (top-down, key nodes):**

```
Function: -3x + 2
Formula: ((¬p ∧ ¬p) ∨ ¬p) ∧ (¬p ∨ ¬p)
          /                         \
   Fn: -2x+1                    Fn: -2x+2
   Fml: (¬p ∧ ¬p)             Fml: (¬p ∨ ¬p)
    /         \                   /           \
Fn: -x+0  Fn: -x+1         Fn: -x+1      Fn: -x+2
Fml: 0    Fml: ¬p           Fml: ¬p       Fml: 1
  / \        / \               / \           / \
 -1   0     0   1             0   1         1   2
  0   0     0   1             0   1         1   1
```

### Question 8.6: Construct a Formula for the Piecewise Function

**Given:**
$$f(x) = \begin{cases} 1 & \text{if } x \leq \frac{1}{3} \\ 2 - 3x & \text{if } \frac{1}{3} < x \leq \frac{1}{2} \\ 1 - x & \text{if } \frac{1}{2} < x \leq \frac{3}{4} \\ 3x - 2 & \text{if } x > \frac{3}{4} \end{cases}$$

**Strategy:** Observe that $f(x)$ is the **maximum** of three sub-functions across $[0,1]$:

- $G_1 = [2-3x]^1_0$: decreasing from 1 (at $x = 0$) down to 0 (at $x = \frac{2}{3}$), then 0.
- $G_2 = [1-x]^1_0 = \neg p$: decreasing from 1 to 0, with slope $-1$.
- $G_3 = [3x-2]^1_0$: 0 until $x = \frac{2}{3}$, then rising to 1.

From questions 8.4 and 8.5:
- $[2-3x]^1_0 \mapsto G_1 = ((\neg p \wedge \neg p) \vee \neg p) \wedge (\neg p \vee \neg p)$
- $[1-x]^1_0 \mapsto G_2 = \neg p$
- $[3x-2]^1_0 \mapsto G_3 = (p \wedge p \wedge p)$

Since $f(x) = \max(G_1, G_2, G_3)$ over the domain, and $\vee$ in $\mathbb{F}^3$ realises $\min(1, \cdot + \cdot)$ which equals $\max$ when applied via $\underline{\vee}$ (the min-max operator):

$$F = G_1 \underline{\vee} G_2 \underline{\vee} G_3$$

$$= \left[((\neg p \wedge \neg p) \vee \neg p) \wedge (\neg p \vee \neg p)\right] \underline{\vee} \left[\neg p\right] \underline{\vee} \left[p \wedge p \wedge p\right]$$

where $\underline{\vee}$ is the **defined operator** realising $\max(x,y) = (x \to y) \to y$.

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Łukasiewicz logic $\mathbb{F}^3$** | The fuzzy logic with $F_\neg(x) = 1-x$, $F_\wedge = \max(0, x+y-1)$, $F_\vee = \min(1, x+y)$, $F_\to = \min(1, 1-x+y)$ |
| **Law of Excluded Middle (LEM)** | $x \vee \neg x = 1$ for all $x \in [0,1]$; satisfied by $\mathbb{F}^3$ but not $\mathbb{F}^1$ or $\mathbb{F}^2$ |
| **Defined operators $\underline{\wedge}$, $\underline{\vee}$** | $x \underline{\wedge} y = (x \to y) \wedge x = \min(x,y)$; $x \underline{\vee} y = (x \to y) \to y = \max(x,y)$ — abbreviations in $\mathbb{F}^3$ |
| **$[z]^1_0$** | Clamping function: $\min(1, \max(0, z))$ — restricts $z$ to $[0,1]$ |
| **Continuous piecewise-linear function with integer coefficients (CPLIC)** | A continuous function $f : [0,1] \to [0,1]$ that is linear with integer slope and intercept on each sub-interval of a finite partition of $[0,1]$ |
| **McNaughton's Theorem (1 variable)** | A function $f : [0,1] \to [0,1]$ is the truth function of a propositional formula in $\mathbb{F}^3$ iff $f$ is a CPLIC function |
| **Lemma 8.2** | Every truth function of a one-variable $\mathbb{F}^3$ formula is a CPLIC function (proved by induction) |
| **Lemma 8.3** | For any $n, m \in \mathbb{Z}$, $f_{n,m}(x) = [nx+m]^1_0$ is the truth function of some formula $F_{n,m}$ |
| **Tree algorithm** | Recursive procedure for constructing $F_{n,m}$: at each node split $(n,m)$ into two children by reducing $|n|$ by 1, with different $m$ values; base case $n=0$ gives $\bot$ or $\top$ |
| **$\bot$** | Contradiction/falsum: $p \wedge \neg p$, truth function constantly 0 |
| **$\top$** | Tautology/verum: $p \vee \neg p$, truth function constantly 1 |
| **Wedge-shaped function** | A piecewise-linear function that rises then falls (or vice versa), used as a building block in the converse proof of McNaughton's Theorem |
| **Partition of $[0,1]$** | A collection $\{I_1, \ldots, I_n\}$ of intervals that are pairwise disjoint and together cover $[0,1]$ |
| **Bézout's Identity** | For integers $r, s$ with $\gcd(r,s)=1$, there exist $a, b \in \mathbb{Z}$ with $ar + bs = 1$ |
| **Inflexion point** | A point $x$ in the domain of a truth function where the gradient (slope) changes |

---

## Summary

- **LO1 (Why $\mathbb{F}^3$):** Łukasiewicz logic $\mathbb{F}^3$ is preferred because (a) it satisfies the Law of Excluded Middle ($x \vee \neg x = 1$), which $\mathbb{F}^1$ and $\mathbb{F}^2$ do not, and (b) it can express $\min$ and $\max$ as defined operators $\underline{\wedge}$ and $\underline{\vee}$, making it an extension of Godel logic $\mathbb{F}^1$.

- **LO2 (McNaughton's Theorem statement):** A function $f : [0,1] \to [0,1]$ is representable in $\mathbb{F}^3$ iff it is a continuous piecewise-linear function with integer coefficients. This is the complete characterisation of the expressive power of $\mathbb{F}^3$.

- **LO3 (Lemma 8.2 — "only if"):** Proved by structural induction. For each connective ($\neg$, $\wedge$, $\vee$, $\to$), if the sub-formulas have CPLIC truth functions, so does the compound formula, since negation reverses signs (still integers), $\wedge$ adds coefficients (still integers), and $\vee$ and $\to$ reduce to previous cases.

- **LO4 (Lemma 8.3 and tree algorithm — "if" direction for building blocks):** Any $[nx+m]^1_0$ is representable. The tree algorithm systematically builds the formula by reducing $|n|$ by 1 at each level, branching on whether $n > 0$ (use $p$) or $n < 0$ (use $\neg p$), until $n = 0$ (use $\bot$ or $\top$).

- **LO5 (Combining sub-formulas):** For simple piecewise-linear functions, take the min (using $\underline{\wedge}$) of two linear pieces. For complex functions, decompose into wedge-shaped pieces and take the max (using $\underline{\vee}$) of the wedges. This is how the converse of McNaughton's Theorem is achieved in practice.

- **LO6 (Łukasiewicz identities):** Key identities verified algebraically: $\neg(x \wedge y) = \neg x \vee \neg y$, $\neg(x \vee y) = \neg x \wedge \neg y$, and $(x \to y) = \neg x \vee y$. All follow from direct computation using the $\mathbb{F}^3$ truth functions.

- **LO7 (Partitions):** The intersection $P = \{A \cap B : A \in P_1, B \in P_2\}$ of two partitions is a partition. This is the technical tool used in the induction proof (Lemma 8.2, $F = G \wedge H$ case) to unify the two partition sequences into one common refinement.

- **LO8 (Truth function plotting):** Repeated $\wedge$ contracts the rising part to the right (inflexion at $\frac{n-1}{n}$); repeated $\vee$ contracts the rising part to the left (inflexion at $\frac{1}{n}$). Mixed formulas like $(p \wedge p) \vee \neg p$ produce V-shapes with inflexion at $x = \frac{1}{2}$.
