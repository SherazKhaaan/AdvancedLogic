# Week 9: Introduction to Modal Logics — Kripke Semantics and Model-Checking

## Learning Objectives
By the end of this week, you should be able to:
- [ ] Explain why modal operators $\Diamond$ ("possibly") and $\Box$ ("necessarily") cannot be captured by truth-functional connectives
- [ ] Define well-formed modal formulas inductively
- [ ] Define a Kripke model $\mathfrak{M} = \langle W, R, V \rangle$ and a Kripke frame $\mathfrak{F} = (W, R)$
- [ ] Evaluate the truth value of a modal formula at a world in a Kripke model using the satisfaction relation $\mathfrak{M}, w \models F$
- [ ] Compute $W(F)$, the set of all worlds in which a formula $F$ is satisfied
- [ ] Distinguish between satisfiability at a world, validity over a model, validity over a frame, and validity over a class of frames
- [ ] Define the minimal modal logic $\mathbf{K}$ and state the Completeness Theorem
- [ ] Apply the Labelling Algorithm to perform model-checking on a Kripke model

---

## 9.1 Motivation: Why Modal Logic?

Classical propositional logic operates in a single, static world where every proposition is simply true or false. However, natural language reasoning often involves **modalities** — words like "possibly", "necessarily", "I know that", "I believe that", "it will be the case that" — which change the **mode of truth** away from this objective, here-and-now picture.

**Example:** Consider the premises:
- "It is not possible that Alice and Bob are both telling the truth"
- "It is possible that Alice is telling the truth"

From these we want to conclude "It is possible that Bob is lying." Classical propositional logic cannot express these modalities.

The solution is to introduce two new **unary connectives** called **modal operators**:

$$\Diamond F \quad : \quad \text{"} F \text{ is possibly true"}$$
$$\Box F \quad : \quad \text{"} F \text{ is necessarily true"}$$

### Why truth tables cannot work

Modal operators are **non-truth-functional**: the truth value of $\Diamond F$ is not determined solely by the truth value of $F$. There are only four possible truth tables for a unary connective; none captures "possibly" or "necessarily":

| $F$ | $\text{Op}_1 F$ | $F$ | $\text{Op}_2 F$ | $F$ | $\text{Op}_3 F$ | $F$ | $\text{Op}_4 F$ |
|-----|----------------|-----|----------------|-----|----------------|-----|----------------|
| 1   | 0              | 1   | 0              | 1   | 1              | 1   | 1              |
| 0   | 0              | 0   | 1              | 0   | 0              | 0   | 1              |

- $\text{Op}_1$: "false" operator — nothing is possible
- $\text{Op}_2$: negation — not useful
- $\text{Op}_3$: identity — only what is true is possible
- $\text{Op}_4$: "true" operator — nothing is impossible

None of these satisfactorily captures possibility/necessity. Hence modalities require a richer semantic framework.

---

## 9.2 Well-Formed Modal Formulas

**Definition 9.1 (Well-formed Modal Formulas).** Given a set of **propositional variables** $\text{Prop} = \{p_1, \ldots, p_n, \ldots\}$, the set $\mathit{Fml}^\Box$ of all **(well-formed) modal formulas** is defined inductively:

- If $p \in \text{Prop}$ then $p \in \mathit{Fml}^\Box$
- If $F, G \in \mathit{Fml}^\Box$ then so are:

$$\neg F, \quad (F \wedge G), \quad (F \vee G), \quad (F \to G), \quad \Box F, \quad \Diamond F$$

Note that classical propositional formulas are a subset: $\mathit{Fml} \subseteq \mathit{Fml}^\Box$.

**Examples of modal formulas:** $\Box p$, $\Diamond q$, $\Box(p \to q)$, $\Diamond \Box p$, $\Box \Diamond \neg p$, $\Diamond p \to \Box q$

---

## 9.3 Kripke Semantics

Introduced in 1963 by Saul Kripke. The key idea: to evaluate $\Diamond F$ at a world, we look at other **possible worlds** accessible from it.

### 9.3.1 Kripke Models

**Definition 9.2 (Kripke Model).** A **Kripke model** is a triple $\mathfrak{M} = \langle W, R, V \rangle$ where:

- $W$ is a non-empty set of **possible worlds** (vertices)
- $R \subseteq W \times W$ is a binary **accessibility relation** on $W$ (directed edges between worlds)
- $V : W \times \text{Prop} \to \{0, 1\}$ is a **propositional valuation** that assigns a Boolean value to each pair $(w, p)$

