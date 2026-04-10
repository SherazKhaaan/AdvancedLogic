# Week 4: Semantics for Rational Consequence — Ranked Preferential Models and the Representation Theorem

## Learning Objectives
By the end of this week, you should be able to:
- [ ] Define what a **ranked preferential model** is and describe its components
- [ ] Determine whether $F \approxeq_{\vec{S}} G$ holds for a given ranked preferential model $\vec{S}$ by applying Definition 2.2
- [ ] Prove that $\approxeq_{\vec{S}}$ is a rational consequence relation for all ranked models $\vec{S}$ (Theorem 2.3), verifying each Gabbay–Makinson rule in turn
- [ ] Apply the Representation Theorem (Theorem 2.4) to show that certain properties/rules hold for all rational consequence relations
- [ ] Use ranked preferential models as counter-models to show that certain conclusions **cannot** be derived from a given set of premises
- [ ] Prove derived rules (Supra-classicality, Conditionalisation, Cautious Cut) using the Representation Theorem
- [ ] Understand the relationship between preferential models (partial order) and ranked preferential models (total linear order), and why RMON distinguishes them

---

## 2.1 Ranked Preferential Models

### Motivation

So far, rational consequence relations have been defined purely **syntactically** — via the Gabbay–Makinson (GM) rules — capturing intuitions about the word "usually". However, as in classical propositional logic, we want a **semantic** definition that is sound and complete with respect to the proof theory. Ranked preferential models provide exactly such a semantics.

### Definition 2.1: Ranked Preferential Model

A **ranked preferential model** is an ordered sequence

$$\vec{S} = \langle S_1, S_2, \ldots, S_k \rangle$$

of length $k$, for some $k \geq 1$, where each $S_i$ is a **set of propositional valuations**.

**Intuition:** $\vec{S}$ gives an ordering over **"most usual" worlds**. $S_1$ contains the most usual worlds; $S_k$ contains the most exceptional cases. Note: the definition does **not** require that every possible valuation appears in some $S_i$ — models deemed impossible need not appear anywhere.

### Definition 2.2: Associated Consequence Relation

Given a ranked preferential model $\vec{S} = \langle S_1, \ldots, S_k \rangle$, define the associated consequence relation $\approxeq_{\vec{S}}$ by:

$$F \approxeq_{\vec{S}} G \iff \begin{cases} \text{mod}(F) \cap S_i = \varnothing & \text{for all } 1 \leq i \leq k, \\ \text{or} & \\ \text{mod}(F) \cap S_j \subseteq \text{mod}(G) & \text{where } j \text{ is minimal s.t. } \text{mod}(F) \cap S_j \neq \varnothing \end{cases}$$

for all $F, G \in \mathit{Fml}$.

**In words:** To check $F \approxeq_{\vec{S}} G$, successively check $S_1, S_2, \ldots$ until finding the first layer $S_j$ that contains at least one valuation satisfying $F$. Then check whether every valuation in $S_j$ that satisfies $F$ also satisfies $G$. If no layer contains any model of $F$, then $F \approxeq_{\vec{S}} G$ holds automatically (the "vacuous" case — $F$ is deemed impossible).

This mirrors the classical check $\text{mod}(F) \subseteq \text{mod}(G)$, but restricted to the single most-usual layer $S_j$.

### Useful Set-Theoretic Identities

For any $F, G \in \mathit{Fml}$:

$$\text{mod}(F \wedge G) = \text{mod}(F) \cap \text{mod}(G)$$

$$\text{mod}(F \vee G) = \text{mod}(F) \cup \text{mod}(G)$$

$$\text{mod}(\neg F) = \overline{\text{mod}(F)}$$

(where $\overline{X}$ denotes the complement of $X$ in the set of all propositional valuations).

### Worked Exercise from Lecture (Definition 2.2 illustration)

Consider $\vec{S} = \langle S_1, S_2, S_3 \rangle$ where:

$$S_1 = \{(p \wedge \neg q \wedge \neg r)\}$$
$$S_2 = \{(\neg p \wedge q \wedge r),\ (p \wedge q \wedge \neg r)\}$$
$$S_3 = \{(p \wedge \neg q \wedge r)\}$$

**(i) $p \approxeq_{\vec{S}} \neg q$?**

$S_1$ is minimal with $\text{mod}(p) \cap S_1 \neq \varnothing$. $\text{mod}(p) \cap S_1 = \{(p \wedge \neg q \wedge \neg r)\} \subseteq \text{mod}(\neg q)$. **True.**

**(ii) $(p \wedge r) \approxeq_{\vec{S}} q$?**

$\text{mod}(p \wedge r) \cap S_1 = \varnothing$, $\text{mod}(p \wedge r) \cap S_2 = \varnothing$. $S_3$ is minimal: $\text{mod}(p \wedge r) \cap S_3 = \{(p \wedge \neg q \wedge r)\} \not\subseteq \text{mod}(q)$. **False.**

**(iii) $q \approxeq_{\vec{S}} (p \to r)$?**

$\text{mod}(q) \cap S_1 = \varnothing$. $S_2$ is minimal: $\text{mod}(q) \cap S_2 = \{(\neg p \wedge q \wedge r), (p \wedge q \wedge \neg r)\}$. Is $(p \wedge q \wedge \neg r) \in \text{mod}(p \to r) = \text{mod}(\neg p \vee r)$? No. **False.**

---

## 2.2 The Representation Theorem: Part I

### Theorem 2.3

> **Let $\vec{S} = \langle S_1, \ldots, S_k \rangle$ be any ranked preferential model. Then $\approxeq_{\vec{S}}$ is a rational consequence relation.**

**Proof strategy:** Show $\approxeq_{\vec{S}}$ satisfies each GM rule. In every case: if $\text{mod}(F) \cap S_i = \varnothing$ for all $i$, the conclusion holds trivially; otherwise let $j$ be minimal with $\text{mod}(F) \cap S_j \neq \varnothing$.

#### (REF) Reflexivity: $F \approxeq_{\vec{S}} F$

Let $j$ be minimal with $\text{mod}(F) \cap S_j \neq \varnothing$. Then $\text{mod}(F) \cap S_j \subseteq \text{mod}(F)$ trivially. $\square$

#### (R$\wedge$) Right And: If $F \approxeq_{\vec{S}} G$ and $F \approxeq_{\vec{S}} H$, then $F \approxeq_{\vec{S}} G \wedge H$

At the minimal layer $j$: $\text{mod}(F) \cap S_j \subseteq \text{mod}(G)$ and $\text{mod}(F) \cap S_j \subseteq \text{mod}(H)$, so

$$\text{mod}(F) \cap S_j \subseteq \text{mod}(G) \cap \text{mod}(H) = \text{mod}(G \wedge H). \quad \square$$

#### (L$\vee$) Left Or: If $F \approxeq_{\vec{S}} H$ and $G \approxeq_{\vec{S}} H$, then $F \vee G \approxeq_{\vec{S}} H$

Let $j$ be minimal with $\text{mod}(F \vee G) \cap S_j \neq \varnothing$. Since $\text{mod}(F \vee G) = \text{mod}(F) \cup \text{mod}(G)$:

$$\text{mod}(F \vee G) \cap S_j = (\text{mod}(F) \cap S_j) \cup (\text{mod}(G) \cap S_j) \neq \varnothing$$

If $\text{mod}(F) \cap S_j \neq \varnothing$, then $j$ is also minimal for $F$, so $\text{mod}(F) \cap S_j \subseteq \text{mod}(H)$. Similarly for $G$. Hence both sub-parts lie in $\text{mod}(H)$, giving $\text{mod}(F \vee G) \cap S_j \subseteq \text{mod}(H)$. $\square$

