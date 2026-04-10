# Week 5: Introduction to Fuzzy Logics

## Learning Objectives
By the end of this week, you should be able to:
- [ ] Evaluate arbitrary propositional formulas in each of the three fuzzy logics $\mathbb{F}^1$ (Min-Max/Zadeh), $\mathbb{F}^2$ (Product/Gödel), and $\mathbb{F}^3$ (Łukasiewicz) under a given real-valued valuation
- [ ] Define Boolean and fuzzy truth functions and explain the motivation for extending truth values from $\{0,1\}$ to $[0,1]$
- [ ] State which classical tautologies remain valid in each fuzzy logic and construct counter-models (specific valuations) showing when they fail
- [ ] Define what it means for a formula to be **valid** (a **theorem**) with respect to a fuzzy logic $\mathbb{F}$
- [ ] Define **isomorphism of ordered structures** and verify that a given function is an order-preserving isomorphism
- [ ] State, prove, and apply **Trillas' Theorem**: that any "reasonable" negation function is isomorphic to $1 - x$
- [ ] Verify that a proposed negation function satisfies properties (N1)–(N4)
- [ ] Find the fixed point $c$ of a given negation function (where $\neg c = c$) using the Intermediate Value Theorem
- [ ] Construct an explicit order-preserving isomorphism $\rho$ witnessing the conclusion of Trillas' Theorem for a given negation
- [ ] State and apply the **Intermediate Value Theorem** and use it in formal arguments

---

## 5.1 Motivation and Definitions

### The Sorites Paradox

Consider the argument:

> **Premise 1:** Mr Whiskers is not bald.
> **Premise 2:** If Mr Whiskers is not bald and we pluck one hair from him, he would still not be bald.
> **Conclusion:** Mr Whiskers still remains not bald.

Repeating this argument indefinitely leads to the absurd conclusion that Mr Whiskers is not bald even after all his hair has been plucked. The issue: "baldness" (and similarly "a pile of sand", etc.) is not a binary switch — it is a **sliding scale**. Classical propositional logic, which assigns only truth values in $\{0, 1\}$, cannot model such **graded** properties.

### Definition 5.1: Boolean Truth Functions

A **Boolean truth function** on $n$ variables is a function $F : \{0,1\}^n \to \{0,1\}$ that takes an $n$-tuple of Boolean values and returns a Boolean output.

Each Boolean connective corresponds to a truth function:

$$F_\neg(x) = 1 - x$$
$$F_\wedge(x,y) = \min(x,y)$$
$$F_\vee(x,y) = \max(x,y)$$
$$F_\to(x,y) = \max(1-x, y)$$

A propositional formula $F$ with $n$ variables $p_1,\ldots,p_n$ corresponds to a truth function $\overline{F} : \{0,1\}^n \to \{0,1\}$ built by composing the above. The classical semantics says:

$$V(F) = 1 \iff \overline{F}(V(p_1),\ldots,V(p_n)) = 1$$

### Fuzzy Truth Functions and the Unit Interval

The key generalisation: allow truth values from the **unit interval** $[0,1] = \{x \in \mathbb{R} : 0 \leq x \leq 1\}$ rather than just $\{0,1\}$.

#### Definition 5.2: Fuzzy Truth Functions

A **fuzzy** (or **real-valued**) **truth function** on $n$ variables is a function $F : [0,1]^n \to [0,1]$.

A **model of fuzzy logic** is a function $V : \text{Prop} \to [0,1]$ assigning a real value $V(p) \in [0,1]$ to each propositional variable $p$.

### The Three Standard Fuzzy Logics

There are infinitely many possible fuzzy truth functions. In practice, three semantics are most common:

#### Min-Max / Zadeh Logic $\mathbb{F}^1$

$$F_\neg(x) = 1 - x$$
$$F_\wedge(x,y) = \min(x,y)$$
$$F_\vee(x,y) = \max(x,y)$$
$$F_\to(x,y) = \min(1, 1-x+y)$$

#### Product / Gödel Logic $\mathbb{F}^2$

