# Week 11: Correspondence Theory — Sahlqvist Formulas and the van Benthem Algorithm

## Learning Objectives
By the end of this week, you should be able to:
- [ ] Explain why restricting to classes of frames is necessary for modelling epistemic, deontic, and other modal phenomena
- [ ] Compute the **standard translation** $\text{ST}_x(F)$ of a modal formula $F$ into first-order predicate logic
- [ ] State Theorem 11.2 relating modal satisfiability to first-order satisfiability via the standard translation
- [ ] Explain why quantifying over propositional variables leads into second-order logic and why this is problematic
- [ ] State Chagrova's Theorem on the undecidability of frame correspondence
- [ ] Define **positive** and **negative** formulas with respect to a propositional variable
- [ ] State Lemma 11.5 (monotonicity of positive/negative formulas)
- [ ] Define **atoms**, **boxed atoms**, and **Sahlqvist formulas** (including Sahlqvist antecedents and Sahlqvist implications)
- [ ] State Sahlqvist's Theorem (Theorem 11.8)
- [ ] Apply the **Sahlqvist–van Benthem algorithm** to compute the first-order correspondent of a Sahlqvist implication

---

## 11.1 Applications of Modal Logic and the Need for Frame Classes

So far we have interpreted $\Box$ and $\Diamond$ as "necessarily" and "possibly". However, the same Kripke semantics supports many other interpretations:

| Reading of $\Box p$ | Logic Type | Domain |
|---------------------|------------|--------|
| "I know $p$ to be true" | **Epistemic logic** | Knowledge |
| "The agent believes $p$" | **Doxastic logic** | Belief |
| "$p$ is obligatory" | **Deontic logic** | Obligation/permission |
| "$p$ will be true" | **Temporal logic** | Time |
| "After action $\alpha$, $p$ holds" | **Dynamic logic** | Programs/transitions |

For **multi-agent systems**, we introduce many operators $\Box_1, \Box_2, \ldots, \Box_n$ where $\Box_i p$ means "Agent $i$ knows that $p$ is true."

### Epistemic Axioms

For knowledge ("I know $p$"), we expect these properties to hold as **valid tautologies**:

| Name | Formula | Meaning |
|------|---------|---------|
| **True Knowledge** | $\Box p \to p$ | "If I know $p$, then $p$ must be true (I can't know false things)" |
| **Positive Introspection** | $\Box p \to \Box\Box p$ | "If I know $p$, I know that I know $p$" |
| **Negative Introspection** | $\neg\Box p \to \Box\neg\Box p$ | "If I don't know $p$, I know that I don't know $p$" |

**Exercise:** None of these is valid over the class of *all* Kripke frames. For example, $p \to \Diamond p$ (if $p$ is true, then $p$ is possible) is not valid in all frames: take $W = \{w, u\}$, $R = \{(w, u)\}$, $V(w, p) = 0$, $V(u, p) = 1$ — then at world $w$... (wait: actually at $w$: $p$ is false so the implication is vacuously true. The standard counter-example uses $W=\{w,u\}$, $R=\{(w,u)\}$, $V(w,p)=0$, $V(u,p)=1$, and checks $p\to\Diamond p$ at $w$: $p$ false at $w$, so true trivially. The actual counter-example for $p\to\Diamond p$ is: $W=\{w\}$, $R=\emptyset$, $V(w,p)=1$: then $p$ is true but $\Diamond p$ is false.)

The formula $p \to \Diamond p$ is valid precisely over **reflexive** frames. Similarly, True Knowledge $\Box p \to p$ is valid over reflexive frames.

To model knowledge accurately, we **restrict** our attention to the appropriate class of frames — those whose accessibility relation has the right structural properties. This raises the key question of **correspondence theory**: which modal formula corresponds to which first-order property of frames?

### Rumsfeld's Epistemology (Motivating Example)