#### (LLE) Left Logical Equivalence: If $F \approxeq_{\vec{S}} H$ and $F \equiv G$ (i.e. $\text{mod}(F) = \text{mod}(G)$), then $G \approxeq_{\vec{S}} H$

$\text{mod}(G) \cap S_j = \text{mod}(F) \cap S_j \subseteq \text{mod}(H)$. $\square$

#### (RWE) Right Weakening: If $F \approxeq_{\vec{S}} G$ and $G \models H$ (i.e. $\text{mod}(G) \subseteq \text{mod}(H)$), then $F \approxeq_{\vec{S}} H$

$\text{mod}(F) \cap S_j \subseteq \text{mod}(G) \subseteq \text{mod}(H)$. $\square$

#### (CMON) Cautious Monotonicity: If $F \approxeq_{\vec{S}} G$ and $F \approxeq_{\vec{S}} H$, then $F \wedge G \approxeq_{\vec{S}} H$

Let $j$ be minimal with $\text{mod}(F \wedge G) \cap S_j \neq \varnothing$. Then:

$$\text{mod}(F \wedge G) \cap S_j = (\text{mod}(F) \cap \text{mod}(G)) \cap S_j \subseteq \text{mod}(F) \cap S_j \subseteq \text{mod}(H). \quad \square$$

#### (RMON) Rational Monotonicity: If $F \approxeq_{\vec{S}} H$ and $F \not\approxeq_{\vec{S}} \neg G$, then $F \wedge G \approxeq_{\vec{S}} H$

Since $F \not\approxeq_{\vec{S}} \neg G$, there exists some $\text{mod}(F) \cap S_j \not\subseteq \text{mod}(\neg G)$, which means $\text{mod}(F) \cap \text{mod}(G) \cap S_j \neq \varnothing$.

Let $j$ be minimal with $\text{mod}(F \wedge G) \cap S_j \neq \varnothing$. From $F \not\approxeq_{\vec{S}} \neg G$, for all $i < j$:

$$\text{mod}(F) \cap S_i \subseteq \overline{\text{mod}(G)} = \text{mod}(\neg G)$$

which means $\text{mod}(F \wedge G) \cap S_i = \varnothing$. So $j$ is also the minimal layer for $F$:

$$\text{mod}(F \wedge G) \cap S_j \subseteq \text{mod}(F) \cap S_j \subseteq \text{mod}(H). \quad \square$$

### Application: Showing Consequences Cannot Be Derived

A key use of Theorem 2.3 is the contrapositive: **to show a conclusion $F \approxeq G$ cannot be derived from the GM rules**, it suffices to construct a ranked preferential model that satisfies all premises but fails the conclusion. Since every $\approxeq_{\vec{S}}$ is rational, if the conclusion were provable from the premises via GM rules, it would hold in every rational consequence relation — in particular in our constructed model.

**Example from lecture (birds, penguins, flying):**

Premises: $b \mid\!\!\sim f$, $p \mid\!\!\sim \neg f$, $p \wedge \neg b \mid\!\!\sim \bot$.

Claim: We cannot conclude $\neg f \mid\!\!\sim p$ ("non-flying things are usually penguins").

Construct a model satisfying all three premises but with $\neg f \not\mid\!\!\sim p$, to refute the conclusion.

### Historical Note: Non-Ranked Preferential Models

Ranked preferential models $\vec{S} = \langle S_1, \ldots, S_k \rangle$ can be re-expressed as a pair $(\{S_1,\ldots,S_k\}, \prec)$ where $\prec$ is a **total linear ordering** ($S_i \prec S_j$ iff $i < j$).

Relaxing to a **partial order** (reflexive, transitive, anti-symmetric) gives a **preferential model**, with a suitably modified consequence relation. This satisfies all GM rules **except** Rational Monotonicity **(RMON)**. If RMON is judged unreasonable, preferential models are a viable alternative.

---