$$F_\neg(x) = 1 - x$$
$$F_\wedge(x,y) = x \cdot y$$
$$F_\vee(x,y) = x + y - xy$$
$$F_\to(x,y) = \begin{cases} 1 & \text{if } x \leq y \\ y/x & \text{otherwise} \end{cases}$$

#### Łukasiewicz Logic $\mathbb{F}^3$

$$F_\neg(x) = 1 - x$$
$$F_\wedge(x,y) = \max(0,\, x + y - 1)$$
$$F_\vee(x,y) = \min(1,\, x + y)$$
$$F_\to(x,y) = \min(1,\, 1-x+y)$$

**Note:** All three logics agree on $F_\neg(x) = 1 - x$. Trillas' Theorem (Section 5.4) explains why this is inevitable for any "reasonable" negation.

### Definition 5.3: Validity and Models in Fuzzy Logic

Given an $n$-variable formula $F(p_1,\ldots,p_n)$, a model $V : \text{Prop} \to [0,1]$ is a **model of $F$ with respect to fuzzy logic $\mathbb{F}$** if $\overline{F}(V(p_1),\ldots,V(p_n)) = 1$.

$F$ is **valid with respect to $\mathbb{F}$** (or a **theorem of $\mathbb{F}$**) if $\overline{F}(x_1,\ldots,x_n) = 1$ for **all** $x_1,\ldots,x_n \in [0,1]$. Written:

$$\models^\mathbb{F} F \iff \overline{F}(x_1,\ldots,x_n) = 1 \text{ for all } x_1,\ldots,x_n \in [0,1]$$

To show $\not\models^\mathbb{F} F$, it suffices to find a single valuation making $\overline{F} < 1$.

---

## 5.2 Evaluating Formulas in the Three Logics

### Lecture Exercise: $V(p) = 0.3$, $V(q) = 0.8$

(From the lecture notes. The SGT uses $V(p) = 0.6$, $V(q) = 0.2$ — see Section 5.6.)

For each logic, compute $F_\neg(0.3) = 0.7$, then compose truth functions.

### SGT 5.1 Exercise: $V(p) = 0.6$, $V(q) = 0.2$

Full computed table (solution from SGT):

| Formula | Min-Max $\mathbb{F}^1$ | Product $\mathbb{F}^2$ | Łukasiewicz $\mathbb{F}^3$ |
|---------|----------------------|----------------------|--------------------------|
| $\neg p$ | $0.4$ | $0.4$ | $0.4$ |
| $p \wedge q$ | $\min(0.6, 0.2) = 0.2$ | $0.6 \times 0.2 = 0.12$ | $\max(0, 0.6+0.2-1) = 0$ |
| $p \vee q$ | $\max(0.6, 0.2) = 0.6$ | $0.6+0.2-0.12 = 0.68$ | $\min(1, 0.6+0.2) = 0.8$ |
| $p \to q$ | $\min(1,1-0.6+0.2) = 0.6$ | $0.2/0.6 \approx 0.3\overline{3}$ | $\min(1,1-0.6+0.2) = 0.6$ |
| $q \to p$ | $\min(1,1-0.2+0.6) = 1$ | $1$ (since $0.2 \leq 0.6$) | $\min(1,1-0.2+0.6) = 1$ |
| $p \wedge p$ | $\min(0.6,0.6) = 0.6$ | $0.6^2 = 0.36$ | $\max(0,0.6+0.6-1) = 0.2$ |
| $p \vee \neg p$ | $\max(0.6,0.4) = 0.6$ | $0.6+0.4-0.24 = 0.76$ | $\min(1,0.6+0.4) = 1$ |
| $p \wedge \neg p$ | $\min(0.6,0.4) = 0.4$ | $0.6 \times 0.4 = 0.24$ | $\max(0,0.6+0.4-1) = 0$ |

**Key observations:**
- $\mathbb{F}^3$ (Łukasiewicz) is the only logic where $p \vee \neg p = 1$ always (law of excluded middle holds), and $p \wedge \neg p = 0$ always (law of non-contradiction holds).
- $\mathbb{F}^1$ violates $p \wedge p = p$ (idempotency of $\wedge$) — since $V(p \wedge p) = V(p) \neq V(p)^2$.
- $\mathbb{F}^2$ violates idempotency of both $\wedge$ and $\vee$ when $V(p) \in (0,1)$.