The pair $\mathfrak{F} = \langle W, R \rangle$ is the **Kripke frame** underlying $\mathfrak{M}$.

**Intuition:** $W$ is a directed graph of possible worlds. $(w, u) \in R$ means "world $u$ is accessible from world $w$". To evaluate possibility at $w$, we look at successors of $w$ in $R$.

### 9.3.2 Satisfiability and Validity

**Definition 9.3 (Satisfiability).** Given $\mathfrak{M} = \langle W, R, V \rangle$ and $F \in \mathit{Fml}^\Box$, the valuation $V$ is extended to all modal formulas via:

$$V(w, \neg F) = 1 - V(w, F)$$
$$V(w, F \wedge G) = \min(V(w, F),\ V(w, G))$$
$$V(w, F \vee G) = \max(V(w, F),\ V(w, G))$$
$$V(w, F \to G) = \max(1 - V(w, F),\ V(w, G))$$
$$V(w, \Diamond F) = \max\{V(u, F) : (w, u) \in R\}$$
$$V(w, \Box F) = \min\{V(u, F) : (w, u) \in R\}$$

We say $F$ is **satisfiable in $\mathfrak{M}$ at world $w$** if $V(w, F) = 1$, written:

$$\mathfrak{M}, w \models F \quad \longleftrightarrow \quad \text{"$F$ is satisfied in $\mathfrak{M}$ at world $w$"}$$

**Equivalent intuitive formulation:**

$$\mathfrak{M}, w \models \Diamond F \iff \exists u \in W;\ (w, u) \in R \text{ and } \mathfrak{M}, u \models F$$
$$\mathfrak{M}, w \models \Box F \iff \forall u \in W;\ (w, u) \in R \text{ implies } \mathfrak{M}, u \models F$$

So:
- $\Diamond F$ is true at $w$ iff $F$ is true at **some** accessible world
- $\Box F$ is true at $w$ iff $F$ is true at **every** accessible world

> **Note on empty successors:** If $w$ has no successors (no $(w, u) \in R$), then $\Box F$ is vacuously true and $\Diamond F$ is vacuously false at $w$.

### 9.3.3 The $W(F)$ Notation

We write $W(F) = \{w \in W : V(w, F) = 1\}$ for the **set of worlds where $F$ is satisfied**. This enables divide-and-conquer model checking.

### 9.3.4 Validity

**Definition 9.4 (Validity).**

- $F$ is **valid over the model** $\mathfrak{M}$ if $\mathfrak{M}, w \models F$ for all $w \in W$; written $\mathfrak{M} \models F$
- $F$ is **valid over the frame** $\mathfrak{F} = (W, R)$ if $\mathfrak{M} \models F$ for **every** Kripke model $\mathfrak{M}$ with underlying frame $\mathfrak{F}$; written $\mathfrak{F} \models F$
- $F$ is **valid over a class of frames** $\mathcal{C}$ if $\mathfrak{F} \models F$ for every $\mathfrak{F} \in \mathcal{C}$

The three uses of $\models$:
1. $\mathfrak{M}, w \models F$: fix model and world; existential over nothing
2. $\mathfrak{M} \models F$: fix model; universal over worlds
3. $\mathfrak{F} \models F$: fix frame; universal over worlds and valuations

### 9.3.5 The Minimal Modal Logic K

**Definition 9.5.** Given $\Gamma \subseteq \mathit{Fml}^\Box$ and $F \in \mathit{Fml}^\Box$, we write

$$\Gamma \models^{\mathbf{K}} F \iff \mathfrak{M}, w \models \Gamma \text{ implies } \mathfrak{M}, w \models F$$

for all Kripke models $\mathfrak{M} = \langle W, R, V \rangle$ and all $w \in W$.

The set of all **validities/tautologies** with respect to $\models^{\mathbf{K}}$ is:

$$\mathbf{K} = \{F \in \mathit{Fml}^\Box : \top \models^{\mathbf{K}} F\} = \{F : \mathfrak{F} \models F \text{ for all } \mathfrak{F}\}$$

$\mathbf{K}$ is called the **(minimal) modal logic**. For finite $\Gamma$:

$$\Gamma \models^{\mathbf{K}} F \iff \left(\bigwedge \Gamma \to F\right) \in \mathbf{K}$$

