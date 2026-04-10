# Week 2: Non-monotonic Reasoning, Argumentation and Dialogue

## Learning Objectives

By the end of this week, you should be able to:

- [ ] **LO1**: Evaluate the non-monotonic consequences for three types of non-monotonic logic (logic programming, maxiconsistent approaches, default logic).
- [ ] **LO2**: Construct an argument framework (AF) of arguments and their relations from a knowledge base (KB) of formulas and the inference rules of a logic.
- [ ] **LO3**: Apply preferences to determine whether one argument is a legitimate counter-argument to another.
- [ ] **LO4**: Apply labelling rules to determine the winning sets of arguments in an AF.
- [ ] **LO5**: Relate the claims of winning arguments to a KB's non-monotonic logic consequences.

---

## Overview and Context

Week 2 is delivered by Dr Sanjay Modgil (King's College London). The week covers **syntactic** presentations of non-monotonic logic, complementing the *semantic* presentation given in Week 1 by Chris.

Three specific non-monotonic logics are covered:

1. **Logic Programming** (with negation-as-failure)
2. **Maxiconsistent Approaches** (stratified / preferred maximal consistent subsets)
3. **Default Logic** (Reiter-style default theories with priority orderings)

The second half of the week formalises non-monotonic reasoning as the **exchange of arguments**: constructing arguments from a KB, relating them by attack, applying preferences to filter attacks, and evaluating winning arguments via labelling rules.

The key insight (from Dung 1995) is:

> "One can think of non-monotonic reasoning as a decision procedure for choosing between contradictory classical (monotonic) consequences" — and, generalising: "as a decision procedure for choosing between **arguments for conflicting claims**."

---

## Section 1: Logic Programming as Non-monotonic Reasoning (LO1)

### 1.1 Background

Logic programs (as in Prolog) do **not** use strict classical negation $(\neg)$. Instead they use **negation as failure** written $\mathit{NOT}$, meaning "cannot be proved."

A logic programming rule has the form:

$$h(X) \mathrel{:-} b_1(X),\, b_2(X),\, \mathit{NOT}\; c(X)$$

meaning: conclude $h(X)$ if $b_1(X)$ and $b_2(X)$ hold and $c(X)$ cannot be proved.

### 1.2 Non-monotonic Consequence Relation

Given a Prolog program $\mathit{KB}$, we write:

$$\mathit{KB} \mathrel{\vert\!\!\sim} Q$$

if and only if the query $?Q$ returns **true** for some instantiation of variables in $Q$.

### 1.3 Worked Example: The Tweety Trilogy

**KB$_1$** (three rules):

| # | Rule |
|---|------|
| 1 | `penguin(tweety)` |
| 2 | `bird(X) :- penguin(X)` |
| 3 | `fly(X) :- bird(X), NOT abnormal_fly(X)` |

From KB$_1$: since `abnormal_fly(tweety)` cannot be proved, rule 3 fires and:

$$\mathit{KB}_1 \mathrel{\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$$

**KB$_2$ = KB$_1$ $\cup$ \{4\}** — add:

| # | Rule |
|---|------|
| 4 | `abnormal_fly(X) :- penguin(X), NOT DC_comic(X)` |

Now `DC_comic(tweety)` cannot be proved, so rule 4 fires and we derive `abnormal_fly(tweety)`. Rule 3 can **no longer** fire (its `NOT abnormal_fly` condition fails):

$$\mathit{KB}_2 \mathrel{\not\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$$

This demonstrates **non-monotonicity**: adding a formula to the KB invalidates a previously derivable consequence.

**KB$_3$ = KB$_2$ $\cup$ \{5\}** — add:

| # | Rule |
|---|------|
| 5 | `DC_comic(tweety)` |

Now `DC_comic(tweety)` is provable, so rule 4 can **no longer** fire. `abnormal_fly(tweety)` is no longer derivable. Rule 3 can fire again:

$$\mathit{KB}_3 \mathrel{\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$$

Non-monotonicity is illustrated three ways: adding rule 4 removes `fly(tweety)`; adding fact 5 reinstates it.

### 1.4 Decision/Preference Example (Cinema vs Restaurant)

**KB** (with beliefs, desires, goals):

| # | Rule |
|---|------|
| 1 | $\mathbf{Bel}\ \mathit{birthday} \wedge \mathbf{Des}\ \mathit{celebrate} \wedge \mathbf{Bel}\ \mathit{NOT}\ \mathit{closed\_cinema} \to \mathbf{Goal}(\mathit{cinema})$ |
| 2 | $\mathbf{Bel}\ \mathit{birthday} \wedge \mathbf{Des}\ \mathit{celebrate} \wedge \mathbf{Bel}\ \mathit{NOT}\ \mathit{closed\_restaurant} \to \mathbf{Goal}(\mathit{restaurant})$ |
| 3 | $\mathbf{Bel}\ \mathit{birthday}$ |
| 4 | $\mathbf{Des}\ \mathit{celebrate\_birthday}$ |

Both `Goal(cinema)` and `Goal(restaurant)` are derivable — a genuine dilemma. Adding the **preference** fact:

| # | Rule |
|---|------|
| 5 | $\mathbf{Goal}(\mathit{restaurant}) > \mathbf{Goal}(\mathit{cinema})$ |

resolves the dilemma: `Goal(restaurant)` becomes the unique non-monotonic consequence.

---

## Section 2: Maxiconsistent Approaches to Non-monotonic Reasoning (LO1)

### 2.1 Motivation

Classical logic is **monotonic**:

$$\mathit{KB} \vdash \alpha \;\longrightarrow\; \mathit{KB} \cup \{\beta\} \vdash \alpha$$

However, when a classical KB is **inconsistent**, every formula follows (ex falso quodlibet). The maxiconsistent approach avoids this by working with *maximal consistent subsets* of the KB, prioritised by a total ordering over formulas.

### 2.2 The Subclass Principle

**KB$_1$** (classical, consistent):

| # | Formula |
|---|---------|
| 1 | $\mathit{penguin}(\mathit{tweety})$ |
| 2 | $\mathit{penguin}(X) \to \mathit{bird}(X)$ |
| 3 | $\mathit{bird}(X) \to \mathit{fly}(X)$ |

$\mathit{KB}_1 \vdash \mathit{fly}(\mathit{tweety})$ — one preferred mcs $E = \{1, 2, 3\}$, and $\mathit{KB}_1 \mathrel{\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$.

**KB$_2$ = KB$_1$ $\cup$ \{4\}**, adding:

| # | Formula |
|---|---------|
| 4 | $\mathit{penguin}(X) \to \neg\mathit{fly}(X)$ |

Now KB$_2$ classically entails both $\mathit{fly}(\mathit{tweety})$ (via 1,2,3) and $\neg\mathit{fly}(\mathit{tweety})$ (via 1,4) — a contradiction. The **subclass principle** justifies preferring rule 4 over rule 3: rules expressing properties of a *subclass* (penguins) take priority over rules expressing properties of the *superclass* (birds).

### 2.3 Total Ordering and Stratification

We assume a **total ordering** $\geq$ over the formulas of KB such that:

$$x \geq y \text{ and } y \not\geq x \;\Rightarrow\; x > y \quad \text{(strictly greater)}$$
$$x \geq y \text{ and } y \geq x \;\Rightarrow\; x \approx y \quad \text{(same equivalence class)}$$

The KB is represented as a **stratified knowledge base** $S_1, S_2, \ldots, S_n$ where:

- $\alpha, \beta \in S_i \iff \alpha \approx \beta$ (same equivalence class)
- $\alpha \in S_i,\; \beta \in S_j,\; i < j \;\Rightarrow\; \alpha > \beta$ (higher stratum = higher priority)

For KB$_2$ above, using the subclass principle:

$$S_1: \quad \mathit{penguin}(\mathit{tweety}),\quad \mathit{penguin}(X)\to\mathit{bird}(X)$$
$$S_2: \quad \mathit{penguin}(X)\to\neg\mathit{fly}(X)$$
$$S_3: \quad \mathit{bird}(X)\to\mathit{fly}(X)$$

Priority ordering: $1 \approx 2$ (both in $S_1$), $1 > 3,4$, $2 > 3,4$, $4 > 3$.

**Other criteria for ordering** include: temporal ordering (more recently acquired information has higher priority).

### 2.4 Preferred Maximal Consistent Subsets (mcs)

**Procedure**: Given stratified KB $S_1 \ldots S_n$:

1. Find all **maximal consistent subsets** $E_1, \ldots, E_m$ of $S_1$.
2. For each $E_i$, **maximally consistently extend** with formulas from $S_2$.
3. Continue down through $S_3, \ldots, S_n$.

**Formal Definition**: A **preferred maximal consistent subset** (preferred mcs) is a set $E = E_1 \cup \cdots \cup E_n$ such that for $i = 1, \ldots, n$:

$$E_1 \cup \cdots \cup E_i \text{ is a maximal (under set inclusion) consistent subset of } S_1 \cup \cdots \cup S_i$$

#### Full Worked Example

$$S_1: \;a,\quad a\to b,\quad \neg b$$
$$S_2: \;c,\quad \neg c$$
$$S_3: \;d$$

**Step 1 — mcs of $S_1$**:

| Subset | Consistent? | Maximal? |
|--------|-------------|----------|
| $\{a, a\to b\}$ | Yes | Yes — cannot add $\neg b$ |
| $\{a, \neg b\}$ | Yes | Yes — cannot add $a\to b$ |
| $\{a\to b, \neg b\}$ | Yes | Yes — cannot add $a$ |

So $E_1 = \{a, a\to b\}$, $E_2 = \{a, \neg b\}$, $E_3 = \{a\to b, \neg b\}$.

**Step 2 — extend each with $S_2 = \{c, \neg c\}$**:

- $E_1 \to E_{1.1} = \{a, a\to b, c\}$; $E_{1.2} = \{a, a\to b, \neg c\}$
- $E_2 \to E_{2.1} = \{a, \neg b, c\}$; $E_{2.2} = \{a, \neg b, \neg c\}$
- $E_3 \to E_{3.1} = \{a\to b, \neg b, c\}$; $E_{3.2} = \{a\to b, \neg b, \neg c\}$

**Step 3 — extend each with $S_3 = \{d\}$** (consistent with all):

$$E_{1.1.3} = \{a, a\to b, c, d\},\quad E_{1.2.3} = \{a, a\to b, \neg c, d\}$$
$$E_{2.1.3} = \{a, \neg b, c, d\},\quad E_{2.2.3} = \{a, \neg b, \neg c, d\}$$
$$E_{3.1.3} = \{a\to b, \neg b, c, d\},\quad E_{3.2.3} = \{a\to b, \neg b, \neg c, d\}$$

These six sets are the preferred mcs of KB.

#### Penguin Example with Stratification

For KB$_2$ (stratified as $S_1, S_2, S_3$ above): starting at $S_1$ (consistent), add $S_2$ rule $\mathit{penguin}(X)\to\neg\mathit{fly}(X)$ — consistent. Cannot then add $S_3$ rule $\mathit{bird}(X)\to\mathit{fly}(X)$ (inconsistency). **Single preferred mcs**:

$$E = \{\mathit{penguin}(\mathit{tweety}),\; \mathit{penguin}(X)\to\mathit{bird}(X),\; \mathit{penguin}(X)\to\neg\mathit{fly}(X)\}$$

From $E$: $\mathit{KB}_2 \mathrel{\not\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$ and $\mathit{KB}_2 \mathrel{\vert\!\!\sim} \neg\mathit{fly}(\mathit{tweety})$.

### 2.5 Credulous and Sceptical Consequence Relations

When there is more than one preferred mcs, two consequence relations are defined:

**Credulous consequence** (at-least-one):

$$\mathit{KB} \mathrel{\vert\!\!\sim_{\mathit{cred}}} \alpha \quad\iff\quad \exists\, E \in \mathit{mcs}(\mathit{KB}) \text{ such that } E \vdash \alpha$$

**Sceptical consequence** (all — assumed by Kraus, Lehmann, Magidor from Week 1):

$$\mathit{KB} \mathrel{\vert\!\!\sim_{\mathit{scept}}} \alpha \quad\iff\quad \forall\, E \in \mathit{mcs}(\mathit{KB}): E \vdash \alpha$$

**Key property**: No individual preferred mcs is inconsistent, so $\bot$ is **not** a credulous consequence.

**Example** (from the 6-subset example above):
- $d$ is in **every** preferred mcs $\Rightarrow$ $d$ is a **sceptical** consequence.
- $a$, $\neg b$, $c$, $\neg c$ are credulous (in at least one) but **not** sceptical.
- $\bot$ is **not** credulous.

---

## Section 3: Default Logic Approaches to Non-monotonic Reasoning (LO1)

### 3.1 Structure of a Default Theory

A **default theory** $(W, \mathit{Defaults})$ consists of:

- $W$ — a **consistent** set of classical well-formed formulas (factual world description).
- $\mathit{Defaults} = \{d_1, \ldots, d_n\}$ — a set of **default rules** of the form:

$$d = \alpha_1,\ldots,\alpha_n \Rightarrow \beta$$

where $\mathit{antecedents}(d) = \{\alpha_1, \ldots, \alpha_n\}$ and $\mathit{consequent}(d) = \beta$.

A default $\alpha_1,\ldots,\alpha_n \Rightarrow \beta$ expresses: "if $\alpha_1$ and $\ldots$ and $\alpha_n$, then **usually / typically / under normal circumstances** $\beta$."

A default with antecedent $\top$ (truth) is **always applicable** (requires no prerequisite).

**Standard examples**:

$$\mathit{bird}(X) \Rightarrow \mathit{fly}(X) \qquad \mathit{penguin}(X) \Rightarrow \neg\mathit{fly}(X) \qquad \top \Rightarrow \mathit{wings}(X)$$

**World** $W$:

$$W = \{\mathit{penguin}(\mathit{tweety}),\quad \mathit{penguin}(X)\to\mathit{bird}(X)\}$$

### 3.2 Default Extensions — Formal Definition

A **default extension** $E$ of $(W, \mathit{Defaults})$ satisfies:

$$\textbf{1.}\quad E = Cn\!\left(W \cup \bigcup_{d_i \in \mathit{Defaults},\; \mathit{antecedents}(d_i)\,\subseteq\, E} \mathit{consequent}(d_i)\right)$$

$$\textbf{2.}\quad E \not\vdash \bot \quad \text{(E is consistent)}$$

$$\textbf{3.}\quad E \text{ is maximal (under set inclusion) satisfying 1 and 2}$$

### 3.3 Constructing Extensions — Procedure

1. Start with $Cn(W)$.
2. Choose a default $d_i$ whose antecedents are in $Cn(W)$; add $\mathit{consequent}(d_i)$ **if consistent**.
3. Obtain $Cn(W \cup \{\beta\})$.
4. Choose another applicable default $d_j$, add its consequent $\delta$ if consistent.
5. Continue until no more defaults can be applied consistently.

**Different choices of defaults yield different extensions.**

### 3.4 Worked Example: Tweety (No Priority)

$$W = \{\mathit{penguin}(\mathit{tweety}),\; \mathit{penguin}(X)\to\mathit{bird}(X)\}$$

Defaults: $D_1 = \mathit{bird}(X)\Rightarrow\mathit{fly}(X)$, $D_2 = \mathit{penguin}(X)\Rightarrow\neg\mathit{fly}(X)$, $D_3 = \top\Rightarrow\mathit{wings}(X)$

$Cn(W)$ contains $\mathit{penguin}(\mathit{tweety})$ and $\mathit{bird}(\mathit{tweety})$.

**Extension $E_1$** (apply $D_1$ then $D_3$):
- $D_1$ antecedent $\mathit{bird}(\mathit{tweety}) \in Cn(W)$: add $\mathit{fly}(\mathit{tweety})$ — consistent. ✓
- $D_3$ antecedent $\top$: add $\mathit{wings}(\mathit{tweety})$ — consistent. ✓
- Try $D_2$: antecedent $\mathit{penguin}(\mathit{tweety}) \in E_1$ ✓, but $\neg\mathit{fly}(\mathit{tweety})$ would be **inconsistent** with $\mathit{fly}(\mathit{tweety})$ already in $E_1$. ✗

$$E_1 = Cn(W \cup \{\mathit{wings}(\mathit{tweety}),\, \mathit{fly}(\mathit{tweety})\})$$

**Extension $E_2$** (apply $D_2$ then $D_3$):
- $D_2$ antecedent $\mathit{penguin}(\mathit{tweety}) \in Cn(W)$: add $\neg\mathit{fly}(\mathit{tweety})$ — consistent. ✓
- $D_3$: add $\mathit{wings}(\mathit{tweety})$ — consistent. ✓
- Try $D_1$: would add $\mathit{fly}(\mathit{tweety})$, inconsistent with $\neg\mathit{fly}(\mathit{tweety})$. ✗

$$E_2 = Cn(W \cup \{\mathit{wings}(\mathit{tweety}),\, \neg\mathit{fly}(\mathit{tweety})\})$$

**Consequences**:
- $\mathit{wings}(\mathit{tweety})$ is in **both** extensions $\Rightarrow$ **sceptical** consequence.
- $\mathit{fly}(\mathit{tweety})$ and $\neg\mathit{fly}(\mathit{tweety})$ are each in one extension only $\Rightarrow$ **credulous** but **not sceptical**.

### 3.5 Priority Ordering Over Defaults

When defaults have a **linear priority ordering** $d_i > d_j$ (default $d_i$ has higher priority than $d_j$):

> When choosing which default to apply next, **start with the highest-priority default** whose antecedents hold. Once applied, lower-priority conflicting defaults are blocked.

**Tweety example with ordering $D_2 > D_1$** (penguin rule beats bird rule):

1. Start with $Cn(W)$.
2. Check $D_2$ (highest relevant priority): antecedent $\mathit{penguin}(\mathit{tweety})$ holds. Add $\neg\mathit{fly}(\mathit{tweety})$. ✓
3. $D_3$: add $\mathit{wings}(\mathit{tweety})$. ✓
4. $D_1$: would add $\mathit{fly}(\mathit{tweety})$ — inconsistent. ✗

**Single extension**:

$$E_2 = Cn(W \cup \{\mathit{wings}(\mathit{tweety}),\, \neg\mathit{fly}(\mathit{tweety})\})$$

Now $\neg\mathit{fly}(\mathit{tweety})$ is **both** a sceptical and credulous consequence; $\mathit{fly}(\mathit{tweety})$ is **neither**.

### 3.6 Second Worked Example ($W = \{a, a\to b, c\}$)

$$W = \{a,\; a\to b,\; c\}$$

$$\text{Defaults:}\quad 1.\;\top\Rightarrow\neg c,\quad 2.\;b\Rightarrow d,\quad 3.\;d\Rightarrow\neg a,\quad 4.\;a\Rightarrow\neg d$$

$$\text{Ordering:}\quad 1 > 2 > 3 > 4$$

Applying in priority order:
- **Default 1**: antecedent $\top$ holds, but consequent $\neg c$ is **inconsistent** with $c \in W$. **Skip**.
- **Default 2**: antecedent $b$: since $a, a\to b \in W$, we have $b \in Cn(W)$. Add $d$. ✓ — obtain $Cn(W \cup \{d\})$.
- **Default 3**: antecedent $d \in Cn(W\cup\{d\})$. Would add $\neg a$; but $a \in Cn(W\cup\{d\})$ — **inconsistent**. **Skip**.
- **Default 4**: antecedent $a \in Cn(W\cup\{d\})$. Would add $\neg d$; but $d$ already in extension — **inconsistent**. **Skip**.

**Single extension**: $Cn(W \cup \{d\})$.

### 3.7 SGT Worked Example

$$W = \{\neg a\lor\neg b\lor\neg c,\; d\to\neg c\}$$

$$\text{Defaults:}\quad 1.\;\top\Rightarrow a,\quad 2.\;a\Rightarrow d,\quad 3.\;\top\Rightarrow c,\quad 4.\;d\Rightarrow b$$

**Without ordering** — two extensions:

- **Extension 1**: Apply $1 \Rightarrow a$; then $2 \Rightarrow d$ (antecedent $a$ holds); then $4 \Rightarrow b$ (antecedent $d$ holds). Can we apply $3 \Rightarrow c$? $\neg a\lor\neg b\lor\neg c$ together with $a,b$ forces $\neg c$, so $c$ would be inconsistent. **Two extensions**:

$$E_1 = Cn(W \cup \{a, d, b\}), \qquad E_2 = Cn(W \cup \{a, c\})$$

**With ordering $1 > 2 > 3 > 4$** — one extension only:

Apply highest-priority applicable defaults first: $1\Rightarrow a$, then $2\Rightarrow d$, then $4\Rightarrow b$, default 3 blocked (inconsistency). **One extension**: $Cn(W \cup \{a, d, b\})$.

---

## Section 4: Non-monotonic Reasoning as Argumentation (LO2)

### 4.1 The Core Idea

Non-monotonic reasoning can be viewed as a **decision procedure for choosing between arguments for conflicting claims**. Arguments are built from the KB (using premises and rules) and related by **attacks**. Evaluating which arguments are **winning** yields the non-monotonic consequences.

This is more intuitive than proof-theoretic accounts because it mirrors everyday debate and discussion.

### 4.2 Argumentation Framework (Dung 1995)

**Definition** (Dung, *Artificial Intelligence* 77, 1995): An **argumentation framework** is a pair:

$$AF = \langle AR, \mathit{attacks}\rangle$$

where $AR$ is a set of arguments and $\mathit{attacks} \subseteq AR \times AR$ is a binary relation. $\mathit{attacks}(A, B)$ means $A$ is a counter-argument to $B$ (attacks $B$).

Graphically: arguments are **nodes** in a directed graph; attacks are **directed edges**.

### 4.3 Constructing Arguments from Logic Programs

An **argument** from a logic programming KB is a chain of facts and rules:

$$[f_1, r_1, f_2, r_2, \ldots] : \mathit{claim}$$

**Attack rule for logic programs**: Argument $A$ **attacks** argument $B$ if $A$'s claim $X$ challenges an assumption in $B$ of the form $\mathit{NOT}\;X$ (i.e., $A$ proves what $B$ assumed was not provable).

#### Full Worked Example (Tweety trilogy, argumentative)

**KB$_1$** (penguin, bird, fly-if-not-abnormal):

- **Argument A1** = $[1, 2, 3]$: uses rules 1, 2, 3 to derive $\mathit{fly}(\mathit{tweety})$.
- A1 is the **only** argument, hence **winning** (unattacked).
- $\mathit{KB}_1 \mathrel{\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$ ✓

**KB$_2$** (add rule 4: `abnormal_fly(X) :- penguin(X), NOT DC_comic(X)`):

- **Argument A2** = $[1, 4]$: uses facts 1 and rule 4 to claim $\mathit{abnormal\_fly}(\mathit{tweety})$.
- A2 **attacks** A1: A2's claim $\mathit{abnormal\_fly}(\mathit{tweety})$ challenges A1's assumption $\mathit{NOT}\;\mathit{abnormal\_fly}(\mathit{tweety})$.
- A2 is **unattacked** $\Rightarrow$ A2 is **winning**.
- A1 is attacked by a winning argument $\Rightarrow$ A1 is **losing**.
- No winning argument claims $\mathit{fly}(\mathit{tweety})$, so $\mathit{KB}_2 \mathrel{\not\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$ ✓

**KB$_3$** (add fact 5: `DC_comic(tweety)`):

- **Argument A3** = $[5]$: claims $\mathit{DC\_comic}(\mathit{tweety})$.
- A3 **attacks** A2: A2 assumed $\mathit{NOT}\;\mathit{DC\_comic}(\mathit{tweety})$, but A3 proves it.
- A3 is **unattacked** $\Rightarrow$ A3 is **winning**.
- A2 is attacked by winning A3 $\Rightarrow$ A2 is **losing**.
- All of A1's attackers (only A2) are losing $\Rightarrow$ A1 is **winning**.
- $\mathit{KB}_3 \mathrel{\vert\!\!\sim} \mathit{fly}(\mathit{tweety})$ ✓ — reinstated.

### 4.4 Constructing Arguments from Classical (Stratified) KBs

Arguments from a stratified classical KB are **tuples** $[\Delta, \alpha]$ such that:

1. $\Delta \subseteq \mathit{KB}$, $\Delta \vdash \alpha$
2. No proper subset of $\Delta$ classically entails $\alpha$
3. $\Delta$ is consistent

**Attack rule for classical KBs**: $A = [\Delta_1, \alpha]$ **attacks** $B = [\Delta_2, \beta]$ if the claim $\alpha$ of $A$ **negates a premise** in $\Delta_2$.

**Attack success** (applying preferences): An attack from $A$ to $B$ **succeeds** if the attacked premise (e.g., $a$ in $B$) is **not ordered above** any of the premises in the attacking argument $A$. In other words, the attack from $A$ to $B$ is cancelled if $B$ uses higher-priority premises than $A$.

**Example** (from slides — stratified KB):

$$S_1:\; a\to b,\; a \qquad S_2:\; \neg b,\; c,\; \neg c \qquad S_3:\; d$$

Arguments:

- $A_1 = [\{a\to b, \neg b\},\, \neg a]$
- $A_2 = [\{a\},\, a]$
- $A_3 = [\{c\},\, c]$
- $A_4 = [\{\neg c\},\, \neg c]$

$A_3$ and $A_4$ attack each other (symmetric). $A_1$ does **not** successfully attack $A_2$ because the attacked premise $a$ is in $S_1$ (higher stratum) than the attacking premises $a\to b, \neg b$ in $S_2$.

---

## Section 5: Applying Preferences to Determine Legitimate Counter-Arguments (LO3)

### 5.1 When Preferences Are Needed

Preferences are needed when two arguments attack each other symmetrically (a genuine dilemma) and we need to determine which attack **succeeds**.

**Rule**: If argument $A$ is **preferred** to (stronger than) argument $B$ (i.e., $A > B$), then the attack from $B$ to $A$ is **cancelled** — $B$ does not legitimately challenge $A$. Only the attack from $A$ to $B$ survives.

### 5.2 Sources of Preferences

1. **Goal/decision preferences**: If $\mathbf{Goal}(\mathit{restaurant}) > \mathbf{Goal}(\mathit{cinema})$, then the argument for restaurant is preferred over the argument for cinema.
2. **Priority over defaults**: $d_i > d_j$ means arguments built using $d_i$ defeat arguments built using $d_j$.
3. **Stratification of classical formulas**: Premises in higher strata defeat premises in lower strata.

### 5.3 General Principle (All Three NM Logics)

For any of the three non-monotonic logics:

1. Construct arguments from the KB (using facts and rules/defaults/classical formulas).
2. Identify attacks (an argument attacks another if its claim undermines an assumption or premise).
3. Apply preferences (from orderings over rules/formulas/goals) to decide which attacks succeed.
4. Evaluate the resulting AF to find winning arguments.
5. **Claims of winning arguments = non-monotonic consequences of the KB.**

> Note: The argumentative characterisation of maxi-consistent non-monotonic reasoning is **not examinable** (mentioned in the lecture but not required for the exam).

---

## Section 6: Labelling Argumentation Frameworks (LO4)

### 6.1 The Argumentation Framework

After applying preferences, we obtain a **pure** argumentation framework:

$$AF = \langle \mathit{Args}, \mathit{attacks}\rangle$$

where $\mathit{attacks}$ is now the set of **successful** attacks (a directed graph).

### 6.2 The Labelling Function

A **labelling** is a total function:

$$\mathcal{L} : \mathcal{A} \mapsto \{\mathtt{IN},\, \mathtt{OUT},\, \mathtt{UNDEC}\}$$

- $\mathtt{IN}$ = winning (justified, accepted)
- $\mathtt{OUT}$ = losing (overruled, rejected)
- $\mathtt{UNDEC}$ = undecided

### 6.3 Three Labelling Rules (Legal Labelling)

A labelling $\mathcal{L}$ is **legal** if and only if it satisfies all three rules:

| Rule | Condition | Label |
|------|-----------|-------|
| **Rule 1** | All attackers of $x$ are $\mathtt{OUT}$ | $x$ is $\mathtt{IN}$ |
| **Rule 2** | At least one attacker of $x$ is $\mathtt{IN}$ | $x$ is $\mathtt{OUT}$ |
| **Rule 3** | No attacker of $x$ is $\mathtt{IN}$, AND at least one attacker of $x$ is $\mathtt{UNDEC}$ | $x$ is $\mathtt{UNDEC}$ |

**Special case**: if $x$ has **no attackers**, then Rule 1 applies vacuously — $x$ must be $\mathtt{IN}$.

### 6.4 Worked Example: Five-Argument Framework

Framework: A5 attacks A1; A2 is unattacked; A3 and A4 attack each other.

| Argument | Attackers | Label | Reason |
|----------|-----------|-------|--------|
| A5 | none | **IN** | Rule 1 (vacuously: all zero attackers are OUT) |
| A1 | A5 (IN) | **OUT** | Rule 2: has an IN attacker |
| A2 | none | **IN** | Rule 1 (vacuously) |
| A3 | A4 (UNDEC) | **UNDEC** | Rule 3: no IN attacker, has UNDEC attacker |
| A4 | A3 (UNDEC) | **UNDEC** | Rule 3: no IN attacker, has UNDEC attacker |

### 6.5 Multiple Legal Labellings

The labelling function is **one-to-many**: an AF may have multiple legal labellings. For the symmetric dilemma $x \leftrightarrow y$ (each attacks the other):

| Labelling | $x$ | $y$ | Valid? |
|-----------|-----|-----|--------|
| $L_1$ | UNDEC | UNDEC | Yes — Rule 3 for both |
| $L_2$ | IN | OUT | Yes — Rule 1 for $x$ (attacker $y$ is OUT); Rule 2 for $y$ (attacker $x$ is IN) |
| $L_3$ | OUT | IN | Yes — symmetric to $L_2$ |

$L_2$ and $L_3$ represent hypothetical outcomes; $L_1$ represents the sceptical position.

### 6.6 Preferred Labelling

**Definition**: A labelling $L$ is **preferred** if for all other legal labellings $L^*$, the set of $\mathtt{IN}$-arguments under $L^*$ is **not a strict superset** of the $\mathtt{IN}$-arguments under $L$.

Equivalently: no other legal labelling makes *more* arguments $\mathtt{IN}$.

- Preferred labellings are the **credulous** labellings — they maximise the set of winning arguments.
- A framework may have **multiple** preferred labellings (one for each possible resolution of dilemmas).

### 6.7 Grounded Labelling

**Definition**: The **grounded labelling** $L$ is unique and satisfies: for all other legal labellings $L^*$, the set of $\mathtt{IN}$-arguments under $L^*$ is **not a strict subset** of the $\mathtt{IN}$-arguments under $L$.

Equivalently: no other legal labelling makes *fewer* arguments $\mathtt{IN}$.

- The grounded labelling is the **sceptical** labelling — it makes as few arguments $\mathtt{IN}$ as possible.
- It corresponds to the **grounded extension** (least fixpoint of $F_{AF}$) from Dung 1995.

**Relationship**:

> Preferred labellings $\leftrightarrow$ Credulous semantics (preferred extensions)  
> Grounded labelling $\leftrightarrow$ Sceptical semantics (grounded extension)

### 6.8 Larger Example: V/W/X/Y Framework

Framework: V attacks W and X; W attacks V and X; X attacks Y.

**Legal labellings**:

- $L_1$: V = UNDEC, W = UNDEC, X = UNDEC, Y = UNDEC (all undecided — grounded labelling, since its IN set $\emptyset$ has no strict subset among legal labellings).
- $L_2$: V = **IN**, W = **OUT**, X = **OUT**, Y = **IN** (preferred).
- $L_3$: W = **IN**, V = **OUT**, X = **OUT**, Y = **IN** (preferred).

$L_1$ is the **grounded** labelling (no IN arguments — most sceptical). $L_2$ and $L_3$ are **preferred** labellings (neither's IN-set is a superset of the other's).

### 6.9 Odd Cycle

For a cycle $V \to X \to W \to V$ (odd cycle), the **only** legal labelling makes **all** arguments $\mathtt{UNDEC}$.

### 6.10 The Non-odd Cycle Example (Preferred vs Grounded)

Framework: $Y \to Z$ (Y attacks Z), $Z \to Y$, $V$ attacks $X$, $W$ attacks $X$, $V \leftrightarrow W$ (mutual attack).

- Grounded labelling: all UNDEC.
- $L_1$ (preferred): Y = **IN**, Z = **OUT**, V = UNDEC, W = UNDEC, X = UNDEC.
- $L_2$ (preferred): Z = **IN**, Y = **OUT**, W = **OUT**, V = **IN**, X = **OUT**.

Note: $L_1$ has one IN-argument (Y) and $L_2$ has two (Z, V) — but both are preferred because neither's IN-set is a superset of the other's.

---

## Section 7: Connecting Arguments to Non-monotonic Consequences (LO5)

The central theorem connecting argumentation to non-monotonic logic:

> **The claims of the winning arguments in the AF correspond exactly to the non-monotonic logic consequences that would be derived from the underlying knowledge base.**

| AF concept | NM Logic concept |
|------------|------------------|
| Preferred labelling ($\mathtt{IN}$ arguments) | Credulous consequences |
| Grounded labelling ($\mathtt{IN}$ arguments) | Sceptical consequences |
| Argument is $\mathtt{OUT}$ | Claim is not a consequence |
| Argument is $\mathtt{UNDEC}$ | Claim is neither in nor out (dilemma) |

This works for all three NM logics studied this week:

- **Logic Programming**: arguments are chains of LP rules and facts; attacks on `NOT X` assumptions.
- **Maxiconsistent**: arguments are $[\Delta, \alpha]$ tuples; attacks negate a premise; preferences from stratification.
- **Default Logic**: arguments correspond to applicable default chains; priorities resolve conflicts.

---

## Section 8: Dung (1995) — Key Formal Definitions

The seminal paper: P.M. Dung, "On the acceptability of arguments and its fundamental role in nonmonotonic reasoning, logic programming and n-person games," *Artificial Intelligence* 77 (1995) 321–357.

### 8.1 Core Definitions

**Definition 2** (Argumentation Framework): $AF = \langle AR, \mathit{attacks}\rangle$ where $AR$ is a set of arguments and $\mathit{attacks} \subseteq AR \times AR$.

**Definition 5** (Conflict-free): A set $S$ of arguments is **conflict-free** if there are no $A, B \in S$ such that $A$ attacks $B$.

**Definition 6** (Acceptability and Admissibility):
1. An argument $A \in AR$ is **acceptable** w.r.t. a set $S$ iff for each $B \in AR$: if $B$ attacks $A$ then $B$ is attacked by $S$.
2. A conflict-free set $S$ is **admissible** iff each argument in $S$ is acceptable w.r.t. $S$.

**Definition 7** (Preferred Extension): A **preferred extension** is a **maximal** (under set inclusion) admissible set.

**Corollary 12**: Every AF possesses at least one preferred extension.

**Definition 13** (Stable Extension): A conflict-free set $S$ is a **stable extension** iff $S$ attacks each argument not in $S$.

**Lemma 14**: $S$ is a stable extension iff $S = \{A \mid A \text{ is not attacked by } S\}$.

**Lemma 15**: Every stable extension is a preferred extension, but not vice versa.

**Definition 16** (Characteristic Function): $F_{AF}: 2^{AR} \to 2^{AR}$:

$$F_{AF}(S) = \{A \mid A \text{ is acceptable w.r.t. } S\}$$

**Lemma 19**: $F_{AF}$ is **monotonic** (w.r.t. set inclusion).

**Definition 20** (Grounded Extension): The **grounded extension** $GE_{AF}$ is the **least fixed point** of $F_{AF}$.

**Definition 23** (Complete Extension): An admissible set $S$ is a **complete extension** iff each argument acceptable w.r.t. $S$ belongs to $S$.

**Theorem 25**:
1. Each preferred extension is a complete extension (not vice versa).
2. The grounded extension is the **least** (w.r.t. set inclusion) complete extension.
3. Complete extensions form a complete semilattice w.r.t. set inclusion.

**Definition 27** (Finitary AF): An AF is **finitary** iff for each argument $A$, there are only finitely many arguments in $AR$ which attack $A$.

**Lemma 28**: If $AF$ is finitary, then $F_{AF}$ is $\omega$-continuous.

### 8.2 Labellings (Modgil & Caminada)

From Chapter 6 of *Argumentation in Artificial Intelligence* (Modgil & Caminada):

**Definition 6.1**: Let $\langle \mathcal{A}, \mathcal{R}\rangle$ be an AF. A **labelling** is a total function $\mathcal{L}: \mathcal{A} \mapsto \{\mathtt{IN}, \mathtt{OUT}, \mathtt{UNDEC}\}$.

The labelling approach provides an alternative, procedural characterisation of AF semantics that is equivalent to the extension-based approach. It supports:
- **Credulous membership**: Is $A$ contained in an extension? ($A$ is $\mathtt{IN}$ in some preferred labelling)
- **Sceptical membership**: Is $A$ contained in all extensions? ($A$ is $\mathtt{IN}$ in the grounded labelling)

---

## Section 9: Tutorial Sheet Questions and Worked Solutions

### LGT Question 1: Full Argument Framework Construction

**Given facts and rules**:

| Facts | Rules |
|-------|-------|
| p, s, j, t, m, k, i | Rule 1: $j \to g$; Rule 2: $g \wedge p \wedge \mathit{NOT}\;q \to r$; Rule 3: $t \wedge \mathit{NOT}\;s \to q$; Rule 4: $w \wedge \mathit{NOT}\;z \wedge \mathit{NOT}\;r \to s$; Rule 5: $k \wedge \mathit{NOT}\;v \to z$; Rule 6: $m \to v$; Rule 7: $i \to w$ |

**Arguments** (from model answer):

| Name | Construction | Claim |
|------|-------------|-------|
| A | $[p]$ | $p$ |
| B | $[s]$ | $s$ |
| C | $[j]$ | $j$ |
| D | $[t]$ | $t$ |
| E | $[m]$ | $m$ |
| F | $[k]$ | $k$ |
| G | $[i]$ | $i$ |
| H | $[j;\; j\to g]$ | $g$ |
| I | $[j,\; j\to g;\; p;\; g\wedge p\wedge \mathit{NOT}\;q\to r]$ | $r$ |
| J | $[t;\; t\wedge \mathit{NOT}\;s\to q]$ | $q$ |
| K | $[i;\; i\to w]$ | $w$ |
| L | $[i;\; i\to w;\; w\wedge \mathit{NOT}\;z\wedge \mathit{NOT}\;r\to s]$ | $s$ |
| M | $[m;\; m\to v]$ | $v$ |
| N | $[k;\; k\wedge \mathit{NOT}\;v\to z]$ | $z$ |

**Attacks** (from model answer diagram):
- $B$ (claims $s$) attacks $J$ (assumes $\mathit{NOT}\;s$)
- $J$ (claims $q$) attacks $I$ (assumes $\mathit{NOT}\;q$)
- $I$ (claims $r$) attacks $L$ (assumes $\mathit{NOT}\;r$)
- $L$ (claims $s$) attacks $J$ (assumes $\mathit{NOT}\;s$)
- $N$ (claims $z$) attacks $L$ (assumes $\mathit{NOT}\;z$)
- $M$ (claims $v$) attacks $N$ (assumes $\mathit{NOT}\;v$)

**Preferred and grounded labellings** (from diagram in model answer):
- B = **IN**, J = **OUT**, I = **IN**, L = **OUT**, N = **OUT**, M = **IN**

### LGT Question 2: Stratified KB — Preferred MCS

**Stratified KB**:

$$S_1:\; \neg a\lor\neg b\lor\neg c$$
$$S_2:\; a,\quad a\to d$$
$$S_3:\; b,\quad c,\quad \neg d\lor\neg c$$

**Answer** (from model answer):

$$E_1 = \{\neg a\lor\neg b\lor\neg c,\; a,\; a\to d,\; b,\; \neg d\lor\neg c\}$$
$$E_2 = \{\neg a\lor\neg b\lor\neg c,\; a,\; a\to d,\; c\}$$

### SGT Question 1: Logic Programming

**KB1**:
1. `p(X) :- v(X), r(X), NOT s(X)`
2. `s(X) :- g(X), NOT t(X)`
3. `m(X) :- v(X), NOT q(X)`
4. `q(X) :- v(X), NOT m(X)`
5. `v(X) :- j(X), NOT w(X)`
6. `j(tom)`, 7. `r(tom)`

**(i)** Is $\mathit{KB}_1 \mathrel{\vert\!\!\sim} p(\mathit{tom})$?

**Answer**: Yes. $\mathit{KB}_1 \mathrel{\not\vert\!\!\sim} s(\mathit{tom})$ (no $g(\mathit{tom})$), so `NOT s(tom)` holds, and rule 1 fires. ✓

**(ii)** What fact removes $p(\mathit{tom})$?

**Answer**: Add $g(\mathit{tom})$. Then $s(\mathit{tom})$ becomes derivable (rule 2, since `NOT t(tom)` holds — no $t(\mathit{tom})$), blocking rule 1.

**(iii)** Is $m(\mathit{tom})$ or $q(\mathit{tom})$ a non-monotonic consequence?

**Answer**: **No**. Rules 3 and 4 mutually depend on each other's `NOT` — both queries lead to an **infinite loop**.

### SGT Question 2: Stratified KB — Conference Example

$$S_1:\; g,\quad g\to\neg a\lor\neg b \qquad S_2:\; (a\land b)\to\neg c,\quad a,\quad b \qquad S_3:\; c$$

**Answer**:

$$E_1 = \{g,\; g\to\neg a\lor\neg b,\; (a\land b)\to\neg c,\; a,\; c\}$$
$$E_2 = \{g,\; g\to\neg a\lor\neg b,\; (a\land b)\to\neg c,\; b,\; c\}$$

(Cannot have both $a$ and $b$ in $S_2$ — $g\to\neg a\lor\neg b$ from $S_1$ forces at most one.)

### SGT Question 3: Default Extensions (no ordering)

$$W = \{\neg a\lor\neg b\lor\neg c,\; d\to\neg c\}$$
$$\text{Defaults:}\quad 1.\;\top\Rightarrow a,\quad 2.\;a\Rightarrow d,\quad 3.\;\top\Rightarrow c,\quad 4.\;d\Rightarrow b$$

**Answer**: Two default extensions:

$$E_1 = Cn(W \cup \{a, d, b\}) \qquad E_2 = Cn(W \cup \{a, c\})$$

*Derivation of $E_1$*: Apply 1 ($\top\Rightarrow a$), then 2 ($a\Rightarrow d$), then 4 ($d\Rightarrow b$). Default 3 blocked: $\neg a\lor\neg b\lor\neg c$ with $a,b$ forces $\neg c$; adding $c$ would be inconsistent.

*Derivation of $E_2$*: Apply 1 ($\top\Rightarrow a$), then 3 ($\top\Rightarrow c$). Default 2 blocked: $d\to\neg c$ with $c$ means adding $d$ forces $\neg c$, contradiction with $c$.

### SGT Question 4: Default Extensions (with ordering $1>2>3>4$)

**Answer**: One extension only: $Cn(W \cup \{a, d, b\})$.

Apply in priority order: 1 (add $a$), 2 (add $d$), 3 is blocked (inconsistency), 4 (add $b$). Default 3 ($\top\Rightarrow c$) cannot be applied because with $a$ and $d$ already in the extension, $\neg c$ is forced by $d\to\neg c$.

### SGT Question 5: Arguments and Attacks from KB1; Preference for $m(\mathit{tom})$

**Arguments** (from KB1 in Q1):

- $A_1 = [r(\mathit{tom})]$, $A_2 = [j(\mathit{tom})]$, $A_3 = [v(\mathit{tom})\mathrel{:-}j(\mathit{tom}), \mathit{NOT}\;w(\mathit{tom});\; j(\mathit{tom})]$: claims $v(\mathit{tom})$
- $B = [p(\mathit{tom})\mathrel{:-}v(\mathit{tom}), r(\mathit{tom}), \mathit{NOT}\;s(\mathit{tom});\; v(\mathit{tom})\mathrel{:-}\ldots;\; r(\mathit{tom}), j(\mathit{tom})]$: claims $p(\mathit{tom})$
- $C_1 = [m(\mathit{tom})\mathrel{:-}v(\mathit{tom}), \mathit{NOT}\;q(\mathit{tom});\; v(\mathit{tom})\mathrel{:-}j(\mathit{tom}), \mathit{NOT}\;w(\mathit{tom});\; j(\mathit{tom})]$: claims $m(\mathit{tom})$
- $C_2 = [q(\mathit{tom})\mathrel{:-}v(\mathit{tom}), \mathit{NOT}\;m(\mathit{tom});\; v(\mathit{tom})\mathrel{:-}j(\mathit{tom}), \mathit{NOT}\;w(\mathit{tom});\; j(\mathit{tom})]$: claims $q(\mathit{tom})$

**Attacks**: $C_1$ attacks $C_2$ (claims $m(\mathit{tom})$, challenges $\mathit{NOT}\;m(\mathit{tom})$ in $C_2$); $C_2$ attacks $C_1$ (claims $q(\mathit{tom})$, challenges $\mathit{NOT}\;q(\mathit{tom})$ in $C_1$).

**Preference to make $m(\mathit{tom})$ win**: Set $C_1 > C_2$. Then the attack from $C_2$ to $C_1$ is **cancelled**. Only $C_1 \to C_2$ survives. $C_1$ is unattacked $\Rightarrow$ **winning**. $C_1$'s claim $m(\mathit{tom})$ corresponds to $\mathit{KB} \mathrel{\vert\!\!\sim} m(\mathit{tom})$.

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Non-monotonic logic** | A logic where $\mathit{KB} \mathrel{\vert\!\!\sim} \alpha$ but $\mathit{KB} \cup \{\beta\} \mathrel{\not\vert\!\!\sim} \alpha$ is possible |
| **Negation as failure (NOT)** | $\mathit{NOT}\;X$ succeeds iff $X$ cannot be proved from the current KB |
| **Credulous consequence** | $\alpha$ follows from at least one preferred extension/mcs |
| **Sceptical consequence** | $\alpha$ follows from all preferred extensions/mcs |
| **Stratified KB** | A KB with formulas arranged in strata $S_1 > S_2 > \cdots > S_n$ by priority |
| **Preferred mcs** | A maximal consistent subset built by processing strata top-down |
| **Default theory** | A pair $(W, \mathit{Defaults})$ of classical facts and default rules |
| **Default extension** | A maximal consistent set of classical consequences obtained by applying applicable defaults |
| **Default rule** | $\alpha_1,\ldots,\alpha_n \Rightarrow \beta$ — if antecedents hold, typically conclude $\beta$ |
| **Argumentation Framework (AF)** | $\langle AR, \mathit{attacks}\rangle$ — a set of arguments with a binary attack relation |
| **Argument** | A set of premises/rules $[\Delta]$ with a claim $\alpha$, written $[\Delta] : \alpha$ |
| **Attack** | $A$ attacks $B$ if $A$'s claim undermines an assumption/premise of $B$ |
| **Successful attack** | An attack that is not cancelled by the preference ordering |
| **Labelling** | Assignment of $\mathtt{IN}$, $\mathtt{OUT}$, or $\mathtt{UNDEC}$ to each argument in an AF |
| **Legal labelling** | A labelling satisfying Rules 1, 2, 3 |
| **Preferred labelling** | A legal labelling with a maximal set of $\mathtt{IN}$ arguments (credulous) |
| **Grounded labelling** | The unique legal labelling with a minimal set of $\mathtt{IN}$ arguments (sceptical) |
| **Conflict-free set** | A set $S$ where no argument in $S$ attacks another in $S$ |
| **Admissible set** | A conflict-free set where every argument is acceptable w.r.t. $S$ |
| **Preferred extension (Dung)** | Maximal admissible set |
| **Grounded extension (Dung)** | Least fixed point of $F_{AF}$ (least complete extension) |
| **Stable extension (Dung)** | A conflict-free set $S$ that attacks all arguments not in $S$ |
| **Characteristic function $F_{AF}$** | $F_{AF}(S) = \{A \mid A \text{ acceptable w.r.t. } S\}$ — monotonic |
| **Subclass principle** | Rules for a subclass take priority over rules for the superclass |
| **Finitary AF** | Each argument has only finitely many attackers |

---

## Summary

| LO | Topic | Key Points |
|----|-------|-----------|
| **LO1** | Three NM logics | (1) LP with NAF: derive $\mathit{KB}\mathrel{\vert\!\!\sim}Q$ iff Prolog query returns true; (2) Maxiconsistent: pick preferred mcs respecting priority stratification, then take credulous/sceptical consequences; (3) Default Logic: apply defaults in priority order, each consistent extension is a set of non-monotonic consequences |
| **LO2** | Constructing AFs | From LP KB: arguments are chains of rules+facts; claim = head of final rule. From classical KB: arguments are $[\Delta,\alpha]$ tuples. Attack = claim undermines NOT-assumption (LP) or negates a premise (classical) |
| **LO3** | Preferences | Preference $A > B$ cancels the attack from $B$ to $A$. Sources: goal preferences (decision KBs), rule priorities (LP/default), formula stratification (classical). Preferred argument wins in a dilemma |
| **LO4** | Labelling rules | IN = all attackers are OUT; OUT = at least one attacker is IN; UNDEC = no attacker is IN but at least one attacker is UNDEC. Preferred labellings maximise IN (credulous). Grounded labelling minimises IN (sceptical, unique) |
| **LO5** | AF → NM consequences | Claims of IN-arguments in preferred labellings = credulous NM consequences. Claims of IN-arguments in grounded labelling = sceptical NM consequences. This holds for LP, maxiconsistent, and default logic KBs |