---

## 5.3 Validity: What Holds in Each Fuzzy Logic

### SGT 5.2 Results

#### (i) $\models^{\mathbb{F}^1} (F \vee F) \to F$ for all $F \in \mathit{Fml}$ — **TRUE**

**Proof:** $V(F \vee F) = \max(V(F), V(F)) = V(F)$. Hence:

$$V((F \vee F) \to F) = \min(1,\, 1 - V(F) + V(F)) = \min(1,1) = 1. \quad \square$$

In $\mathbb{F}^1$, $\vee$ is idempotent: $V(F \vee F) = V(F)$.

#### (iii) $\not\models^{\mathbb{F}^2} (p \vee p) \to p$ — **FALSE in $\mathbb{F}^2$**

**Counter-model:** Choose $V(p) = 0.25$ (any $0 < V(p) < 1$ works).

In $\mathbb{F}^2$: $V(p \vee p) = 2V(p) - V(p)^2$. Then since $V(p \vee p) > V(p)$ (for $0 < V(p) < 1$):

$$V((p \vee p) \to p) = \frac{V(p)}{V(p \vee p)} = \frac{V(p)}{2V(p) - V(p)^2} < 1$$

This is $< 1$ whenever $2 - V(p) > 1$, i.e. whenever $V(p) < 1$. So any $0 < V(p) < 1$ is a counter-model. For $V(p) = 0.25$: $V(p \vee p) = 0.5 - 0.0625 = 0.4375$, so $V \to = 0.25/0.4375 \approx 0.571 < 1$. $\square$

**Conclusion:** In $\mathbb{F}^2$, $\vee$ is **not** idempotent — $V(F \vee F) \neq V(F)$ in general.

#### (iv) $\models^{\mathbb{F}^2} (F \wedge G) \to F$ for all $F, G \in \mathit{Fml}$ — **TRUE**

**Proof:** In $\mathbb{F}^2$: $V(F \wedge G) = V(F) \cdot V(G)$. Since $V(G) \leq 1$, we have $V(F \wedge G) \leq V(F)$, so $V(F \wedge G) \to F = 1$ by definition of $F^2_\to$. $\square$

#### (v) $\not\models^{\mathbb{F}^3} (p \to \neg p)$ — **FALSE in $\mathbb{F}^3$**

**Counter-model:** Choose $V(p) > 1/2$, e.g. $V(p) = 0.6$.

In $\mathbb{F}^3$: $V(\neg p) = 1 - V(p)$. So $V(p \to \neg p) = \min(1, 1 - V(p) + (1 - V(p))) = \min(1, 2 - 2V(p))$.

For $V(p) > 1/2$: $2 - 2V(p) < 1$, so $V(p \to \neg p) = 2 - 2(0.6) = 0.8 < 1$. $\square$

---

## 5.4 Interlude 1: The Intermediate Value Theorem

Many results in fuzzy logic depend on the following classical theorem from real analysis.

### Definition 5.4: Continuity

A function $f : [a,b] \to \mathbb{R}$ is **continuous at** $c \in [a,b]$ if $\lim_{x \to c} f(x) = f(c)$, i.e.:

$$\forall \varepsilon > 0;\ \exists \delta > 0;\ |x - c| < \delta \implies |f(x) - f(c)| < \varepsilon$$

$f$ is **continuous** (on $[a,b]$) if it is continuous at every $c \in [a,b]$.

### Theorem 5.5: The Intermediate Value Theorem (IVT)

> Let $f : [a,b] \to \mathbb{R}$ be any continuous real-valued function defined on $[a,b]$, and let $N \in \mathbb{R}$ be some value **between** $f(a)$ and $f(b)$. Then there is some $c \in [a,b]$ such that $f(c) = N$.

**Proof (case $f(a) < N < f(b)$):** Let $c = \sup(H)$ where $H = \{x \in [a,b] : f(x) \leq N\}$.

