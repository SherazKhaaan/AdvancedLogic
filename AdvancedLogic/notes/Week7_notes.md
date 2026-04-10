# Week 7: The Mostert–Shields Theorems — Classification of Continuous T-Norms and T-Conorms

## Learning Objectives

By the end of this week, you should be able to:

- [ ] State the four defining properties **(C1)–(C4)** of a continuous t-norm and verify them for given functions
- [ ] State the Mostert–Shields Theorem for conjunction and explain what it means for a t-norm to be a "chimera"
- [ ] Identify the set $A = \{z \in [0,1] : z \wedge z = z\}$ for a given t-norm and use it to analyse its structure
- [ ] Prove that $F^1_\wedge = \min(x,y)$, $F^2_\wedge = xy$, and $F^3_\wedge = \max(0, x+y-1)$ are continuous t-norms by verifying (C1)–(C4)
- [ ] Apply Lemma 6.2 (the "min-behaviour" lemma) and Lemmas 6.3–6.4 to characterise a t-norm over sub-intervals
- [ ] Construct the isomorphism $\rho$ that witnesses a sub-interval being isomorphic to $F^2_\wedge$ (product logic) or $F^3_\wedge$ (Łukasiewicz logic)
- [ ] State the four defining properties **(D1)–(D4)** of a continuous t-conorm and state the Mostert–Shields Theorem for disjunction
- [ ] Express a given chimera t-norm in terms of $F^1_\wedge$, $F^2_\wedge$, $F^3_\wedge$ on identified sub-intervals
- [ ] Prove that continuity of $\wedge$ implies commutativity with limits of convergent sequences

---

## 1. Background and Motivation

### 1.1 The Three Canonical T-Norms

From prior weeks, we have three functions $F^1_\wedge, F^2_\wedge, F^3_\wedge : [0,1]^2 \to [0,1]$ that serve as "reasonable" interpretations of conjunction in a fuzzy/many-valued logic:

| Name | Formula | Logic |
|------|---------|-------|
| $F^1_\wedge(x,y)$ | $\min(x,y)$ | Godel / $\mathbb{F}^1$ |
| $F^2_\wedge(x,y)$ | $xy$ | Product / $\mathbb{F}^2$ |
| $F^3_\wedge(x,y)$ | $\max(0, x+y-1)$ | Łukasiewicz / $\mathbb{F}^3$ |

These are **not** mutually isomorphic: $F^1_\wedge(x,x) = x$ for all $x$, whereas $F^2_\wedge(x,x) = x^2 < x$ for $0 < x < 1$. No order-preserving isomorphism can map one to the other.

### 1.2 The Disc Notation

It is convenient to define the **open disc** of radius $\delta$ centred at $(a,b) \in [0,1]^2$:

$$B_\delta(a,b) = \{(x,y) \in [0,1]^2 : (x-a)^2 + (y-b)^2 < \delta^2\}$$

This is used in the $\varepsilon$-$\delta$ formulation of continuity for functions of two variables. (Topologists call this a "ball" of dimension 2, hence the notation $B$.)

---

## 2. Continuous T-Norms: The Four Axioms

### 2.1 Definitions — Properties (C1)–(C4)

A binary function $\wedge : [0,1]^2 \to [0,1]$ is a **(continuous) triangular norm** (or **t-norm**) if it satisfies all four of the following properties:

**(C1) Boolean:** The function behaves correctly on Boolean values:
$$0 \wedge 0 = 1 \wedge 0 = 0 \wedge 1 = 0 \qquad \text{and} \qquad 1 \wedge 1 = 1$$

**(C2) Increasing (monotone):** $\wedge$ is **increasing** in each component. That is, if $z < z'$ then:
$$(x \wedge z) \leq (x \wedge z') \quad \text{and} \quad (z \wedge y) \leq (z' \wedge y) \quad \text{for all } x,y \in [0,1]$$

**(C3) Continuous:** $\wedge$ is **continuous** as a function $[0,1]^2 \to [0,1]$. That is, for all $(a,b) \in [0,1]^2$ and $\varepsilon > 0$ there is a $\delta > 0$ such that:
$$|(x \wedge y) - (x' \wedge y')| < \varepsilon \quad \text{whenever } (x,y) \in B_\delta(a,b)$$