**Theorem 9.6 (Completeness of K).** For $\Gamma \subseteq \mathit{Fml}^\Box$ and $F \in \mathit{Fml}^\Box$:

$$\Gamma \models^{\mathbf{K}} F \iff \Gamma \vdash^{\mathbf{k}} F$$

(Semantic consequence coincides with provability in the proof system for $\mathbf{K}$.)

**Important tautology (Axiom K):**

$$\Box(p \to q) \to (\Box p \to \Box q)$$

This is valid in **all** Kripke frames.

---

## 9.4 Frame Properties and Associated Axioms

Some modal formulas are not valid in all frames, but are valid only when the accessibility relation has specific properties. Key examples:

| Axiom | Formula | Frame Property |
|-------|---------|---------------|
| $A_T$ (T) | $\Box p \to p$ | Reflexive: $(x,x) \in R$ for all $x$ |
| $A_4$ (4) | $\Diamond\Diamond p \to \Diamond p$ | Transitive: $(x,y),(y,z)\in R \Rightarrow (x,z)\in R$ |

### Reflexive Frames and $A_T$

The formula $A_T = \Box p \to p$ says "if $p$ is necessary then $p$ is true."

- **Not valid in all frames:** Take $W = \{a\}$, $R = \emptyset$, $V(a,p)=0$. Then $\Box p$ is vacuously true but $p$ is false, so $\mathfrak{M}, a \not\models A_T$.
- **Valid in all reflexive frames:** Proof by contradiction. Suppose $\mathfrak{M}, w \not\models \Box p \to p$ on a reflexive frame. Then $\mathfrak{M}, w \models \Box p$ and $\mathfrak{M}, w \not\models p$. Since the frame is reflexive, $(w,w) \in R$, so from $\Box p$ we get $\mathfrak{M}, w \models p$ — contradiction.

### Transitive Frames and $A_4$

The formula $A_4 = \Diamond\Diamond p \to \Diamond p$ says "if $p$ is possibly possibly true, then $p$ is possibly true."

- **Not valid in all frames:** Take $W = \{a,b,c\}$, $R = \{(a,b),(b,c)\}$, $V(c,p)=1$, all others 0. Then $\mathfrak{M}, a \models \Diamond\Diamond p$ (via $b$ then $c$), but $\mathfrak{M}, a \not\models \Diamond p$ (only $b$ accessible and $p$ is false there).
- **Valid in all transitive frames:** Proof by contradiction. Suppose $\mathfrak{M}, w \models \Diamond\Diamond p$ but $\mathfrak{M}, w \not\models \Diamond p$ on a transitive frame. From $\Diamond\Diamond p$: $\exists u$ with $(w,u)\in R$ and $\mathfrak{M}, u \models \Diamond p$. From $\Diamond p$ at $u$: $\exists v$ with $(u,v)\in R$ and $\mathfrak{M}, v \models p$. By transitivity, $(w,v)\in R$, so $\mathfrak{M}, w \models \Diamond p$ — contradicts $\mathfrak{M}, w \not\models \Diamond p$.

---

## 9.5 The Labelling Algorithm for Model-Checking

Given a Kripke model $\mathfrak{M} = \langle W, R, V \rangle$ and $H \in \mathit{Fml}^\Box$, the goal is to compute $W(H) \subseteq W$ — the set of all worlds where $H$ is satisfied. This is **model-checking**.

**Key insight:** Use divide-and-conquer on subformulas, processing them in order of increasing size (bottom-up).

**Pre-processing step:** Rewrite $H$ using these equivalences to eliminate $\to$ and $\Box$:

$$F \to G \mapsto (\neg F \vee G)$$
$$\Box F \mapsto \neg\Diamond\neg F$$

**Cases of the algorithm:**

**Case 1** ($H = p$, propositional variable):
$$W(p) = \{w \in W : V(w, p) = 1\}$$

**Case 2** ($H = \neg F$):
$$W(\neg F) = W \setminus W(F)$$

**Case 3** ($H = F \wedge G$):
$$W(F \wedge G) = W(F) \cap W(G)$$

**Case 4** ($H = F \vee G$):
$$W(F \vee G) = W(F) \cup W(G)$$

**Case 6** ($H = \Diamond F$):
$$W(\Diamond F) = \{w \in W : (w, v) \in R \text{ for some } v \in W(F)\}$$

(Worlds that can "see" some world in $W(F)$.)