Donald Rumsfeld's famous 2002 speech categorised knowledge into:
- **Known knowns:** $\Box p$ (I know $p$)
- **Known unknowns:** $\Box\neg\Box p$ (I know that I don't know $p$)
- **Unknown unknowns:** $\neg\Box p \wedge \neg\Box\neg\Box p$ (I neither know $p$ nor know that I don't know $p$)

Unknown unknowns ($\neg\Box p \wedge \neg\Box\neg\Box p$) directly refute **Negative Introspection**. Modal logic allows us to determine on which class of frames such things are possible.

---

## 11.2 First-Order Correspondence

### 11.2.1 Translating Kripke Models into Predicate Logic

The semantics of $\Diamond$ and $\Box$ is reminiscent of first-order quantifiers:

$$\mathfrak{M}, w \models \Diamond F \iff \exists u \in W;\ (w,u)\in R \text{ and } \mathfrak{M}, u \models F$$
$$\mathfrak{M}, w \models \Box F \iff \forall u \in W;\ (w,u)\in R \text{ implies } \mathfrak{M}, u \models F$$

Given a Kripke model $\mathfrak{M} = \langle W, R, V \rangle$, we can construct a first-order predicate model $\mathfrak{M}^* = \langle W, \mathcal{I} \rangle$ over a language with:
- A **unary predicate symbol** $P_i \in \text{Pred}$ for each propositional variable $p_i \in \text{Prop}$
- A **binary predicate symbol** $R \in \text{Pred}$ for the accessibility relation
- (Optionally) a **constant** $c_i \in \text{Cons}$ for each world $w_i \in W$

with interpretations: $R^\mathcal{I} = R$ and $P_i^\mathcal{I} = V(p_i)$ for all $p_i \in \text{Prop}$.

### 11.2.2 Standard Translation

**Definition 11.1 (Standard Translation).** Given a modal formula $F \in \mathit{Fml}^\Box$, the **standard translation of $F$** relative to a variable $x \in \text{Vars}$ is $\text{ST}_x(F)$, defined inductively:

$$\text{ST}_x(p_i) = P_i(x)$$
$$\text{ST}_x(\neg G) = \neg\,\text{ST}_x(G)$$
$$\text{ST}_x(G \odot H) = \text{ST}_x(G) \odot \text{ST}_x(H) \quad \text{for } \odot \in \{\wedge, \vee, \to\}$$
$$\text{ST}_x(\Diamond G) = \exists y\bigl(R(x,y) \wedge \text{ST}_y(G)\bigr)$$
$$\text{ST}_x(\Box G) = \forall y\bigl(R(x,y) \to \text{ST}_y(G)\bigr)$$

where $y \in \text{Vars}$ is a **fresh** first-order variable (not already used).

**In words:**
- Propositional variables translate to unary predicates applied to the current world variable $x$
- Boolean connectives are unchanged
- $\Diamond G$ becomes an existential quantifier over an accessible world $y$ where $\text{ST}_y(G)$ holds
- $\Box G$ becomes a universal quantifier over accessible worlds

### 11.2.3 Correspondence Theorem

**Theorem 11.2.** Let $\mathfrak{M} = \langle W, R, V \rangle$ be an arbitrary Kripke model and $F \in \mathit{Fml}^\Box$. Then:

$$\mathfrak{M}, x \models F \iff \mathfrak{M}^* \models^h \text{ST}_x(F) \tag{1}$$

where $\mathfrak{M}^* = \langle W, \mathcal{I} \rangle$ with $R^\mathcal{I} = R$ and $P_i^\mathcal{I} = V(p_i)$, and $h(x) = x$.

*Proof:* By induction on the size of $F$.

**Global satisfiability:** $F$ is globally satisfied in $\mathfrak{M}$ (i.e., true at all worlds):

$$F \text{ is globally satisfied in } \mathfrak{M} \iff \forall x\,\text{ST}_x(F) \text{ is satisfiable in } \mathfrak{M}^*$$

**Validity over frame $\mathfrak{F}$:**

$$F \text{ is valid over } \mathfrak{F} \iff \forall P_1 \ldots \forall P_n\,\forall x\,\text{ST}_x(F) \text{ is satisfiable in } \mathfrak{M}^*$$

> **Problem:** $\forall P_1 \ldots \forall P_n \forall x\,\text{ST}_x(F)$ is **not a well-formed formula of first-order predicate logic** — we are quantifying over *predicates*, not individual variables. This takes us into **second-order predicate logic**.

To obtain a **first-order** condition on the frame, we must **eliminate** the predicate quantifiers $P_1, \ldots, P_n$ from $\text{ST}_x(F)$ in a way that preserves satisfiability.

---

## 11.3 Sahlqvist Formulas

### 11.3.1 The Undecidability Barrier

**Theorem 11.3 (Chagrova's Theorem).** It is **undecidable** whether a given modal axiom has a first-order correspondent.

This means there is no general algorithm that, given any modal formula, produces its first-order frame correspondent. However, large useful subclasses of formulas do have computable correspondents.

### 11.3.2 Positive and Negative Formulas

**Definition 11.4 (Positive and Negative Formulas).** A formula $F$ is said to be **positive** (resp. **negative**) with respect to a variable $p \in \text{Prop}$ if every occurrence of $p$ in the conjunctive normal form of $F$ appears under the scope of an **even** (resp. **odd**) number of negation symbols.

$F$ is **positive** (resp. **negative**) if it is positive (resp. negative) with respect to **all** propositional variables appearing in $F$.

**Lemma 11.5 (Monotonicity).** Suppose $\mathfrak{M} = \langle W, R, V \rangle$ and $\mathfrak{M}' = \langle W, R, V' \rangle$ differ only in their valuation, with $V(p) \subseteq V'(p)$ for every $p \in \text{Prop}$ (i.e., $V'$ makes more atoms true):

- **(i)** If $F$ is **positive**: $\mathfrak{M}, w \models F \Rightarrow \mathfrak{M}', w \models F$ for all $w \in W$
- **(ii)** If $F$ is **negative**: $\mathfrak{M}', w \models F \Rightarrow \mathfrak{M}, w \models F$ for all $w \in W$

Positive formulas are **monotonically increasing** in the valuation; negative formulas are **monotonically decreasing**.

### 11.3.3 Atoms and Boxed Atoms

**Definition 11.6 (Atoms and Boxed Atoms).**
- An **atom** is any propositional variable $p \in \text{Prop}$
- A **boxed atom** is any formula of the form $\Box^n p$ for some $p \in \text{Prop}$ and $n \geq 1$ — i.e., a propositional variable under one or more $\Box$ operators

**Examples of boxed atoms:** $\Box p$, $\Box\Box q$, $\Box\Box\Box r$

### 11.3.4 Sahlqvist Formulas

**Definition 11.7 (Sahlqvist Formulas).**

A **Sahlqvist antecedent** is a modal formula built from $\top$, $\bot$, atoms, boxed atoms, and **negative formulas** using only $\wedge$ (conjunction) and $\vee$ (disjunction).

A **Sahlqvist implication** is a modal formula of the form $F \to G$ where:
- $F$ is a **Sahlqvist antecedent**, and
- $G$ is a **positive formula**

A **Sahlqvist formula** is a formula built from Sahlqvist implications by:
- freely applying $\Box$, and
- applying $\wedge$ and $\vee$ only between formulas that **share no propositional variables**

**Key intuition:** Sahlqvist antecedents allow nesting of $\Box$ inside $\Diamond$ (e.g., $\Diamond\Box p$) but **not** $\Diamond$ inside $\Box$ (e.g., $\Box\Diamond p$ is not a Sahlqvist antecedent because $\Diamond p$ is not negative).

**Examples:**
- $p \to \Diamond p$: Sahlqvist (antecedent $p$ is an atom; $\Diamond p$ is positive)
- $(p \wedge \Diamond\Box p) \to \Box p$: Sahlqvist (Example 2 from lectures)
- $\Diamond\Box p \to \Box\Diamond p$: **NOT Sahlqvist** (the consequent $\Box\Diamond p$ — while positive — but this formula in fact does not have a first-order correspondent, confirming non-Sahlqvist status)

**Theorem 11.8 (Sahlqvist's Theorem).**
- **(i)** Every Sahlqvist formula has a **first-order correspondent**
- **(ii)** There is an **algorithm** to compute the first-order correspondent of any Sahlqvist formula

---

## 11.4 Lambda Expressions (Interlude)

The algorithm uses $\lambda$**-notation** as a shorthand for describing predicates/subsets.

Given a formula $F(x)$ with free variable $x$, we write $(\lambda x.F(x))$ to denote the **unary predicate** "the set of all those $x$ who satisfy $F(x)$."

**Evaluation rule:** Applying $(\lambda x.F(x))$ at term $t$ gives:

$$(\lambda x.F(x))(t) = F(x/t)$$

i.e., uniformly substitute $t$ for $x$ in $F(x)$.

**Examples:**
- $(\lambda u.\ u = x)(y) = (y = x)$
- $(\lambda u.\ u = x \vee R(y, u))(z) = (z = x \vee R(y, z))$

Lambda expressions come from the **Lambda calculus**, which is computationally equivalent to Turing Machines. In Python, `lambda` functions are the same concept.

---

## 11.5 The Sahlqvist–van Benthem Algorithm

Proposed independently by **Henrik Sahlqvist** (1975) and **Johan van Benthem** (1976). Takes as input a **Sahlqvist implication** $F \to G$ and outputs its **first-order correspondent**.

### 11.5.1 The Algorithm

**Input:** A modal formula $F \in \mathit{Fml}^\Box$ (must be a Sahlqvist implication).

---

**Step 1: Compute the Standard Translation**

Compute $\text{ST}_x(F)$.

---

**Step 2: Pull out 'Diamonds'**

Wherever an **existential quantifier** appears in the **antecedent** of $\text{ST}_x(F)$, pull it to the front using these logical equivalences:

$$(\exists x\,A(x)) \wedge B \equiv \exists x(A(x) \wedge B) \quad \text{(when $x$ not free in $B$)}$$
$$(\exists x\,A(x)) \to B \equiv \forall x(A(x) \to B) \quad \text{(when $x$ not free in $B$)}$$

This converts the formula to the form $\forall x_1 \ldots \forall x_n[\cdots]$ with all existentials promoted out.

---

**Step 3: Group Terms**

Identify a predicate symbol $P_i$ and reorganise the antecedent into the standard form:

$$\forall x_1 \ldots \forall x_n \bigl[(\text{REL} \wedge \text{AT} \wedge \text{BOX} \wedge \text{NEG}) \to \text{POS}\bigr]$$

where:
- **REL** is a conjunction of **relational formulas** of the form $R(x,y)$ for variables $x,y$
- **AT** is a conjunction of **(translations of) atoms** of the form $P_i(x)$ for variable $x$
- **BOX** is a conjunction of **(translations of) boxed atoms** of the form $\text{ST}_x(\Box^n p_i)$
- **NEG** is a **negative** formula with respect to $P_i$
- **POS** is a **positive** formula with respect to $P_i$

---

**Step 4: Read off Minimal Instances**

Identify the **minimum valuation** $\sigma(P_i)$ — a lambda expression — that makes the conjunction $(\text{AT} \wedge \text{BOX})$ true.

$$\sigma(\text{AT} \wedge \text{BOX}) \equiv \top$$

The minimal valuation assigns $P_i$ to be true at exactly the worlds forced by AT and BOX:
- AT forces $P_i(x)$ at the current world $x$
- BOX forces $P_i$ at all worlds reachable from $x$ via $n$ steps of $R$

---

**Step 5: Apply the Minimal Substitution**

Apply $\sigma$ to $\text{ST}_x(F)$, substituting all occurrences of $P_i$ with $\sigma(P_i)$, and simplify the resulting formula. The result is free of $P_i$.

---

**Step 6 (Repeat if applicable)**

If any propositional variables remain in the formula (multiple variables in the original implication), repeat Steps 3–5 for the next variable.

---

### 11.5.2 Worked Example 1: $F = p \to \Diamond p$

**Step 1 — Standard Translation:**

$$\text{ST}_x(F) = P(x) \to \exists y\bigl(R(x,y) \wedge P(y)\bigr)$$

**Step 2 — Pull out diamonds:** No existential in the antecedent. The existential is in the consequent — no pulling needed.

**Step 3 — Group Terms:**

$$\text{AT} = P(x), \quad \text{POS} = \exists y(R(x,y) \wedge P(y))$$

(No REL, BOX, or NEG.)

**Step 4 — Minimal Valuation:**

The only information about $P$ from AT is $P(x)$ is true. So the minimal valuation assigns $P$ to be true at exactly $x$:

$$\sigma(P) = \lambda u.\ (u = x)$$

**Step 5 — Apply substitution:**

$$\sigma(\text{ST}_x(F)) = (\lambda u.u=x)(x) \to \exists y\bigl(R(x,y) \wedge (\lambda u.u=x)(y)\bigr)$$
$$= (x = x) \to \exists y\bigl(R(x,y) \wedge (y = x)\bigr)$$
$$= \top \to \exists y\bigl(R(x,y) \wedge y = x\bigr)$$
$$= \exists y\bigl(R(x,y) \wedge y = x\bigr)$$
$$= \exists y\bigl(R(x,x)\bigr)$$
$$\equiv R(x,x)$$

**Universally quantify over $x$:**

$$\forall x\,\sigma(\text{ST}_x(F)) \equiv \forall x\,R(x,x)$$

**Conclusion:** $p \to \Diamond p$ is valid precisely over the class of **reflexive** frames, with first-order correspondent $\forall x\,R(x,x)$. $\square$

---

### 11.5.3 Worked Example 2: $F = (p \wedge \Diamond\Box p) \to \Box p$

**Step 1 — Standard Translation:**

$$\text{ST}_x(F) = P(x) \wedge \exists y\bigl(R(x,y) \wedge \forall z(R(y,z) \to P(z))\bigr) \to \forall u\bigl(R(x,u) \to P(u)\bigr)$$

**Step 2 — Pull out diamonds** (existential in antecedent):

Using $(\exists y A(y)) \to B \equiv \forall y(A(y) \to B)$:

$$\text{ST}_x(F) \equiv \forall y\bigl[\bigl(P(x) \wedge R(x,y) \wedge \forall z(R(y,z) \to P(z))\bigr) \to \forall u(R(x,u) \to P(u))\bigr]$$

**Step 3 — Group Terms** (identify the components):

$$\underbrace{P(x)}_{\text{AT}} \wedge \underbrace{R(x,y)}_{\text{REL}} \wedge \underbrace{\forall z(R(y,z) \to P(z))}_{\text{BOX}} \to \underbrace{\forall u(R(x,u) \to P(u))}_{\text{POS}}$$

**Step 4 — Minimal Valuation:**

$P$ must be true at $x$ (from AT) and at all $R$-successors of $y$ (from BOX). So:

$$\sigma(P) = \lambda u.\ (u = x \vee R(y,u))$$

**Step 5 — Apply substitution:**

$$\sigma(\text{ST}_x(F)) = (\top \wedge R(x,y) \wedge \top) \to \forall u\bigl(R(x,u) \to (\lambda u.u=x\vee R(y,u))(u)\bigr)$$
$$= R(x,y) \to \forall u\bigl(R(x,u) \to (u=x \vee R(y,u))\bigr)$$

After variable relabelling and tidying (including the hidden $\forall y$):

$$\forall x\,\sigma(\text{ST}_x(F)) \equiv \forall x\forall y\forall z\bigl(R(x,y) \wedge R(y,z) \to (x=z \vee R(y,z))\bigr)$$

After simplification this yields the first-order correspondent describing a specific structural property of $R$.

**Full first-order correspondent:**

$$\forall x\forall y\forall z\bigl(R(x,y) \wedge R(y,z) \to (x = z \vee R(y,z))\bigr)$$

---

## 11.6 Standard Common Modal Axioms and Their Frame Correspondents

| Axiom Name | Modal Formula | Frame Property | First-Order Correspondent |
|------------|---------------|----------------|--------------------------|
| **T** (Reflexivity) | $\Box p \to p$ | Reflexive | $\forall x\,R(x,x)$ |
| **4** (Transitivity) | $\Box p \to \Box\Box p$ | Transitive | $\forall x\forall y\forall z(R(x,y)\wedge R(y,z)\to R(x,z))$ |
| **B** (Symmetry) | $p \to \Box\Diamond p$ | Symmetric | $\forall x\forall y(R(x,y)\to R(y,x))$ |
| **5** | $\Diamond p \to \Box\Diamond p$ | Euclidean | $\forall x\forall y\forall z(R(x,y)\wedge R(x,z)\to R(y,z))$ |
| **D** | $\Box p \to \Diamond p$ | Serial | $\forall x\,\exists y\,R(x,y)$ |
| **K** | $\Box(p\to q)\to(\Box p\to\Box q)$ | All frames | $\top$ (always valid) |

---

## 11.7 SGT Questions

### SGT 11.1 — Computing Standard Translations

**(i) $F = \Box p \to p$**

$$\text{ST}_x(F) = \forall y(R(x,y) \to P(y)) \to P(x)$$

**(ii) $F = p \to \Box\Diamond p$**

$$\text{ST}_x(F) = P(x) \to \forall y\bigl(R(x,y) \to \exists z(R(y,z) \wedge P(z))\bigr)$$

**(iii) $F = \Box\Box p \to \Box p$**

$$\text{ST}_x(F) = \forall y\bigl(R(x,y) \to \forall z(R(y,z) \to P(z))\bigr) \to \forall u(R(x,u) \to P(u))$$

---

### SGT 11.2 — Full Sahlqvist–van Benthem Worked Example

**Formula:** $F = \Diamond(p \wedge \Diamond\Box p) \to \Box p$

**Given standard translation:**

$$\text{ST}_x(F) = \exists y_1\Bigl(R(x,y_1) \wedge P(y_1) \wedge \exists y_2\bigl(R(y_1,y_2) \wedge \forall z(R(y_2,z)\to P(z))\bigr)\Bigr) \to \forall u(R(x,u)\to P(u))$$

**(i) Rewrite in REL $\wedge$ AT $\wedge$ BOX $\wedge$ NEG $\to$ POS form:**

Pull out existentials from the antecedent. Using $(\exists y A(y)) \to B \equiv \forall y(A(y)\to B)$:

Step 1 — pull $\exists y_1$:
$$\equiv \forall y_1\Bigl[R(x,y_1) \wedge P(y_1) \wedge \exists y_2\bigl(R(y_1,y_2)\wedge\forall z(R(y_2,z)\to P(z))\bigr) \to \forall u(R(x,u)\to P(u))\Bigr]$$

Step 2 — pull $\exists y_2$:
$$\equiv \forall y_1\forall y_2\Bigl[R(x,y_1) \wedge P(y_1) \wedge R(y_1,y_2) \wedge \forall z(R(y_2,z)\to P(z)) \to \forall u(R(x,u)\to P(u))\Bigr]$$

Identifying components:
$$\underbrace{R(x,y_1) \wedge R(y_1,y_2)}_{\text{REL}} \wedge \underbrace{P(y_1)}_{\text{AT}} \wedge \underbrace{\forall z(R(y_2,z)\to P(z))}_{\text{BOX}} \wedge \underbrace{\top}_{\text{NEG}} \to \underbrace{\forall u(R(x,u)\to P(u))}_{\text{POS}}$$

**(ii) Identify minimal substitution $\sigma(P)$:**

From AT: $P(y_1)$ must be true — $P$ is true at $y_1$.
From BOX: $P(z)$ must be true for all $z$ with $R(y_2,z)$ — $P$ is true at all $R$-successors of $y_2$.

So the minimal valuation making AT $\wedge$ BOX true is:

$$\sigma(P) = \lambda u.\ (u = y_1 \vee R(y_2, u))$$

**(iii) Apply minimal substitution and simplify:**

$$\sigma(\text{ST}_x(F)) = R(x,y_1) \wedge R(y_1,y_2) \wedge \underbrace{(\sigma(P)(y_1) = \top)}_{\text{AT trivially true}} \wedge \underbrace{(\forall z(R(y_2,z)\to\sigma(P)(z)) = \top)}_{\text{BOX trivially true}}$$
$$\to \forall u\bigl(R(x,u) \to \sigma(P)(u)\bigr)$$

$$= R(x,y_1) \wedge R(y_1,y_2) \to \forall u\bigl(R(x,u) \to (u = y_1 \vee R(y_2,u))\bigr)$$

Universally quantifying (including the hidden $\forall y_1\forall y_2$):

$$\forall x\forall y_1\forall y_2\Bigl[R(x,y_1)\wedge R(y_1,y_2) \to \forall u\bigl(R(x,u)\to(u=y_1\vee R(y_2,u))\bigr)\Bigr]$$

This is the first-order correspondent of $F$, describing frames where whenever $x$ sees $y_1$ sees $y_2$, every $R$-successor $u$ of $x$ is either $y_1$ itself or a successor of $y_2$.

**(iv) Example frame where $F$ is NOT valid:**

Take $W = \{x, y_1, y_2, u\}$ with $R = \{(x,y_1),(y_1,y_2),(x,u)\}$ where $u \neq y_1$ and $u$ is not a successor of $y_2$. This violates the first-order correspondent, so $F$ is not valid on this frame.

Simplest example: $W = \{a, b, c\}$, $R = \{(a,b),(b,c),(a,c)\}$ — but we need to check the specific condition. A counter-example frame where $F$ is not valid: $W = \{a, b\}$, $R = \{(a,b)\}$ (a frame without reflexivity or appropriate closure).

---

### SGT 11.3 — Modelling Agent Beliefs

Raymond models beliefs with $\Box F =$ "The agent believes $F$".

**(i) Modal representations of the criteria:**

| Criterion | Natural Language | Modal Formula |
|-----------|-----------------|---------------|
| **R1** | If agent believes $p$, they believe they believe $p$ | $\Box p \to \Box\Box p$ |
| **R2** | If agent doesn't believe $p$, they believe they don't believe $p$ | $\neg\Box p \to \Box\neg\Box p$ |
| **R3** | Agent doesn't simultaneously believe $p$ and $\neg p$ | $\neg(\Box p \wedge \Box\neg p)$, equivalently $\Box p \to \neg\Box\neg p$, or $\Box\bot \to \bot$ |
| **R4** | If agent believes $p$ and believes $p\to q$, they believe $q$ | $\Box p \wedge \Box(p\to q) \to \Box q$ |

**(ii) Frame classes via Sahlqvist–van Benthem:**

- **R1** ($\Box p \to \Box\Box p$): This is the **4 axiom**. Standard translation: $\forall y(R(x,y)\to P(y)) \to \forall y(R(x,y)\to\forall z(R(y,z)\to P(z)))$. First-order correspondent: $\forall x\forall y\forall z(R(x,y)\wedge R(y,z)\to R(x,z))$ — **transitivity**.

- **R2** ($\neg\Box p \to \Box\neg\Box p$): This is the **5 axiom** (in one reading). Equivalent to $\Diamond p \to \Box\Diamond p$ (by duality). First-order correspondent: $\forall x\forall y\forall z(R(x,y)\wedge R(x,z)\to R(y,z))$ — **Euclidean property** (if $x$ can see $y$ and $z$, then $y$ can see $z$).

- **R3**: The formula $\Box\bot \to \bot$ (or equivalently, consistency of beliefs) corresponds to **serial frames**: $\forall x\exists y\,R(x,y)$ (every world has at least one successor — the agent always has some state they consider possible).

- **R4** ($\Box p \wedge \Box(p\to q) \to \Box q$): Equivalent to $\Box(p\to q)\to(\Box p\to\Box q)$ — this is the **K axiom**, valid in **all frames**.

**(iii) Tricky criterion R*:**

R*: "The agent believes $p$ if they believe it is not the case that they believe $p$ despite $p$ being false."

As a modal formula: $\Box(\neg\Box(\neg p \wedge \Box p)) \to \Box p$, or more precisely:

$$\Box\neg(\Box p \wedge \neg p) \to \Box p$$

This is equivalent to (using tautological simplification): $\Box(\Box p \to p) \to \Box p$.

**Issues:** This formula has $\Box p$ appearing both positively and negatively — it is **not a Sahlqvist formula**. Specifically, the antecedent $\Box(\Box p \to p)$ contains $\Box p$ in a negative position (inside $\Box p \to p$, which is $\neg\Box p \vee p$, so $\Box p$ has negative polarity), and $\Box p$ in the consequent is positive. This formula involves $\Box p$ with mixed polarity, preventing us from identifying a minimal substitution. Therefore the Sahlqvist–van Benthem algorithm cannot be applied, and it is unclear (undecidable in general by Chagrova's Theorem) whether this formula even has a first-order frame correspondent.

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Epistemic logic** | Modal logic where $\Box p$ means "the agent knows $p$" |
| **Doxastic logic** | Modal logic where $\Box p$ means "the agent believes $p$" |
| **Deontic logic** | Modal logic where $\Box p$ means "$p$ is obligatory" |
| **Standard translation** $\text{ST}_x(F)$ | Inductive translation of modal formula $F$ into first-order predicate logic relative to variable $x$ |
| **Second-order predicate logic** | Extension of first-order logic allowing quantification over predicates/sets |
| **First-order correspondent** | A first-order formula describing the class of frames validating a given modal formula |
| **Chagrova's Theorem** | It is undecidable whether a given modal axiom has a first-order correspondent |
| **Positive formula** | Formula where every propositional variable occurs under an even number of negations (monotonically increasing in valuation) |
| **Negative formula** | Formula where every propositional variable occurs under an odd number of negations (monotonically decreasing in valuation) |
| **Atom** | Any propositional variable $p \in \text{Prop}$ |
| **Boxed atom** | A formula $\Box^n p$ for some $p\in\text{Prop}$, $n\geq 1$ (variable under one or more boxes) |
| **Sahlqvist antecedent** | Formula built from $\top,\bot$, atoms, boxed atoms, and negative formulas using $\wedge$ and $\vee$ only |
| **Sahlqvist implication** | Formula $F\to G$ where $F$ is a Sahlqvist antecedent and $G$ is positive |
| **Sahlqvist formula** | Formula built from Sahlqvist implications using $\Box$, and $\wedge$/$\vee$ between variable-disjoint parts |
| **Sahlqvist's Theorem** | Every Sahlqvist formula has a first-order correspondent, computable by algorithm |
| **$\lambda$-expression** | $(\lambda x.F(x))$: the set/predicate of all $x$ satisfying $F(x)$; $(\lambda x.F(x))(t) = F(x/t)$ |
| **Minimal valuation** $\sigma(P_i)$ | The smallest (fewest worlds) valuation making AT $\wedge$ BOX trivially true |
| **Sahlqvist–van Benthem algorithm** | 5-step algorithm computing the first-order correspondent of a Sahlqvist implication |
| **True Knowledge axiom** | $\Box p \to p$ — valid on reflexive frames |
| **Positive Introspection axiom** | $\Box p \to \Box\Box p$ — valid on transitive frames |
| **Negative Introspection axiom** | $\neg\Box p \to \Box\neg\Box p$ — valid on Euclidean frames |

---

## Summary

- **LO 1 (Frame classes and applications):** Different interpretations of $\Box$ (knowledge, belief, obligation, time) require different frame properties. Restricting to appropriate classes of frames (reflexive, transitive, serial, Euclidean) validates the desired tautologies.
- **LO 2 (Standard translation):** $\text{ST}_x(F)$ translates a modal formula into first-order predicate logic, mapping $\Diamond$ to $\exists$ and $\Box$ to $\forall$. Theorem 11.2 guarantees modal and predicate satisfiability coincide.
- **LO 3 (Second-order problem):** Frame validity requires quantifying over predicates $P_1,\ldots,P_n$, taking us into second-order logic. We need to eliminate predicate quantifiers to get a first-order frame condition.
- **LO 4 (Chagrova's Theorem):** In general, it is undecidable whether a modal formula has a first-order correspondent, so no universal algorithm exists.
- **LO 5 (Positive/Negative formulas):** Positive formulas are monotonically increasing; negative formulas are monotonically decreasing with respect to the valuation. This monotonicity is exploited by the Sahlqvist algorithm.
- **LO 6 (Sahlqvist formulas):** Sahlqvist antecedents allow $\Diamond$ and $\Box^n$ applied to atoms but restrict polarity. Sahlqvist implications ($F\to G$ with $F$ a Sahlqvist antecedent and $G$ positive) all have computable first-order correspondents.
- **LO 7 (Sahlqvist's Theorem):** Every Sahlqvist formula has a first-order correspondent, guaranteed to exist and computable by the Sahlqvist–van Benthem algorithm.
- **LO 8 (The algorithm):** Five steps: (1) compute $\text{ST}_x(F)$; (2) pull existentials to front; (3) group into REL/AT/BOX/NEG $\to$ POS; (4) find minimal valuation $\sigma(P_i)$ making AT$\wedge$BOX $\equiv\top$; (5) apply $\sigma$, simplify, and universally quantify. Repeat for each propositional variable.