## 2.3 The Representation Theorem: Part II

### Theorem 2.4: The Representation Theorem

> **Let $\mid\!\!\sim$ be any rational consequence relation. Then there is some ranked preferential model $\vec{S}$ such that $\approxeq_{\vec{S}} \;=\; \mid\!\!\sim$.**

This is the converse of Theorem 2.3. Together, Theorems 2.3 and 2.4 give a complete characterisation: a relation is rational if and only if it is generated by some ranked preferential model.

### Proof Sketch (Constructive)

Assume $\mid\!\!\sim \;= \approxeq_{\vec{S}}$ for some $\vec{S} = \langle S_1, \ldots, S_k \rangle$. The construction proceeds in steps.

**Step 1 — Deduce $S_1$:** The most usual worlds are those in which every "usually true" formula (i.e. every $F$ such that $\top \mid\!\!\sim F$) holds:

$$S_1 = \bigcap\{\text{mod}(F) : \top \mid\!\!\sim F\}$$

This is the largest set of valuations satisfying all formulas that usually follow from a tautology.

**Step 2 — Deduce $S_2$:** Let $\Theta_1$ be a formula such that $\text{mod}(\Theta_1) = \overline{S_1}$ (true exactly outside $S_1$). Then define $S_2$ as the most usual worlds among those **not** in $S_1$:

$$S_2 = \bigcap\{\text{mod}(F) : \Theta_1 \mid\!\!\sim F\}$$

**Step $i+1$ — General Recursion:** Let $\Theta_i \in \mathit{Fml}$ be such that:

$$\text{mod}(\Theta_i) = \overline{S_1 \cup \cdots \cup S_i} \tag{3}$$

Then:

$$S_{i+1} = \bigcap\{\text{mod}(F) : \Theta_i \mid\!\!\sim F\} \tag{4}$$

**Termination:** Since there are only finitely many propositional valuations, by the **pigeonhole principle**, there must be some finite $k$ after which the construction generates no further non-empty sets: $S_{k+1} = S_{k+2} = \cdots = \varnothing$. Take $\vec{S} = \langle S_1, \ldots, S_k \rangle$.

**Key structural properties used in the proof:**

- $S_j \subseteq \text{mod}(\Theta_j)$ (follows from the construction)
- $S_i \cap S_j = \varnothing$ for $i < j$ (the layers are disjoint)
- The set $\{\text{mod}(F) : \Theta_i \mid\!\!\sim F\}$ is finite and **closed under intersections** (by (R$\wedge$)), so its common intersection $S_i$ is itself a member of the family — achieved by the formula $\Psi_i$ where $\text{mod}(\Psi_i) = S_i$, giving: $\Theta_{i-1} \mid\!\!\sim \Psi_i$

**Correctness ($F \mid\!\!\sim G \Leftrightarrow F \approxeq_{\vec{S}} G$):**

$(\Rightarrow)$ Suppose $F \mid\!\!\sim G$. Find minimal $j$ with $\text{mod}(F) \cap S_j \neq \varnothing$. Since $j$ is minimal, $\text{mod}(F) \subseteq \text{mod}(\Theta_{j-1})$, i.e. $F \models \Theta_{j-1}$. Using (SC), (CMON), and (CON) one shows $S_j \subseteq \text{mod}(F \to G)$, and then:

$$\text{mod}(F) \cap S_j \subseteq \text{mod}(F) \cap \text{mod}(F \to G) = \text{mod}(F \wedge (F \to G)) \subseteq \text{mod}(G)$$

Hence $F \approxeq_{\vec{S}} G$.

$(\Leftarrow)$ Suppose $F \approxeq_{\vec{S}} G$. Two cases:

- **Case 1** ($\text{mod}(F) \cap S_i = \varnothing$ for all $i$): Then $\text{mod}(F) \subseteq \overline{S_1 \cup \cdots \cup S_k} = \text{mod}(\Theta_k)$, so $F \equiv F \wedge \Theta_k$. Since $S_{k+1} = \varnothing$, we have $\Theta_k \mid\!\!\sim \bot$. Using (RWE), (CMON), and (LLE): $F \mid\!\!\sim G$.

- **Case 2** ($\text{mod}(F) \cap S_j \neq \varnothing$ for minimal $j$): $S_j \not\subseteq \text{mod}(\neg F)$, so $\Theta_{j-1} \not\mid\!\!\sim \neg F$. Also $F \equiv F \wedge \Theta_{j-1}$. Since $F \approxeq_{\vec{S}} G$, $\text{mod}(F) \cap S_j \subseteq \text{mod}(G)$, giving $F \wedge \Psi_j \models G$, i.e. $\Psi_j \models F \to G$. Combining with $\Theta_{j-1} \mid\!\!\sim \Psi_j$ via (RWE), (RMON), and (LLE): $F \mid\!\!\sim (F \to G)$ and hence $F \mid\!\!\sim G$.

### Derived Rule: Negation Rationality (NRAT)

By the Representation Theorem, the following rule holds for all rational consequence relations:

$$\frac{(F \wedge G) \not\mid\!\!\sim H \quad (F \wedge \neg G) \not\mid\!\!\sim H}{F \not\mid\!\!\sim H} \quad \textbf{(NRAT)}$$

---

## SGT Week 4 — Worked Examples

### Question 4.1

Consider $\vec{S} = (S_1, S_2, S_3)$ where:

$$S_1 = \{(p \wedge \neg q \wedge r),\ (p \wedge q \wedge \neg r)\}$$
$$S_2 = \{(p \wedge q \wedge r)\}$$
$$S_3 = \{(\neg p \wedge q \wedge r),\ (\neg p \wedge \neg q \wedge r)\}$$

Which of the following are true?

#### (i) $p \wedge \neg r \approxeq_{\vec{S}} q$ — **TRUE**

$S_1$ is minimal with $\text{mod}(p \wedge \neg r) \cap S_1 \neq \varnothing$.

$$\text{mod}(p \wedge \neg r) \cap S_1 = \{(p \wedge q \wedge \neg r)\} \subseteq \text{mod}(q). \quad \checkmark$$

#### (ii) $\neg p \approxeq_{\vec{S}} (r \to p)$ — **FALSE**

$S_3$ is minimal with $\text{mod}(\neg p) \cap S_3 \neq \varnothing$.

$$\text{mod}(\neg p) \cap S_3 = \{(\neg p \wedge q \wedge r),\ (\neg p \wedge \neg q \wedge r)\}$$

But $\text{mod}(r \to p) = \text{mod}(\neg r) \cup \text{mod}(p)$, and neither valuation in the set satisfies $p$ or $\neg r$. So $\text{mod}(\neg p) \cap S_3 \not\subseteq \text{mod}(r \to p)$. $\times$

#### (iii) $(\neg p \wedge \neg r) \approxeq_{\vec{S}} \neg q$ — **TRUE**

$\text{mod}(\neg p \wedge \neg r) \cap S_i = \varnothing$ for all $i = 1, 2, 3$ (no valuation with $\neg p \wedge \neg r$ appears in any layer). The vacuous case applies. $\checkmark$

#### (iv) $(q \wedge r) \approxeq_{\vec{S}} \neg p$ — **FALSE**

$S_2$ is minimal with $\text{mod}(q \wedge r) \cap S_2 \neq \varnothing$.

$$\text{mod}(q \wedge r) \cap S_2 = \{(p \wedge q \wedge r)\} \not\subseteq \text{mod}(\neg p). \quad \times$$

---

### Question 4.2

**Premises** (using propositional variables: $c$ = core, $ls$ = less specialised, $mr$ = mathematically rigorous, $l$ = logic module, $o$ = optional):