**(C4) Associative:** $\wedge$ is **associative**:
$$((x \wedge y) \wedge z) = (x \wedge (y \wedge z))$$

### 2.2 Derived Properties

Any t-norm automatically satisfies the following (provable from (C1)–(C4)):

- **(i)** $x \wedge 0 = 0 \wedge x = 0$
- **(ii)** $x \wedge 1 = 1 \wedge x = x$
- **(iii)** $x \wedge x \leq x$

These are derived consequences that reinforce the "reasonableness" of the axioms.

### 2.3 Continuity Implies Limit-Commutativity

**Result:** If $\wedge$ satisfies **(C3)**, then for any convergent sequence $x_n \in [0,1]$ and any $y \in [0,1]$:

$$\left(\lim_{n \to \infty} x_n\right) \wedge y = \lim_{n \to \infty}(x_n \wedge y)$$

**Proof sketch:** Let $L = \lim x_n$. Given $\varepsilon > 0$, by (C3) there is some $\delta > 0$ such that $|(x_n \wedge y) - (L \wedge y)| < \varepsilon$ whenever $(x_n, y) \in B_\delta(L, y)$. Since $x_n \to L$, there is $N \geq 0$ such that $|x_n - L| < \delta$ for all $n \geq N$, which places $(x_n, y)$ inside $B_\delta(L,y)$. Hence $\lim_{n \to \infty}(x_n \wedge y) = L \wedge y$.

---

## 3. Verifying the Three Standard T-Norms

### 3.1 $F^1_\wedge(x,y) = \min(x,y)$

**(C1):** $\min(0,0) = \min(1,0) = \min(0,1) = 0$ and $\min(1,1) = 1$. ✓

**(C2):** Let $z < z'$. Three cases:
- If $x \leq z$: $\min(x,z) = x \leq x = \min(x,z')$.
- If $z < x \leq z'$: $\min(x,z) = z < x \leq \min(x,z')$.
- If $x > z'$: $\min(x,z) = z < z' = \min(x,z')$.
Similarly for the second component by commutativity of $\min$. ✓

**(C3):** Fix $(a,b)$ and let $\varepsilon > 0$.
- If $a = b$: take $\delta = \varepsilon$. Then $|\min(x,y) - a| = |\min(x-a, y-a)| < \delta$ since both differences are bounded by $\delta$ in absolute value.
- If $a \neq b$: choose $\delta = \min\!\left(\varepsilon, \frac{|a-b|}{2\sqrt{2}}\right)$ so the disc $B_\delta(a,b)$ lies entirely in either the half-plane $x < y$ or $x > y$.
  - If $a < b$: $\min(a,b) = a$, and for $(x,y) \in B_\delta(a,b)$ we have $x < y$, so $|\min(x,y) - \min(a,b)| = |x - a| < \delta \leq \varepsilon$.
  - If $a > b$: analogously $|\min(x,y) - \min(a,b)| = |y - b| < \delta \leq \varepsilon$. ✓

**(C4):** $t \leq \min(a,b)$ iff $t \leq a$ and $t \leq b$. Therefore:
$$t \leq \min(\min(x,y),z) \iff t \leq x \text{ and } t \leq y \text{ and } t \leq z \iff t \leq \min(x,\min(y,z))$$
So $\min(\min(x,y),z) = \min(x,\min(y,z))$. ✓

### 3.2 $F^2_\wedge(x,y) = xy$

**(C1):** $0 \times 0 = 1 \times 0 = 0 \times 1 = 0$ and $1 \times 1 = 1$. ✓

**(C2):** Let $z < z'$, so $z' = z + \delta$ for some $\delta > 0$. Then $xz \leq xz + x\delta = x(z+\delta) = xz'$ since $x\delta \geq 0$. Similarly $zy \leq z'y$. ✓

**(C3):** Fix $(a,b) \in [0,1]^2$ and let $\varepsilon > 0$. Write $x = a+u$, $y = b+v$ with $|u|,|v| < \delta$. Then:
$$|xy - ab| = |(a+u)(b+v) - ab| = |av + bu + uv| \leq a|v| + b|u| + |uv| < \delta + \delta + \delta^2$$
since $a, b \leq 1$. Choose $\delta = \varepsilon/3$; then $\delta + \delta + \delta^2 = \varepsilon/3 + \varepsilon/3 + (\varepsilon/3)^2 \leq \varepsilon$. ✓

**(C4):** $(xy)z = x(yz)$ by associativity of multiplication over $\mathbb{R}$. ✓

### 3.3 $F^3_\wedge(x,y) = \max(0, x+y-1)$

**(C1):**
$$\max(0, 0+0-1) = \max(0, 1+0-1) = \max(0, 0+1-1) = \max(0,0) = 0$$
$$\max(0, 1+1-1) = \max(0,1) = 1$$  ✓

**(C2):** Let $z < z'$. Then $\max(0, x+z-1) \leq \max(0, x+z'-1)$ since $x + z - 1 \leq x + z' - 1$ and both are clamped to be $\geq 0$. ✓