After pre-processing, $\Box F$ becomes $\neg\Diamond\neg F$, handled by Cases 6 and 2.

To check **validity** over $\mathfrak{M}$: check whether $W(H) = W$.

---

## 9.6 Worked Examples: SGT Questions

### SGT 9.1 — Evaluating Formulas in a Kripke Model

**Model:** $\mathfrak{M} = \langle W, R, V \rangle$ with:
- $W = \{a, b, c, d, e\}$
- $R = \{(a,b),(a,c),(d,e),(a,e),(a,a),(e,e),(c,d),(d,c)\}$
- $V(w, p) = 1 \iff w \in \{a, e\}$ (so $W(p) = \{a, b, c\}$... wait — see solution below)
- $V(w, q) = 1 \iff w \in \{a, b, d\}$

> **Note on the model:** From the solutions, $W(p) = \{b, c, a\}$ i.e. $p$ holds at $a$, $b$, $c$; and $W(q) = \{b, d\}$ ... the solutions show $W(q) = \{b, d\}$ and the problem states $V(w,p)=1 \iff w \in \{a,e\}$ but the solutions compute $W(p)=\{b,c,a\}$. Use the solutions as authoritative.

From the solutions:
- $W(p) = \{b, c, a\}$, $W(q) = \{b, d\}$

Accessibility from each world:
- $a$ can see: $b, c, e, a$ (successors)
- $b$ can see: (none listed — no outgoing edges from $b$)
- $c$ can see: $d$
- $d$ can see: $e, c$
- $e$ can see: $e$

**(i) $\Box(p \vee \neg q)$**

Step by step:
$$W(q) = \{b, d\}$$
$$W(p) = \{a, b, c\}$$
$$W(\neg q) = \{a, c, e\}$$
$$W(p \vee \neg q) = \{a, b, c, e\}$$
$$W(\Box(p \vee \neg q)) = \{a, b, d, e\}$$

Explanation: $w \in W(\Box(p\vee\neg q))$ iff all successors of $w$ are in $\{a,b,c,e\}$. World $c$ has successor $d$, and $d \notin \{a,b,c,e\}$, so $c \notin W(\Box(p\vee\neg q))$.

**Answer:** $W(\Box(p \vee \neg q)) = \{a, b, d, e\}$

**(ii) $\Box\Diamond p$**

$$W(p) = \{b, c, a\}$$
$$W(\Diamond p) = \{a, d\}$$

($a$ can see $b,c,e,a$ — and $b,c,a \in W(p)$, so $a \in W(\Diamond p)$; $d$ can see $e, c$ — $c \in W(p)$, so $d \in W(\Diamond p)$; $b$ has no successors, so $b \notin W(\Diamond p)$; $c$ can see $d$ only, $d \notin W(p)$; $e$ can see $e$ only, $e \notin W(p)$.)

$$W(\Box\Diamond p) = \{b, c\}$$

($b$ has no successors so $\Box$ is vacuously true; $c$ has only successor $d$, and $d \in W(\Diamond p)$. World $a$ has successor $e$, $e \notin W(\Diamond p)$.)

**Answer:** $W(\Box\Diamond p) = \{b, c\}$

**(iii) $\Diamond(\Box p \wedge \Box\neg q)$**

$$W(q) = \{b, d\}$$
$$W(p) = \{a, b, c\}$$
$$W(\neg q) = \{a, c, e\}$$
$$W(\Box p) = \{b\}$$

($b$ has no successors, vacuously true; $a$ can see $b,c,e,a$ — all in $W(p)$? $e \notin W(p)$, so $a \notin W(\Box p)$; $e$ can see $e$, $e \notin W(p)$; etc.)

$$W(\Box\neg q) = \{b, d, e\}$$

($b$: no successors, vacuously true; $d$ can see $e,c$ — both in $W(\neg q) = \{a,c,e\}$? $e \in W(\neg q)$, $c \in W(\neg q)$ — yes; $e$ sees $e \in W(\neg q)$ — yes; $a$ sees $b,c,e,a$ — $b \notin W(\neg q)$, so $a \notin W(\Box\neg q)$; $c$ sees $d$, $d \notin W(\neg q)$.)

$$W(\Box p \wedge \Box\neg q) = \{b\} \cap \{b, d, e\} = \{b\}$$
$$W(\Diamond(\Box p \wedge \Box\neg q)) = \{a\}$$