| Premise | Natural language | Formal |
|---------|-----------------|--------|
| P1 | Core modules are usually less specialised | $c \mid\!\!\sim ls$ |
| P2 | Core modules that are mathematically rigorous are usually more specialised | $(c \wedge mr) \mid\!\!\sim \neg ls$ |
| P3 | Logic modules are usually mathematically rigorous | $l \mid\!\!\sim mr$ |
| P4 | Logic modules are not usually optional | $l \not\mid\!\!\sim \neg c$ (as stated; note this encodes optionality via $c$) |

**Claim:** The following conclusions **cannot** be derived from the premises:
- **Conclusion 1:** $mr \mid\!\!\sim \neg c$ (mathematically rigorous modules are usually optional)
- **Conclusion 2:** $mr \mid\!\!\sim c$ (mathematically rigorous modules are usually core)

**Solution:** By the Representation Theorem, it suffices to construct a single ranked preferential model $\vec{S} = \langle S_1, S_2 \rangle$ satisfying all premises but failing both conclusions.

Take:

$$S_1 = \{(c \wedge ls \wedge \neg mr \wedge \neg l)\}$$
$$S_2 = \{(c \wedge \neg ls \wedge mr \wedge l),\ (\neg c \wedge \neg ls \wedge mr \wedge l)\}$$

**Verifying premises:**

- **P1: $c \mid\!\!\sim ls$** — $S_1$ is minimal with $\text{mod}(c) \cap S_1 \neq \varnothing$. $\text{mod}(c) \cap S_1 = \{(c \wedge ls \wedge \neg mr \wedge \neg l)\} \subseteq \text{mod}(ls)$. $\checkmark$

- **P2: $(c \wedge mr) \mid\!\!\sim \neg ls$** — $\text{mod}(c \wedge mr) \cap S_1 = \varnothing$. $S_2$ is minimal. $\text{mod}(c \wedge mr) \cap S_2 = \{(c \wedge \neg ls \wedge mr \wedge l)\} \subseteq \text{mod}(\neg ls)$. $\checkmark$

- **P3: $l \mid\!\!\sim mr$** — $\text{mod}(l) \cap S_1 = \varnothing$. $S_2$ is minimal. $\text{mod}(l) \cap S_2 = \{(c \wedge \neg ls \wedge mr \wedge l), (\neg c \wedge \neg ls \wedge mr \wedge l)\} \subseteq \text{mod}(mr)$. $\checkmark$

- **P4: $l \not\mid\!\!\sim \neg c$** — $\text{mod}(l) \cap S_2 \not\subseteq \text{mod}(\neg c)$ since $(c \wedge \neg ls \wedge mr \wedge l) \in \text{mod}(c)$. $\checkmark$

**Verifying the conclusions fail:**

- **Conclusion 1 fails:** $mr \not\mid\!\!\sim \neg c$ — $S_2$ is minimal for $mr$. $\text{mod}(mr) \cap S_2 = \{(c \wedge \neg ls \wedge mr \wedge l), (\neg c \wedge \neg ls \wedge mr \wedge l)\} \not\subseteq \text{mod}(\neg c)$.

- **Conclusion 2 fails:** $mr \not\mid\!\!\sim c$ — same layer; $\text{mod}(mr) \cap S_2 \not\subseteq \text{mod}(c)$ since $(\neg c \wedge \neg ls \wedge mr \wedge l) \in \text{mod}(\neg c)$.

By the Representation Theorem, every consequence relation generated by a ranked preferential model is rational. Hence if either conclusion were derivable from the premises via the GM rules, it would hold in this model — but it doesn't. $\square$

---

### Question 4.3: Classical Consequence $\models$ is Rational

**Claim:** The classical consequence relation $\models$ is also a rational consequence relation.

**Construction:** Take $\vec{S} = (S_1)$ where $S_1$ is the set of **all possible propositional valuations**. Then $\text{mod}(F) \cap S_1 = \text{mod}(F)$ for any $F$.