**(C3):** Choose $\delta$ so the disc $B_\delta(a,b)$ lies entirely in the half-plane $y < 1-x$ or $y > 1-x$.
- If $(a,b)$ has $a+b < 1$: then $a+b-1 < 0$ and $x+y-1 < 0$ for $(x,y) \in B_\delta(a,b)$, so both sides are 0, giving $|\max(0,x+y-1) - \max(0,a+b-1)| = 0 < \varepsilon$. ✓

**(C4):** Verified by direct computation (analogous to the associativity of bounded addition). ✓

---

## 4. The Mostert–Shields Theorem for Conjunction

### 4.1 The Chimera Idea

The key question is: are $F^1_\wedge, F^2_\wedge, F^3_\wedge$ the **only** continuous t-norms (up to isomorphism)? The answer is **no** — but any continuous t-norm must be a **chimera** of these three, meaning it behaves like one of the three t-norms on each sub-interval of $[0,1]$, and like $F^1_\wedge = \min$ outside those sub-intervals.

### 4.2 The Idempotent Set $A$

For any t-norm $\wedge$, define the set of **idempotent elements**:

$$A = \{z \in [0,1] : z \wedge z = z\}$$

Note that always $0, 1 \in A$ since $0 \wedge 0 = 0$ and $1 \wedge 1 = 1$ by (C1). For $F^1_\wedge$, every $z \in [0,1]$ is in $A$. For $F^2_\wedge$, only $z = 0$ and $z = 1$ are in $A$ (since $z^2 = z$ iff $z = 0$ or $z = 1$). For $F^3_\wedge$, only $\{0, 1\}$.

### 4.3 Lemma 6.2 — Min Behaviour Near Idempotents

**Lemma 6.2:** Let $z \in [0,1]$ be such that $z \wedge z = z$. Then:
- **(i)** For all $x \leq z \leq y$ we have $(x \wedge y) = x = \min(x,y)$
- **(ii)** For all $y \leq z \leq x$ we have $(x \wedge y) = y = \min(x,y)$

**Proof (case i):** Suppose $x \leq z \leq y$ and $z \wedge z = z$.
- By **(C1)** and the IVT, there is some $t \in [0,1]$ such that $x = z \wedge t$.
- Then:

$$x = z \wedge t = (z \wedge z) \wedge t \quad \text{[since } z \wedge z = z\text{]}$$
$$= z \wedge (z \wedge t) \quad \text{by (C4)}$$
$$\leq y \wedge (z \wedge t) \quad \text{by (C2), since } z \leq y$$
$$= y \wedge x \quad \text{by definition}$$

- Also $y \wedge x \leq 1 \wedge x = x$ by **(C2)** since $y \leq 1$.
- Therefore $x \wedge y = x = \min(x,y)$. $\square$

### 4.4 Interval Classification — Types 2 and 3

From $A$, we identify sub-intervals $[a_i, b_i] \subseteq [0,1]$ where $a_i, b_i \in A$ and $(a_i, b_i) \cap A = \varnothing$. These are the intervals where the behaviour of $\wedge$ is **not yet determined** to be $\min$. They split into two types:

- **Type 2:** $z \wedge z \neq a_i$ for all $z \in (a_i, b_i)$ — behaves like $F^2_\wedge = xy$
- **Type 3:** $z \wedge z = a_i$ for some $z \in (a_i, b_i)$ — behaves like $F^3_\wedge = \max(0, x+y-1)$

### 4.5 Lemma 6.3 — Type 2 Intervals are Isomorphic to $F^2_\wedge$

**Lemma 6.3:** Let $[a,b] \subseteq [0,1]$ be a type 2 interval ($a,b \in A$, $(a,b) \cap A = \varnothing$, and $z \wedge z \neq a$ for all $z \in (a,b)$). Then $\langle [a,b], <, \wedge \rangle \cong \langle [0,1], <, \times \rangle$.

**Proof sketch:** Fix $c \in [a,b]$ (e.g. $c = (a+b)/2$). Define **$\wedge$-exponentiation**:
- Integer powers: $x^{\wedge n} = \underbrace{x \wedge x \wedge \cdots \wedge x}_{n}$
- Fractional powers: $x^{\wedge \frac{1}{n}} = y$ where $y^{\wedge n} = x$ (unique by IVT + monotonicity)
- Rational powers: $x^{\wedge \frac{n}{m}} = (x^{\wedge n})^{\wedge \frac{1}{m}}$
- Real powers: $x^{\wedge r} = \lim_{n \to \infty} x^{\wedge r_n}$ for any sequence of rationals $r_n \to r$

The isomorphism $\rho : [0,1] \to [a,b]$ from $\langle [0,1], \times, < \rangle$ to $\langle [a,b], \wedge, < \rangle$ is:

$$\rho(x) = \begin{cases} a & \text{if } x = 0 \\ b & \text{if } x = 1 \\ c^{\wedge r} & \text{if } 0 < x < 1 \text{ where } r = -\log_2(x) \end{cases}$$

Key properties of $\wedge$-exponentiation used in this proof:
- $(x^{\wedge r} \wedge x^{\wedge s}) = x^{\wedge(r+s)}$ for real $r, s > 0$
- If $r < s$ then $x^{\wedge r} \geq x^{\wedge s}$ (exponentiation is order-reversing)

### 4.6 Lemma 6.4 — Type 3 Intervals are Isomorphic to $F^3_\wedge$

**Lemma 6.4:** Let $[a,b] \subseteq [0,1]$ be a type 3 interval ($a,b \in A$, $(a,b) \cap A = \varnothing$, and $\exists z \in (a,b)$ with $z \wedge z = a$). Then $\langle [a,b], <, \wedge \rangle \cong \langle [0,1], \max(0, x+y-1), < \rangle$.

**Proof sketch:** Let $c \in [a,b]$ be the **largest** value with $c \wedge c = a$. Define $x^{\wedge r}$ as before, but note $c^{\wedge r} = a$ for all $r \geq 2$ (since $c \wedge c = a$, which is an idempotent).

The isomorphism is constructed differently here: define $\rho : [0,2] \to [a,b]$ from $\langle [0,2], >, \min(2, x+y) \rangle$ to $\langle [a,b], <, \wedge \rangle$ by:

$$\rho(x) = \begin{cases} a & \text{if } x = 2 \\ b & \text{if } x = 0 \\ c^{\wedge x} & \text{if } 0 < x < 2 \end{cases}$$

This is an **order-reversing** isomorphism (since $\rho(x) < \rho(y)$ iff $x > y$). To get an order-preserving one, compose with the order-reversing isomorphism $\sigma : [0,1] \to [0,2]$ defined by $\sigma(x) = 2(1-x)$. Then $\rho \circ \sigma : [0,1] \to [a,b]$ is an order-preserving isomorphism from $\langle [0,1], <, \max(0, x+y-1) \rangle$ to $\langle [a,b], <, \wedge \rangle$.

### 4.7 Theorem 6.1 / 6.5 — Mostert–Shields Theorem for $\wedge$

**Theorem 6.1 (Mostert–Shields, 1957):** Suppose $\wedge$ satisfies **(C1)–(C4)** and let $A = \{z \in [0,1] : z \wedge z = z\}$. Then for all $[a,b] \subseteq [0,1]$ such that $a, b \in A$ and $(a,b) \cap A = \varnothing$:

- **If $z \wedge z \neq a$ for all $z \in (a,b)$** (Type 2) then:
$$\langle [a,b], <, \wedge \rangle \cong \langle [0,1], <, F^2_\wedge \rangle$$

- **If $z \wedge z = a$ for some $z \in (a,b)$** (Type 3) then:
$$\langle [a,b], <, \wedge \rangle \cong \langle [0,1], <, F^3_\wedge \rangle$$

- **Otherwise** (when $x, y$ do not belong to any common sub-interval $[a,b]$ with $a,b \in A$, $(a,b) \cap A = \varnothing$):
$$(x \wedge y) = F^1_\wedge(x,y) = \min(x,y)$$

**Proof summary:** Combine Lemma 6.2 (outside the sub-intervals, $\wedge$ equals $\min$) with Lemma 6.3 (type 2 sub-intervals are isomorphic to $F^2_\wedge$) and Lemma 6.4 (type 3 sub-intervals are isomorphic to $F^3_\wedge$).

The domain $[0,1]^2$ of $F_\wedge$ can be visualised as a unit square, where the main diagonal contains the sub-intervals $[a_i, b_i]^2$ arranged as squares; inside each square the t-norm behaves like $F^2_\wedge$ or $F^3_\wedge$, and everywhere else it behaves like $F^1_\wedge = \min$.

---

## 5. The Mostert–Shields Theorem for Disjunction

### 5.1 Axioms (D1)–(D4) for T-Conorms

The three standard t-conorms (disjunction operators) are:

| Name | Formula | Logic |
|------|---------|-------|
| $F^1_\vee(x,y)$ | $\max(x,y)$ | Godel / $\mathbb{F}^1$ |
| $F^2_\vee(x,y)$ | $x + y - xy$ | Product / $\mathbb{F}^2$ |
| $F^3_\vee(x,y)$ | $\min(1, x+y)$ | Łukasiewicz / $\mathbb{F}^3$ |

A binary function $\vee : [0,1]^2 \to [0,1]$ is a **(continuous) triangular conorm** (or **t-conorm**) if it satisfies:

**(D1) Boolean:** $0 \vee 0 = 0$ and $1 \vee 0 = 0 \vee 1 = 1 \vee 1 = 1$