($a$ can see $b$, and $b \in W(\Box p \wedge \Box\neg q)$.)

**Answer:** $W(\Diamond(\Box p \wedge \Box\neg q)) = \{a\}$

---

### SGT 9.2 — Distinguishing Worlds with Formulas

For the same model, find $F_w$ such that $W(F_w) = \{w\}$:

- **$w = a$:** $F_a = \Diamond(\Box p \wedge \Box\neg q)$ (from part (iii) above, or equivalently $F_a = \Diamond F_b$)
- **$w = b$:** $F_b = (p \wedge q)$, or (independent of valuation) $F_b = \Box\bot$ where $\bot$ is a propositional contradiction (since $b$ has no successors, $\Box\bot$ is vacuously true only at $b$)
- **$w = c$:** $F_c = p \wedge \Diamond(q \wedge \Diamond p)$
- **$w = d$:** $F_d = q \wedge \Diamond\neg q$
- **$w = e$:** $F_e = \neg q \wedge \Box\neg q$, or more simply $F_e = \neg(p \vee q)$

---

### SGT 9.3 — Proving a Formula Valid in All Frames

**Claim:** $A = \Diamond\neg p \leftrightarrow \neg\Box p$ is valid in all Kripke frames.

**Proof by contradiction:**

Suppose $\mathfrak{M}, w \not\models \Diamond\neg p \leftrightarrow \neg\Box p$ for some model and world. Then either:

**Case 1:** $\mathfrak{M}, w \models \Diamond\neg p$ and $\mathfrak{M}, w \not\models \neg\Box p$

- From $\Diamond\neg p$: $\exists u$ with $(w,u)\in R$ and $\mathfrak{M}, u \models \neg p$, i.e. $V(u,p) = 0$
- From $\not\models \neg\Box p$: $\mathfrak{M}, w \models \Box p$, so for all $(w,u)\in R$: $\mathfrak{M}, u \models p$, i.e. $V(u,p)=1$
- Contradiction: $V(u,p) = 0$ and $V(u,p) = 1$ simultaneously

**Case 2:** $\mathfrak{M}, w \not\models \Diamond\neg p$ and $\mathfrak{M}, w \models \neg\Box p$

- From $\models \neg\Box p$: $\mathfrak{M}, w \not\models \Box p$, so $\exists u$ with $(w,u)\in R$ and $\mathfrak{M}, u \not\models p$, i.e. $\mathfrak{M}, u \models \neg p$
- But then $\mathfrak{M}, w \models \Diamond\neg p$ (since $(w,u)\in R$ and $\mathfrak{M}, u \models \neg p$) — contradicts $\not\models \Diamond\neg p$

Both cases yield contradictions, so the formula is valid in all frames. $\square$

---

### SGT 9.4 — $A_T = \Box p \to p$ and Reflexive Frames

**(i) Counter-example for general frames:**

Minimal: $W = \{a\}$, $R = \emptyset$, $V(a,p)=0$.

Then $\Box p$ is vacuously true (no successors) but $p$ is false, so $\mathfrak{M}, a \not\models \Box p \to p$.

Slightly larger: $W = \{a,b\}$, $R = \{(a,b)\}$, $V(a,p)=0$, $V(b,p)=1$. Then $\mathfrak{M}, a \models \Box p$ but $\mathfrak{M}, a \not\models p$.

**(ii) Validity on reflexive frames:**

Proof by contradiction: suppose $\mathfrak{M}, w \not\models \Box p \to p$ on a reflexive frame.

Then $\mathfrak{M}, w \models \Box p$ (1) and $\mathfrak{M}, w \not\models p$ (2).

Since the frame is reflexive: $(w,w) \in R$. From (1), $\mathfrak{M}, w \models p$. Contradicts (2). $\square$

---

### SGT 9.5 — $A_4 = \Diamond\Diamond p \to \Diamond p$ and Transitive Frames

**(i) Counter-example for general frames:**

$W = \{a,b,c\}$, $R = \{(a,b),(b,c)\}$, $V(a,p)=V(b,p)=0$, $V(c,p)=1$.

Then $\mathfrak{M}, a \models \Diamond\Diamond p$ (reach $c$ via $b$), but $\mathfrak{M}, a \not\models \Diamond p$ (only $b$ accessible, $p$ false there).

**(ii) Validity on transitive frames:**