Note $H \neq \varnothing$ (since $a \in H$) and $H$ is bounded above by $b$, so $c = \sup(H)$ is well-defined. Also $c \in (a,b)$ since $f(a) < N < f(b)$.

**Case 1 — $f(c) < N$:** Let $\varepsilon = N - f(c) > 0$. By continuity, $\exists \delta > 0$ with $|x - c| < \delta \Rightarrow |f(x) - f(c)| < \varepsilon$. Let $d = c + \delta/2$; then $|f(d) - f(c)| < N - f(c)$ so $f(d) < N$, hence $d \in H$. But $d > c = \sup(H)$, contradiction.

**Case 2 — $f(c) > N$:** Let $\varepsilon = f(c) - N > 0$. By continuity, $\exists \delta > 0$ with $|x - c| < \delta \Rightarrow |f(x) - f(c)| < \varepsilon$. Let $d = c - \delta/2$. For all $x \in [d,c]$: $f(x) > N$, so $x \notin H$. Hence $H \subseteq [a,d]$, giving $d$ as an upper bound of $H$, contradicting $c > d$ being the **least** upper bound.

Since $<$ is a total linear order on $[0,1]$, we must have $f(c) = N$. $\square$

**Application (from lecture):** Use IVT to show there always exist two antipodal points on Earth with the same temperature. (Define $g(\theta) = T(\theta) - T(\theta + \pi)$ for temperature $T$; $g$ is continuous and $g(0) = -g(\pi)$, so IVT gives some $\theta_0$ with $g(\theta_0) = 0$.)

---

## 5.5 Interlude 2: Isomorphism of Ordered Structures

### Graph Isomorphism (Motivation)

Two structures are **isomorphic** if they are fundamentally the same up to relabelling. For graphs $G_1 = (V_1, E_1)$ and $G_2 = (V_2, E_2)$:

**Definition 5.6 (Graph Isomorphism):** $G_1 \cong G_2$ if there exists a bijection $\rho : V_1 \to V_2$ such that:

$$(x,y) \in E_1 \iff (\rho(x), \rho(y)) \in E_2$$

### Definition 5.7: Isomorphism of Ordered Structures

We work with **ordered structures** of the form $\langle I, <, f_1, \ldots, f_n \rangle$ where $I$ is an interval of $\mathbb{R}$, $<$ is a total linear ordering, and $f_1,\ldots,f_n$ are functions on $I$.

Two ordered structures $M_1 = \langle I, <, f \rangle$ and $M_2 = \langle I', <, g \rangle$ are **isomorphic** if there exists a bijection $\rho : I \to I'$ such that:

**Order is preserved:** For all $x, y \in I$:
$$x < y \iff \rho(x) < \rho(y)$$

**Functions are preserved:** For all $x_1,\ldots,x_n, y \in I$:
$$f(x_1,\ldots,x_n) = y \iff g(\rho(x_1),\ldots,\rho(x_n)) = \rho(y)$$

Such a $\rho$ is called an **isomorphism** between $M_1$ and $M_2$.

### Lecture Exercise: Examples of Isomorphic Structures

- $\langle [0,1], <, x^2 \rangle$ and $\langle [0,2], <, x^2/2 \rangle$ — isomorphism: $\rho(x) = 2x^2$ (maps $[0,1]$ to $[0,2]$, order-preserving, and $\rho(x^2) = 2x^4 \neq (2x^2)^2/2$... the precise isomorphism is to be worked out).

- $\langle \mathbb{R}^{>0}, <, \times \rangle$ and $\langle \mathbb{R}, <, + \rangle$ — isomorphism: $\rho(x) = \log(x)$ (the logarithm maps multiplication to addition and is order-preserving).

- $\langle [0,1], <, \min \rangle$ and $\langle [-1,1], >, \max \rangle$ — note the **reversal** of order; $\rho(x) = 1 - 2x$.

---

## 5.6 Trillas' Theorem

### Why All Three Logics Agree on Negation

Despite $\mathbb{F}^1$, $\mathbb{F}^2$, $\mathbb{F}^3$ differing on $F_\wedge$, $F_\vee$, $F_\to$, all three set $F_\neg(x) = 1 - x$. To understand this, introduce four desirable properties for a negation function $\neg : [0,1] \to [0,1]$:

**(N1) Boolean:** $\neg 0 = 1$ and $\neg 1 = 0$

**(N2) Involutive:** $\neg\neg x = x$ (double negation cancels)

**(N3) Continuous:** $\neg$ is continuous, i.e. $\forall \varepsilon > 0\ \exists \delta > 0$ such that $|x - y| < \delta \Rightarrow |\neg x - \neg y| < \varepsilon$

**(N4) Decreasing:** If $x < y$ then $\neg x \geq \neg y$ (as truth of $\varphi$ increases, truth of $\neg\varphi$ decreases)

**Note:** (N4) only says $\neg$ is not increasing; but any function satisfying (N1)–(N4) must actually be **strictly** decreasing.

**Note on (N2):** Implies $\neg$ is a bijection (both injective and surjective):
- *Injective*: if $\neg x = \neg y$ then $x = \neg\neg x = \neg\neg y = y$.
- *Surjective*: for any $y \in [0,1]$, $\neg(\neg y) = y$, so $\neg$ maps $\neg y \mapsto y$.

**Existence of a fixed point:** By (N1) and IVT: define $f(x) = \neg x - x$. Then $f(0) = \neg 0 - 0 = 1 > 0$ and $f(1) = \neg 1 - 1 = -1 < 0$. Since $f$ is continuous by (N3), IVT gives some $c \in [0,1]$ with $f(c) = 0$, i.e. $\neg c = c$.

### Theorem 5.8: Trillas' Theorem

> **If $\neg$ satisfies (N1)–(N4), then $\langle [0,1], <, \neg \rangle$ is isomorphic to $\langle [0,1], <, 1-x \rangle$. That is to say, $\neg x = 1 - x$, up to isomorphism.**

**Interpretation:** Any "reasonable" negation (satisfying (N1)–(N4)) is merely a rescaling of $1 - x$. Differences in choice of negation can be explained as differences in how we subjectively scale the interval $[0,1]$. This justifies why all standard fuzzy logics use $F_\neg(x) = 1 - x$ — it is the simplest representative.

**Proof:**

1. Define $f(x) = \neg x - x$. By (N1): $f(0) = 1 > 0$ and $f(1) = -1 < 0$. By (N3): $f$ is continuous. By IVT: $\exists c \in [0,1]$ with $\neg c = c$.

2. Define $\rho : [0,1] \to [0,1]$ by:

$$\rho(x) = \begin{cases} \dfrac{x}{2c} & \text{if } x \leq c \\ 1 - \dfrac{\neg x}{2c} & \text{if } x \geq c \end{cases}$$

3. Note $\rho(0) = 0$ and $\rho(1) = 1$ (using $\neg 1 = 0$ from (N1)).

4. **Claim:** $\rho(\neg x) = 1 - \rho(x)$.

   - If $x \leq c$: then $\neg x \geq \neg c = c$ (by (N4) and $\neg c = c$). So by (N2):
   $$\rho(\neg x) = 1 - \frac{\neg(\neg x)}{2c} = 1 - \frac{x}{2c} = 1 - \rho(x). \quad \checkmark$$

   - If $x > c$: then $\neg x < c$ (by (N4)). So:
   $$\rho(\neg x) = \frac{\neg x}{2c} = 1 - \left(1 - \frac{\neg x}{2c}\right) = 1 - \rho(x). \quad \checkmark$$

Hence $\rho$ is the required isomorphism. $\square$

---

## SGT Week 5 — Worked Examples

### Question 5.3: Isomorphism $\rho(x) = \log_2\!\left(\tfrac{x}{1-x}\right)$ between $M_1 = \langle (0,1), <, \oplus \rangle$ and $M_2 = \langle \mathbb{R}, <, + \rangle$

where $x \oplus y = \dfrac{xy}{xy + (1-x)(1-y)}$.

#### Part (i): Simplify $\dfrac{x \oplus y}{1-(x \oplus y)}$

$$1 - (x \oplus y) = 1 - \frac{xy}{xy+(1-x)(1-y)} = \frac{(1-x)(1-y)}{xy+(1-x)(1-y)}$$