**(D2) Increasing:** If $z < z'$ then $(x \vee z) \leq (x \vee z')$ and $(z \vee y) \leq (z' \vee y)$

**(D3) Continuous:** For all $(a,b) \in [0,1]^2$ and $\varepsilon > 0$, there is $\delta > 0$ such that $|(x \vee y) - (a \vee b)| < \varepsilon$ whenever $(x,y) \in B_\delta(a,b)$

**(D4) Associative:** $((x \vee y) \vee z) = (x \vee (y \vee z))$

### 5.2 Theorem 6.6 — Mostert–Shields Theorem for $\vee$

**Theorem 6.6:** Suppose $\vee$ satisfies **(D1)–(D4)** and let $A = \{z \in [0,1] : z \vee z = z\}$. Then for all $[a,b] \subseteq [0,1]$ with $a,b \in A$ and $(a,b) \cap A = \varnothing$:

- **If $z \vee z \neq b$ for all $z \in (a,b)$:** $\langle [a,b], <, \vee \rangle \cong \langle [0,1], <, F^2_\vee \rangle$
- **If $z \vee z = b$ for some $z \in (a,b)$:** $\langle [a,b], <, \vee \rangle \cong \langle [0,1], <, F^3_\vee \rangle$
- **Otherwise:** $(x \vee y) = F^1_\vee(x,y) = \max(x,y)$

### 5.3 Deriving the Disjunction Result from the Conjunction Result

For any t-conorm $\vee$ satisfying **(D1)–(D4)**, define:

$$(x \wedge y) = 1 - ((1-x) \vee (1-y))$$

This $\wedge$ satisfies **(C1)–(C4)**. Moreover:

$$F^1_\wedge(x,y) = 1 - F^1_\vee(1-x, 1-y)$$
$$F^2_\wedge(x,y) = 1 - F^2_\vee(1-x, 1-y)$$
$$F^3_\wedge(x,y) = 1 - F^3_\vee(1-x, 1-y)$$

So if $\langle [a,b], <, \wedge \rangle \cong \langle [0,1], <, F^k_\wedge \rangle$ for $k = 2, 3$, then $\langle [a,b], <, \vee \rangle \cong \langle [0,1], <, F^k_\vee \rangle$.

---

## 6. SGT Questions and Full Solutions

### Question 7.1: Verify $F^1_\wedge(x,y) = \min(x,y)$ is a continuous t-norm

*(Full solution given in Section 3.1 above.)*

The key insight for **(C1)** is noting this is actually asking about $\min$, **not** $xy$ (the question sheet has a typo — both 7.1 and 7.2 are labelled $xy$ but 7.1 is $\min(x,y)$). The solution in the solutions PDF addresses $\min(x,y)$ for 7.1.

### Question 7.2: Verify $F^2_\wedge(x,y) = xy$ is a continuous t-norm

*(Full solution given in Section 3.2 above.)*

Key calculation for **(C3)**: Write $x = a+u, y = b+v$, then:
$$|xy - ab| \leq a|v| + b|u| + |uv| < \delta + \delta + \delta^2$$
Choose $\delta = \varepsilon/3$.

### Question 7.3: Verify $F^3_\wedge(x,y) = \max(0, x+y-1)$ is a continuous t-norm

*(Full solution given in Section 3.3 above.)*

### Question 7.4: Continuity implies limit commutativity

**Statement:** If $\wedge$ satisfies **(C3)**, then for any convergent sequence $x_n \in [0,1]$ with $y \in [0,1]$:
$$\left(\lim_{n \to \infty} x_n\right) \wedge y = \lim_{n \to \infty}(x_n \wedge y)$$

**Solution:**
- Let $L = \lim x_n$: for all $\varepsilon > 0$, $\exists N$ such that $|x_n - L| < \varepsilon$ for $n \geq N$.
- Want to show: $\exists N$ such that $|(x_n \wedge y) - (L \wedge y)| < \varepsilon$ for $n \geq N$.
- Fix $\varepsilon > 0$. By **(C3)**, $\exists \delta > 0$ such that $|(x_n \wedge y) - (L \wedge y)| < \varepsilon$ whenever $(x_n, y) \in B_\delta(L, y)$.
- Since $x_n \to L$, $\exists N$ such that $|x_n - L| < \delta$ for $n \geq N$, which means $(x_n, y) \in B_\delta(L, y)$.
- Hence $|(x_n \wedge y) - (L \wedge y)| < \varepsilon$ for all $n \geq N$, giving $\lim_{n\to\infty}(x_n \wedge y) = L \wedge y$.  $\square$

### Question 7.5: The Chimera Function $x \wedge y = \dfrac{xy}{\max(x, y, \frac{1}{2})}$

Consider:
$$x \wedge y = \frac{xy}{\max\!\left(x, y, \tfrac{1}{2}\right)}, \quad x,y \in [0,1]$$

**Simplification by cases:**

- If $x, y \leq \frac{1}{2}$: $\max(x, y, \frac{1}{2}) = \frac{1}{2}$, so $x \wedge y = 2xy$.
- If $\frac{1}{2} < x \leq y$: $\max(x, y, \frac{1}{2}) = y$, so $x \wedge y = \frac{xy}{y} = x = \min(x,y)$.
- If $\frac{1}{2} < y \leq x$: $\max(x, y, \frac{1}{2}) = x$, so $x \wedge y = \frac{xy}{x} = y = \min(x,y)$.

#### Part (i): Verify (C1)–(C4)

**For (C1):** If $x = 0$ or $y = 0$, then $xy = 0$, so $x \wedge y = 0$. If $x = y = 1$: $1 \wedge 1 = 1/\max(1,1,\frac{1}{2}) = 1$. ✓

**For (C2):** Let $z < z'$. The goal is $(x \wedge z) \leq (x \wedge z')$, i.e.:
$$\frac{xz}{\max(x, z, \frac{1}{2})} \leq \frac{xz'}{\max(x, z', \frac{1}{2})}$$
which is equivalent to $xz \cdot \max(x, z', \frac{1}{2}) \leq xz' \cdot \max(x, z, \frac{1}{2})$, i.e. $\max(x^2 z, xzz', \frac{1}{2}xz) \leq \max(x^2z', xz'^2, \frac{1}{2}xz')$.

Since $z \leq z'$, any upper bound of the right side is also an upper bound of the left side (each term on the left is $\leq$ the corresponding term on the right). Formally: if $t \geq \max(x^2z', xz', \frac{1}{2}xz')$ then $t \geq x^2z, xz, \frac{1}{2}xz$, so $t \geq \max(x^2z, xz, \frac{1}{2}xz)$. ✓