**Proof ($F \models G \Leftrightarrow F \approxeq_{\vec{S}} G$):**

$(\Rightarrow)$ Suppose $F \models G$. If $\text{mod}(F) = \varnothing$ then $\text{mod}(F) \cap S_i = \varnothing$ for all $S_i$, so $F \approxeq_{\vec{S}} G$. Otherwise, $\text{mod}(F) \cap S_1 = \text{mod}(F) \subseteq \text{mod}(G)$, so $F \approxeq_{\vec{S}} G$.

$(\Leftarrow)$ Suppose $F \approxeq_{\vec{S}} G$. If $\text{mod}(F) \cap S_1 = \varnothing$ then $\text{mod}(F) = \varnothing \subseteq \text{mod}(G)$, so $F \models G$. Otherwise $\text{mod}(F) \cap S_1 = \text{mod}(F) \subseteq \text{mod}(G)$, so $F \models G$. $\square$

---

### Question 4.4: Proving Derived Rules via the Representation Theorem

#### (i) Supra-classicality (SC): $\dfrac{F \models G}{F \mid\!\!\sim G}$

Let $\vec{S} = \langle S_1, \ldots, S_k \rangle$ be arbitrary. Suppose $F \models G$, i.e. $\text{mod}(F) \subseteq \text{mod}(G)$.

If $\text{mod}(F) \cap S_i = \varnothing$ for all $i$, done. Otherwise let $j$ be minimal with $\text{mod}(F) \cap S_j \neq \varnothing$:

$$\text{mod}(F) \cap S_j \subseteq \text{mod}(F) \subseteq \text{mod}(G)$$

Hence $F \approxeq_{\vec{S}} G$. Since $\vec{S}$ is arbitrary and every rational consequence relation arises from some $\vec{S}$, we have $F \mid\!\!\sim G$. $\square$

#### (ii) Conditionalisation (CON): $\dfrac{(F \wedge G) \mid\!\!\sim H}{F \mid\!\!\sim (G \to H)}$

Suppose $(F \wedge G) \mid\!\!\sim H$, i.e. $(F \wedge G) \approxeq_{\vec{S}} H$ for an arbitrary ranked model $\vec{S}$.

Let $j$ be minimal with $\text{mod}(F) \cap S_j \neq \varnothing$. Two subcases:

- If $\text{mod}(F \wedge G) \cap S_j \neq \varnothing$: by hypothesis, $\text{mod}(F \wedge G) \cap S_j \subseteq \text{mod}(H) \subseteq \text{mod}(G \to H)$.

- If $\text{mod}(F \wedge G) \cap S_j = \varnothing$: then $\text{mod}(F) \cap \text{mod}(G) \cap S_j = \varnothing$, so $\text{mod}(F) \cap S_j \subseteq \overline{\text{mod}(G)} = \text{mod}(\neg G) \subseteq \text{mod}(G \to H)$.

In both cases, $\text{mod}(F) \cap S_j \subseteq \text{mod}(G \to H)$, giving $F \approxeq_{\vec{S}} (G \to H)$. $\square$

#### (iii) Cautious Cut (CC): $\dfrac{F \mid\!\!\sim G \quad (F \wedge G) \mid\!\!\sim H}{F \mid\!\!\sim H}$

Suppose $F \approxeq_{\vec{S}} G$ and $(F \wedge G) \approxeq_{\vec{S}} H$.

Let $j$ be minimal with $\text{mod}(F) \cap S_j \neq \varnothing$.

Since $\text{mod}(F \wedge G) \cap S_i \subseteq \text{mod}(F) \cap S_i = \varnothing$ for all $i < j$, it follows that $j$ is also minimal with $\text{mod}(F \wedge G) \cap S_j \neq \varnothing$.

From $F \approxeq_{\vec{S}} G$: $\text{mod}(F) \cap S_j \subseteq \text{mod}(G)$.

Therefore:

$$\text{mod}(F) \cap S_j \subseteq \text{mod}(F) \cap \text{mod}(G) \cap S_j = \text{mod}(F \wedge G) \cap S_j \subseteq \text{mod}(H)$$

Hence $F \approxeq_{\vec{S}} H$. $\square$

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Ranked preferential model** | An ordered sequence $\vec{S} = \langle S_1, \ldots, S_k \rangle$ where each $S_i$ is a set of propositional valuations; $S_1$ contains the most usual worlds, $S_k$ the most exceptional |
| **$\approxeq_{\vec{S}}$** | The consequence relation generated by $\vec{S}$: $F \approxeq_{\vec{S}} G$ iff the most usual $F$-worlds (the minimal layer intersecting $\text{mod}(F)$) are all $G$-worlds; vacuously true if no layer contains an $F$-world |
| **$\text{mod}(F)$** | The set of all propositional valuations satisfying formula $F$ |
| **Rational consequence relation** | A consequence relation satisfying all six Gabbay–Makinson rules: REF, R$\wedge$, L$\vee$, LLE, RWE, CMON, RMON |
| **Supra-classicality (SC)** | If $F \models G$ classically, then $F \mid\!\!\sim G$; rational consequence extends classical consequence |
| **Conditionalisation (CON)** | If $(F \wedge G) \mid\!\!\sim H$ then $F \mid\!\!\sim (G \to H)$ |
| **Cautious Cut (CC)** | If $F \mid\!\!\sim G$ and $(F \wedge G) \mid\!\!\sim H$ then $F \mid\!\!\sim H$ |
| **Negation Rationality (NRAT)** | If $(F \wedge G) \not\mid\!\!\sim H$ and $(F \wedge \neg G) \not\mid\!\!\sim H$, then $F \not\mid\!\!\sim H$ |
| **Preferential model** | Like a ranked model but with a partial order instead of total linear order; satisfies all GM rules except RMON |
| **Representation Theorem** | Every rational consequence relation $\mid\!\!\sim$ is generated by some ranked preferential model $\vec{S}$, i.e. $\mid\!\!\sim \;= \approxeq_{\vec{S}}$. Combined with Theorem 2.3: rational $\Leftrightarrow$ rankedly representable |
| **Counter-model method** | To show a conclusion $F \mid\!\!\sim G$ cannot be derived from premises using GM rules: construct a ranked model satisfying the premises but falsifying $F \approxeq_{\vec{S}} G$ |

---

## Summary

This week established the **semantic foundation** of rational consequence:

- **LO1 (Define ranked preferential models):** A ranked model $\vec{S} = \langle S_1, \ldots, S_k \rangle$ assigns propositional worlds to ordered layers of typicality.

- **LO2 (Evaluate $\approxeq_{\vec{S}}$):** $F \approxeq_{\vec{S}} G$ holds iff the first (most typical) layer containing any $F$-world has all its $F$-worlds also satisfying $G$, or there are no $F$-worlds at all (vacuous truth). The SGT 4.1 exercise demonstrated all four cases including vacuous truth (iii) and how a single world in the minimal layer can falsify the relation (ii, iv).

- **LO3 (Prove Theorem 2.3):** Every $\approxeq_{\vec{S}}$ is rational. The proof verified all six GM rules by set-theoretic arguments restricted to the minimal layer $S_j$.

- **LO4 (Apply Representation Theorem):** Theorem 2.4 is the converse: every rational relation comes from some ranked model. This gives a powerful proof tool — to prove a rule holds for **all** rational relations, prove it holds for **all** ranked models. To show a conclusion is **not** provable, construct a ranked counter-model.

- **LO5–LO7 (Derived rules and counter-models):** SC, CON, and CC were proved via Theorem 2.4. The module-premises problem (4.2) demonstrated the counter-model technique. Classical consequence $\models$ was shown to be rational (4.3) via the single-layer model with all valuations.