Hence:

$$\frac{x \oplus y}{1-(x \oplus y)} = \frac{xy}{xy+(1-x)(1-y)} \div \frac{(1-x)(1-y)}{xy+(1-x)(1-y)} = \frac{xy}{(1-x)(1-y)}$$

#### Part (ii): Show $\rho(x) = \log_2\!\left(\tfrac{x}{1-x}\right)$ is an order-preserving isomorphism

**Step 1 — Order-preserving ($x < y \Rightarrow \rho(x) < \rho(y)$):**

$$\log_2\!\left(\frac{x}{1-x}\right) < \log_2\!\left(\frac{y}{1-y}\right) \iff \frac{x}{1-x} < \frac{y}{1-y} \quad \text{(since } \log_2 \text{ is increasing)}$$

$$\iff x(1-y) < y(1-x) \quad \text{(since } x,y < 1\text{)}$$

$$\iff x - xy < y - xy \iff x < y. \quad \checkmark$$

**Step 2 — Function-preserving ($\rho(x \oplus y) = \rho(x) + \rho(y)$):**

$$\rho(x) + \rho(y) = \log_2\!\left(\frac{x}{1-x}\right) + \log_2\!\left(\frac{y}{1-y}\right) = \log_2\!\left(\frac{x}{1-x} \cdot \frac{y}{1-y}\right) = \log_2\!\left(\frac{xy}{(1-x)(1-y)}\right)$$

$$= \log_2\!\left(\frac{x \oplus y}{1-(x \oplus y)}\right) = \rho(x \oplus y). \quad \checkmark \quad \square$$

---

### Question 5.4: Negation $\neg x = \dfrac{1-x}{1+x}$

#### Part (i): Verify (N1)–(N4)

**(N1):**
$$\neg 0 = \frac{1-0}{1+0} = 1, \qquad \neg 1 = \frac{1-1}{1+1} = 0. \quad \checkmark$$

**(N2) Involutive:**
$$\neg\neg x = \frac{1 - \frac{1-x}{1+x}}{1 + \frac{1-x}{1+x}} = \frac{(1+x)-(1-x)}{(1+x)+(1-x)} = \frac{2x}{2} = x. \quad \checkmark$$

**(N3) Continuous:** Compute $|\neg x - \neg y|$:

$$|\neg x - \neg y| = \left|\frac{1-x}{1+x} - \frac{1-y}{1+y}\right| = \left|\frac{(1-x)(1+y)-(1-y)(1+x)}{(1+x)(1+y)}\right| = \left|\frac{2(x-y)}{(1+x)(1+y)}\right| \leq 2|x-y|$$

(since $(1+x)(1+y) \geq 1$). Choose $\delta = \varepsilon/2$: if $|x-y| < \delta$ then $|\neg x - \neg y| \leq 2|x-y| < 2\delta = \varepsilon$. $\checkmark$

**(N4) Decreasing:** If $x < y$ then $1-x > 1-y$ and $\frac{1}{1+x} > \frac{1}{1+y}$. Hence:

$$\frac{1-x}{1+x} > \frac{1-x}{1+y} > \frac{1-y}{1+y}$$

so $\neg x > \neg y$. $\checkmark$

#### Part (ii): Find fixed point $c$ with $\neg c = c$

$$\frac{1-c}{1+c} = c \implies 1 - c = c(1+c) = c + c^2 \implies c^2 + 2c - 1 = 0$$

$$\implies (c+1)^2 = 2 \implies c = -1 + \sqrt{2} \approx 0.414$$

#### Part (iii): Construct isomorphism $\rho : [0,1] \to [0,1]$ with $\rho(\neg x) = 1 - \rho(x)$

With $c = -1 + \sqrt{2}$:

$$\rho(x) = \begin{cases} \dfrac{x}{2c} = \dfrac{x(1+\sqrt{2})}{2} & \text{if } x \leq -1+\sqrt{2} \\[8pt] 1 - \dfrac{1+\sqrt{2}}{2} \cdot \dfrac{1-x}{1+x} = \dfrac{1+(1-x)\sqrt{2}}{2+x} & \text{if } x \geq -1+\sqrt{2} \end{cases}$$