**For (C3):** We know $\min$ is continuous on $[0,1]$ and $f(x,y) = 2xy$ is continuous on $[0,1]$ (as multiplication). The only concern is the boundary at $x = \frac{1}{2}$ or $y = \frac{1}{2}$. On this boundary:
- When $y \leq x = \frac{1}{2}$: $2xy = y = \min(x,y)$.
- When $x \leq y = \frac{1}{2}$: $2xy = x = \min(x,y)$.
So both pieces agree on the boundary — no discontinuity. ✓

**For (C4):** If $x, y, z \leq \frac{1}{2}$ then $2xy \leq \frac{1}{2}$, so:
$$(x \wedge y) \wedge z = (2xy) \wedge z = 2(2xy)z = 4xyz$$
$$x \wedge (y \wedge z) = x \wedge (2yz) = 2x(2yz) = 4xyz \quad ✓$$

If $w = \max(x,y,z) > \frac{1}{2}$ (say $w = x$, so $x \geq y, z$):
$$(x \wedge y) \wedge z = \min(x,y) \wedge z = y \wedge z \quad \text{[since } x \geq y,z \text{ is maximum]}$$
$$= \min(x, y \wedge z) \quad \text{[since } (y \wedge z) \leq y \leq x\text{]}$$
$$= x \wedge (y \wedge z) \quad ✓$$

#### Part (ii): The Set $A = \{z \in [0,1] : z \wedge z = z\}$

$$A = \{0\} \cup \left[\frac{1}{2}, 1\right]$$

**Proof:**
- For $z \geq \frac{1}{2}$: $\max(z, z, \frac{1}{2}) = z$, so $z \wedge z = z^2/z = z$. ✓
- For $0 < z < \frac{1}{2}$: $z \wedge z = 2z^2$. Setting $2z^2 = z$ gives $z = 1/2$ (excluded) or $z = 0$ (excluded). So no idempotents in $(0, \frac{1}{2})$.
- $z = 0$: $0 \wedge 0 = 0$. ✓

#### Part (iii): Chimera Characterisation

The function $\wedge$ is a chimera of $F^1_\wedge$ and $F^2_\wedge$:

- **On $[0, \frac{1}{2}]$:** $x \wedge y = 2xy$, which is isomorphic to the **product t-norm** $F^2_\wedge$ under the isomorphism $\rho(z) = 2z$, since:
$$\rho(x \wedge y) = 2(2xy) = 4xy = (2x)(2y) = \rho(x)\rho(y)$$

- **Outside this interval** (i.e. when $\max(x,y) > \frac{1}{2}$): $\wedge$ is exactly the **minimum t-norm** $F^1_\wedge = \min(x,y)$.