Proof by contradiction: suppose $\mathfrak{M}, w \models \Diamond\Diamond p$ (16) but $\mathfrak{M}, w \not\models \Diamond p$ (17) on a transitive frame.

From (16): $\exists u$ with $(w,u)\in R$ and $\mathfrak{M}, u \models \Diamond p$ (18).

From (18): $\exists v$ with $(u,v)\in R$ and $\mathfrak{M}, v \models p$ (19).

By transitivity: $(w,u),(u,v)\in R \Rightarrow (w,v)\in R$. So $\mathfrak{M}, w \models \Diamond p$ — contradicts (17). $\square$

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Modality** | A word/operator that changes the mode of truth (e.g., possibly, necessarily, knows, believes) |
| **Modal operator** | $\Diamond$ ("possibly") or $\Box$ ("necessarily"), unary connectives extending propositional logic |
| **Non-truth-functional** | An operator whose value is not determined solely by the truth value of its argument |
| **Well-formed modal formula** | Element of $\mathit{Fml}^\Box$, built inductively from propositional variables using $\neg, \wedge, \vee, \to, \Box, \Diamond$ |
| **Kripke model** | A triple $\mathfrak{M} = \langle W, R, V \rangle$: worlds, accessibility relation, valuation |
| **Kripke frame** | A pair $\mathfrak{F} = \langle W, R \rangle$: worlds and accessibility relation (no valuation) |
| **Possible world** | A node $w \in W$ representing a state of affairs with its own propositional valuation |
| **Accessibility relation** | $R \subseteq W \times W$; $(w,u)\in R$ means "$u$ is accessible from $w$" |
| **Satisfiability** | $\mathfrak{M}, w \models F$: formula $F$ is true at world $w$ in model $\mathfrak{M}$ |
| **Validity over model** | $\mathfrak{M} \models F$: $F$ is true at every world in $\mathfrak{M}$ |
| **Validity over frame** | $\mathfrak{F} \models F$: $F$ is true in every model on frame $\mathfrak{F}$ |
| **$W(F)$** | The set $\{w \in W : \mathfrak{M}, w \models F\}$ of worlds where $F$ is satisfied |
| **Minimal modal logic K** | The set of all modal formulas valid in all Kripke frames |
| **Reflexive frame** | A frame where $(x,x)\in R$ for all $x\in W$ |
| **Transitive frame** | A frame where $(x,y),(y,z)\in R \Rightarrow (x,z)\in R$ |
| **Model-checking** | Determining whether $\mathfrak{M}, w \models F$ for a given model, world, and formula |
| **Labelling Algorithm** | Bottom-up algorithm computing $W(F)$ by processing subformulas in order of size |

---

## Summary

- **LO 1 (Non-truth-functionality):** Modal operators $\Diamond$, $\Box$ cannot be captured by any of the four truth tables for a unary connective — they are non-truth-functional, requiring a richer semantic framework.
- **LO 2 (Syntax):** Modal formulas extend propositional logic by adding $\Box F$ and $\Diamond F$ as well-formed constructs for any formula $F$.
- **LO 3 (Kripke models):** A Kripke model $\mathfrak{M} = \langle W, R, V \rangle$ consists of possible worlds $W$, an accessibility relation $R$, and a valuation $V$. The underlying frame is $\mathfrak{F} = \langle W, R \rangle$.
- **LO 4 (Satisfaction):** $\mathfrak{M}, w \models \Diamond F$ iff $F$ holds at some $R$-successor of $w$; $\mathfrak{M}, w \models \Box F$ iff $F$ holds at all $R$-successors.
- **LO 5 (Computing $W(F)$):** Formulas are evaluated bottom-up: $W(\neg F) = W \setminus W(F)$, $W(F\wedge G) = W(F)\cap W(G)$, $W(\Diamond F)$ = worlds with an $R$-successor in $W(F)$.
- **LO 6 (Validity hierarchy):** Satisfiability at a world $\subset$ validity over model $\subset$ validity over frame $\subset$ validity over class of frames. Each step universally quantifies over more components.
- **LO 7 (Modal logic K):** The minimal modal logic $\mathbf{K}$ is the set of all formulas valid in every Kripke frame; it is sound and complete with respect to its proof system.
- **LO 8 (Frame properties):** $\Box p \to p$ is valid iff the frame is reflexive; $\Diamond\Diamond p \to \Diamond p$ is valid iff the frame is transitive. Counter-examples and proofs by contradiction are the standard tools for establishing these correspondences.