**Verification of key property for $x \leq c$:** $\neg x \geq c$ (by (N4), since $x \leq c = \neg c$ and $\neg$ is decreasing). So by (N2):

$$\rho(\neg x) = 1 - \frac{\neg(\neg x)}{2c} = 1 - \frac{x}{2c} = 1 - \rho(x). \quad \checkmark$$

---

### Question 5.5: Negation $\neg x = \sqrt{1 - x^2}$ (note: defined on $[0,1]$)

**Note on form:** This uses $\neg x = \sqrt{1-x^2}$ (i.e. $x$ is squared, not just $\neg x = \sqrt{1-x}$, as clarified by the N2 check).

#### Part (i): Verify (N1)–(N4) — [Tricky]

**(N1):**
$$\neg 0 = \sqrt{1-0^2} = 1, \qquad \neg 1 = \sqrt{1-1^2} = 0. \quad \checkmark$$

**(N2):**
$$\neg\neg x = \sqrt{1-(\sqrt{1-x^2})^2} = \sqrt{1-(1-x^2)} = \sqrt{x^2} = x. \quad \checkmark$$

**(N3) Continuous:** Express $\neg x = f_3(f_2(f_1(x)))$ where $f_1(x) = x^2$, $f_2(x) = 1-x$, $f_3(x) = \sqrt{x}$. Each is continuous; composition of continuous functions is continuous.

- $f_1(x) = x^2$: fix $a \in [0,1]$, $\varepsilon > 0$. Choose $\delta = \varepsilon/2$. Then $|x^2-a^2| = |(x-a)(x+a)| \leq 2|x-a| < \varepsilon$ (since $x+a \leq 2$).

- $f_2(x) = 1-x$: Choose $\delta = \varepsilon$. $|(1-x)-(1-a)| = |x-a| < \varepsilon$.

- $f_3(x) = \sqrt{x}$: fix $a \in [0,1]$, choose $\delta = \varepsilon\sqrt{a}$. Then:
$$|\sqrt{x} - \sqrt{a}| = \frac{|x-a|}{\sqrt{x}+\sqrt{a}} \leq \frac{|x-a|}{\sqrt{a}} < \frac{\delta}{\sqrt{a}} = \varepsilon. \quad \checkmark$$

**(N4):** If $x < y$ then $x^2 < y^2$ (since $x,y \in [0,1]$, so $0 < x < y \leq 1$). Then $1-y^2 > 1-x^2$... wait, $x < y \Rightarrow x^2 < y^2 \Rightarrow 1-x^2 > 1-y^2$. So $\neg x = \sqrt{1-x^2} > \sqrt{1-y^2} = \neg y$. $\checkmark$

#### Part (ii): Find fixed point $c$ with $\neg c = c$

$$\sqrt{1-c^2} = c \implies 1-c^2 = c^2 \implies 2c^2 = 1 \implies c = \frac{\sqrt{2}}{2} = \frac{1}{\sqrt{2}} \approx 0.707$$

#### Part (iii): Construct isomorphism $\rho : [0,1] \to [0,1]$ with $\rho(\neg x) = 1 - \rho(x)$

With $c = \sqrt{2}/2$:

$$\rho(x) = \begin{cases} \dfrac{x}{2c} = \dfrac{x\sqrt{2}}{2} & \text{if } x \leq \dfrac{\sqrt{2}}{2} \\[8pt] 1 - \dfrac{\sqrt{1-x^2}}{2c} = \dfrac{2 - \sqrt{2(1-x^2)}}{2} & \text{if } x \geq \dfrac{\sqrt{2}}{2} \end{cases}$$

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Boolean truth function** | A function $F : \{0,1\}^n \to \{0,1\}$ interpreting connectives on classical truth values |
| **Fuzzy truth function** | A function $F : [0,1]^n \to [0,1]$ generalising Boolean functions to real-valued truth |
| **Model of fuzzy logic** | A function $V : \text{Prop} \to [0,1]$ assigning a degree of truth in $[0,1]$ to each variable |
| **Valid / theorem of $\mathbb{F}$** | $F$ is valid w.r.t. $\mathbb{F}$ ($\models^\mathbb{F} F$) if $\overline{F}(x_1,\ldots,x_n)=1$ for all $x_i \in [0,1]$ |
| **$\mathbb{F}^1$ (Min-Max/Zadeh)** | Fuzzy logic with $\wedge = \min$, $\vee = \max$, $\neg = 1-x$, $\to = \min(1,1-x+y)$ |
| **$\mathbb{F}^2$ (Product/Gödel)** | Fuzzy logic with $\wedge = xy$, $\vee = x+y-xy$, $\neg = 1-x$, $\to = y/x$ (or $1$ if $x \leq y$) |
| **$\mathbb{F}^3$ (Łukasiewicz)** | Fuzzy logic with $\wedge = \max(0,x+y-1)$, $\vee = \min(1,x+y)$, $\neg = 1-x$, $\to = \min(1,1-x+y)$ |
| **Continuity** | $f$ is continuous at $c$ if $\lim_{x\to c} f(x) = f(c)$ (epsilon-delta definition) |
| **IVT** | If $f$ is continuous on $[a,b]$ and $N$ is between $f(a)$ and $f(b)$, then $\exists c \in [a,b]$ with $f(c) = N$ |
| **Isomorphism (ordered structures)** | A bijection $\rho : I \to I'$ that preserves both the ordering and all functions |
| **(N1) Boolean** | $\neg 0 = 1$ and $\neg 1 = 0$ |
| **(N2) Involutive** | $\neg\neg x = x$ for all $x \in [0,1]$ |
| **(N3) Continuous** | $\neg$ is a continuous function |
| **(N4) Decreasing** | $x < y \Rightarrow \neg x \geq \neg y$ |
| **Fixed point of $\neg$** | A value $c \in [0,1]$ with $\neg c = c$; guaranteed to exist by IVT under (N1)–(N3) |
| **Trillas' Theorem** | If $\neg$ satisfies (N1)–(N4), then $\langle [0,1], <, \neg \rangle \cong \langle [0,1], <, 1-x \rangle$ |

---

## Summary

This week introduced **fuzzy logic** as a framework for reasoning with graded truth:

- **LO1 (Evaluate in $\mathbb{F}^1$, $\mathbb{F}^2$, $\mathbb{F}^3$):** The three standard fuzzy logics provide three different ways to interpret $\wedge$, $\vee$, and $\to$ over $[0,1]$. All agree on $\neg = 1-x$. Evaluating formulas requires carefully composing the appropriate truth functions. The SGT 5.1 table gives complete worked values for $V(p)=0.6$, $V(q)=0.2$.

- **LO2 (Motivation):** Classical $\{0,1\}$ logic cannot model sorites-type ("heap"/"bald") arguments; relaxing truth values to $[0,1]$ resolves this.

- **LO3 (Validity):** Classical tautologies can fail in fuzzy logics. $p \vee \neg p = 1$ (excluded middle) holds only in $\mathbb{F}^3$; idempotency of $\vee$ fails in $\mathbb{F}^2$; $(p \to \neg p)$ fails in $\mathbb{F}^3$ for $V(p) > 0.5$.

- **LO4 (IVT):** The Intermediate Value Theorem is a key mathematical tool: it guarantees that continuous functions on $[a,b]$ hit every intermediate value, and it is used to prove existence of the fixed point $\neg c = c$.

- **LO5 (Isomorphism):** Two ordered structures are isomorphic if there is an order- and function-preserving bijection between them. The SGT 5.3 example showed $\langle(0,1),<,\oplus\rangle \cong \langle\mathbb{R},<,+\rangle$ via $\rho(x) = \log_2(x/(1-x))$.

- **LO6–LO7 (Trillas' Theorem):** Any negation satisfying (N1)–(N4) is isomorphic to $1-x$ via an explicit piecewise isomorphism $\rho$ built using the fixed point $c$. This justifies the universal use of $F_\neg(x) = 1-x$ across all standard fuzzy logics — all other reasonable negations are merely rescalings.