The Łukasiewicz t-norm $F^3_\wedge$ does **not** appear in this particular chimera.

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **T-norm** | A binary function $\wedge : [0,1]^2 \to [0,1]$ satisfying (C1) Boolean, (C2) Increasing, (C3) Continuous, (C4) Associative |
| **T-conorm** | A binary function $\vee : [0,1]^2 \to [0,1]$ satisfying (D1)–(D4), the dual axioms |
| **Idempotent element** | A value $z \in [0,1]$ such that $z \wedge z = z$ (or $z \vee z = z$) |
| **Idempotent set $A$** | $A = \{z \in [0,1] : z \wedge z = z\}$ |
| **Chimera** | A t-norm that behaves like $F^1_\wedge$ outside sub-intervals and like $F^2_\wedge$ or $F^3_\wedge$ on each sub-interval $[a_i, b_i]$ with $a_i, b_i \in A$ |
| **Type 2 interval** | Sub-interval $[a,b]$ with $a,b \in A$, $(a,b)\cap A = \varnothing$, and $z \wedge z \neq a$ for all $z \in (a,b)$ — isomorphic to $F^2_\wedge$ |
| **Type 3 interval** | Sub-interval $[a,b]$ with $a,b \in A$, $(a,b)\cap A = \varnothing$, and $z \wedge z = a$ for some $z \in (a,b)$ — isomorphic to $F^3_\wedge$ |
| **$\wedge$-exponentiation** | $x^{\wedge n} = x \wedge \cdots \wedge x$ ($n$ times); extended to rationals via IVT and to reals via limits |
| **Order-preserving isomorphism** | A bijection $\rho : [a,b] \to [c,d]$ with $\rho(x) \leq \rho(y) \iff x \leq y$ and $\rho(x \wedge y) = \rho(x) \star \rho(y)$ |
| **Order-reversing isomorphism** | A bijection $\rho$ with $\rho(x) \leq \rho(y) \iff x \geq y$ and the operation compatibility |
| **$B_\delta(a,b)$** | Open disc of radius $\delta$ centred at $(a,b)$: $\{(x,y) \in [0,1]^2 : (x-a)^2 + (y-b)^2 < \delta^2\}$ |
| **$F^1_\wedge$** | $\min(x,y)$ — the Godel/minimum t-norm |
| **$F^2_\wedge$** | $xy$ — the product t-norm |
| **$F^3_\wedge$** | $\max(0, x+y-1)$ — the Łukasiewicz t-norm |
| **$F^1_\vee$** | $\max(x,y)$ — the Godel/maximum t-conorm |
| **$F^2_\vee$** | $x + y - xy$ — the product t-conorm |
| **$F^3_\vee$** | $\min(1, x+y)$ — the Łukasiewicz t-conorm |

---

## Summary

- **LO1 (T-norm axioms):** A continuous t-norm satisfies (C1) Boolean boundary conditions, (C2) monotonicity, (C3) continuity (in the $\varepsilon$-$\delta$ sense over $[0,1]^2$), and (C4) associativity. Any such function also satisfies $x \wedge 0 = 0$, $x \wedge 1 = x$, and $x \wedge x \leq x$.

- **LO2 (The three t-norms):** $F^1_\wedge = \min$, $F^2_\wedge = xy$, and $F^3_\wedge = \max(0, x+y-1)$ all satisfy (C1)–(C4). The proofs are constructive $\varepsilon$-$\delta$ arguments.

- **LO3 (Continuity and limits):** If $\wedge$ is continuous (satisfies C3) then it commutes with limits of convergent sequences (Q7.4). This follows directly from the definition of continuity.

- **LO4 (Mostert–Shields for $\wedge$):** Any continuous t-norm is a chimera of $F^1_\wedge$, $F^2_\wedge$, $F^3_\wedge$. The idempotent set $A$ carves $[0,1]$ into sub-intervals; on each sub-interval the t-norm is either isomorphic to $F^2_\wedge$ (type 2) or $F^3_\wedge$ (type 3), and acts as $\min$ everywhere else.

- **LO5 (Isomorphisms):** The isomorphism to $F^2_\wedge$ is built via $\wedge$-exponentiation and logarithms; the isomorphism to $F^3_\wedge$ requires composing an order-reversing isomorphism with another order-reversing map $\sigma(x) = 2(1-x)$.

- **LO6 (Mostert–Shields for $\vee$):** The dual theorem for t-conorms holds by the duality $(x \wedge y) = 1 - ((1-x) \vee (1-y))$.

- **LO7 (Chimera example, Q7.5):** The function $x \wedge y = xy / \max(x, y, \frac{1}{2})$ is a chimera of $F^2_\wedge$ (on $[0, \frac{1}{2}]$, via the isomorphism $\rho(z) = 2z$) and $F^1_\wedge$ (outside). The idempotent set is $A = \{0\} \cup [\frac{1}{2}, 1]$.
