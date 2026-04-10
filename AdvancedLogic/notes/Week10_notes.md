# Week 10: The Tableau Algorithm for Modal Logic

## Learning Objectives
By the end of this week, you should be able to:
- [ ] Define a propositional tableau and apply the propositional tableau rules ($\neg_\text{true}$, $\wedge_\text{true}$, $\vee_\text{true}$, $\to_\text{true}$, $\neg_\text{false}$, $\wedge_\text{false}$, $\vee_\text{false}$, $\to_\text{false}$)
- [ ] Determine whether a propositional formula is satisfiable or a tautology using the tableau algorithm
- [ ] Extend the propositional tableau to a modal tableau using tagged formulas and the Relation column
- [ ] Apply the four modal tableau rules: $(\Diamond_\text{true})$, $(\Box_\text{false})$, $(\Box_\text{true})$, $(\Diamond_\text{false})$
- [ ] Read off a satisfying Kripke model from a fully expanded open modal tableau
- [ ] Adapt the modal tableau algorithm to decide satisfiability with respect to a specific class of frames $\mathcal{C}$
- [ ] Use the tableau algorithm to decide modal validity by placing the negated formula in the False column

---

## 10.1 Why a Tableau Algorithm?

For classical propositional logic, satisfiability and validity are decidable by truth tables: a formula with $n$ propositional variables requires at most $2^n$ rows. However, for modal logic:

- Kripke models are not required to be **finite** — an infinite number of possible worlds may be needed (e.g., in temporal logics where each world represents a future time instant)
- There is no fixed bound on the number of worlds to check
- Simply exhausting all possible models is infeasible

The **Tableau Algorithm** provides a systematic, finite procedure for deciding modal satisfiability and validity.

---

## 10.2 Propositional Tableaux

### 10.2.1 Definition

**Definition 10.1 (Tableau).** A **(propositional) tableau** (plural: **tableaux**) is a two-column table with:

- A **True** column: lists all subformulas we want to be true
- A **False** column: lists all subformulas we want to be false

Formally, a tableau is a pair $(\mathbf{True}, \mathbf{False}) \in \mathcal{P}(\mathit{Fml}) \times \mathcal{P}(\mathit{Fml})$ where **True** and **False** are finite subsets of formulas.

A tableau is **closed** if $\mathbf{True} \cap \mathbf{False} \neq \emptyset$ (same formula appears in both columns — contradiction). A tableau is **open** otherwise.

### 10.2.2 Checking Satisfiability

To decide whether $F \in \mathit{Fml}$ is **satisfiable**: create the **initial tableau** $(\{F\}, \emptyset)$ with $F$ in the True column. Then successively apply the expansion rules below until no more rules can be applied.

- If **any** branch results in a fully expanded **open** tableau: $F$ is **satisfiable**. Read off the satisfying assignment: $V(p) = 1 \iff p \in \mathbf{True}$.
- If **every** branch of the algorithm results in a **closed** tableau: $F$ is **unsatisfiable**.

### 10.2.3 Rules for the True Column

**$(\neg_\text{true})$ — Negation True:** If $\neg F$ appears in **True**, move $F$ to **False** and mark $\neg F$ as expanded (cross it out):

$$\frac{\neg F \in \mathbf{True}}{F \in \mathbf{False}} \quad (\neg_\text{true})$$

**$(\wedge_\text{true})$ — Conjunction True:** If $(F \wedge G)$ appears in **True**, add both conjuncts $F$ and $G$ to **True** and mark $(F\wedge G)$ as expanded:

$$\frac{(F \wedge G) \in \mathbf{True}}{F \in \mathbf{True},\ G \in \mathbf{True}} \quad (\wedge_\text{true})$$

**$(\vee_\text{true})$ — Disjunction True (branching rule):** If $(F \vee G)$ appears in **True**, **branch** into two sub-tableaux: one with $F$ added to **True**, one with $G$ added to **True**:

$$\frac{(F \vee G) \in \mathbf{True}}{\text{Branch 1: } F \in \mathbf{True} \quad | \quad \text{Branch 2: } G \in \mathbf{True}} \quad (\vee_\text{true})$$

**$(\to_\text{true})$ — Implication True (branching rule):** If $(F \to G)$ appears in **True**, branch: one branch with $F$ in **False** (i.e., $F$ is false, making $F\to G$ trivially true), one branch with $G$ in **True**:

$$\frac{(F \to G) \in \mathbf{True}}{\text{Branch 1: } F \in \mathbf{False} \quad | \quad \text{Branch 2: } G \in \mathbf{True}} \quad (\to_\text{true})$$

(Alternatively: $F \to G \equiv \neg F \vee G$, so this is a combination of $(\neg_\text{true})$ and $(\vee_\text{true})$.)

### 10.2.4 Rules for the False Column

**$(\neg_\text{false})$ — Negation False:** If $\neg F$ appears in **False**, move $F$ to **True**:

$$\frac{\neg F \in \mathbf{False}}{F \in \mathbf{True}} \quad (\neg_\text{false})$$

**$(\wedge_\text{false})$ — Conjunction False (branching rule):** $(F \wedge G)$ is false iff $F$ is false **or** $G$ is false. Branch:

$$\frac{(F \wedge G) \in \mathbf{False}}{\text{Branch 1: } F \in \mathbf{False} \quad | \quad \text{Branch 2: } G \in \mathbf{False}} \quad (\wedge_\text{false})$$

**$(\vee_\text{false})$ — Disjunction False:** $(F \vee G)$ is false iff **both** $F$ and $G$ are false. Add both to **False**:

$$\frac{(F \vee G) \in \mathbf{False}}{F \in \mathbf{False},\ G \in \mathbf{False}} \quad (\vee_\text{false})$$

**$(\to_\text{false})$ — Implication False:** $(F \to G)$ is false iff $F$ is true **and** $G$ is false. Add $F$ to **True** and $G$ to **False**:

$$\frac{(F \to G) \in \mathbf{False}}{F \in \mathbf{True},\ G \in \mathbf{False}} \quad (\to_\text{false})$$

### 10.2.5 Checking Validity (Tautology)

To decide whether $F$ is a **tautology** (valid):

$$F \text{ is valid} \iff \neg F \text{ is NOT satisfiable}$$

Place $F$ in the **False** column of the initial tableau $(\emptyset, \{F\})$ and expand. $F$ is valid iff every fully expanded branch is **closed** (no open branch corresponds to a counter-model).

---

## 10.3 The Modal Tableau Algorithm

### 10.3.1 Motivation and Tagged Formulas

In modal logic, satisfiability varies world-by-world. We must track not just whether a subformula should be true/false, but **at which world**.

**Definition 10.2 (Tagged Formula).** A **tagged formula** is a pair $(w : F)$ where:
- $w$ is a **label** designating a specific world in the to-be-determined Kripke model
- $F \in \mathit{Fml}^\Box$ is a modal formula

### 10.3.2 Modal Tableaux

**Definition 10.3 (Modal Tableau).** A **(modal) tableau** is a three-column table with:
- A **True** column: tagged (sub)formulas that must be true
- A **False** column: tagged (sub)formulas that must be false
- A **Relation** column: relations $(w, u)$ that must be in the accessibility relation $R$

A modal tableau is **closed** if $\mathbf{True} \cap \mathbf{False} \neq \emptyset$ (a tagged formula $(w:F)$ appears in both columns), and **open** otherwise.

### 10.3.3 All Propositional Rules Carry Over

All eight propositional rules remain valid, but now applied to **tagged** formulas. For example, $(\to_\text{true})$ becomes: if $(w:(F\to G)) \in \mathbf{True}$, branch into $(w:F) \in \mathbf{False}$ or $(w:G) \in \mathbf{True}$.

### 10.3.4 Modal Rules

There are four additional rules for formulas involving $\Box$ and $\Diamond$:

**$(\Diamond_\text{true})$ — Diamond True:** If $(w:\Diamond F)$ appears in **True**, there must exist some accessible world $u$ where $F$ is true. Introduce a **fresh label** $u$ (not appearing elsewhere on the current branch):

$$\frac{(w : \Diamond F) \in \mathbf{True}}{(u : F) \in \mathbf{True},\quad (w, u) \in \mathbf{Relation}} \quad (\Diamond_\text{true})$$

> **IMPORTANT:** $u$ must be a **fresh** label — use sequential numbering $w_0, w_1, w_2, \ldots$ and take the next available index each time $(\Diamond_\text{true})$ is applied.

**$(\Box_\text{false})$ — Box False:** If $(w:\Box F)$ appears in **False**, there must exist some accessible world where $F$ is false. Introduce a fresh label $u$:

$$\frac{(w : \Box F) \in \mathbf{False}}{(u : F) \in \mathbf{False},\quad (w, u) \in \mathbf{Relation}} \quad (\Box_\text{false})$$

**$(\Box_\text{true})$ — Box True:** If $(w:\Box F)$ appears in **True**, then $F$ must be true at every accessible world. Look through **Relation** for all pairs of the form $(w, u_i)$ and add $(u_i : F)$ to **True** for each:

$$\frac{(w : \Box F) \in \mathbf{True},\quad (w,u_1),\ldots,(w,u_n) \in \mathbf{Relation}}{(u_1:F), \ldots, (u_n:F) \in \mathbf{True}} \quad (\Box_\text{true})$$

> **CRITICAL:** Do **NOT** mark $(w:\Box F)$ as expanded. It may need to be re-applied if additional relations $(w,u)$ are added to **Relation** later.

**$(\Diamond_\text{false})$ — Diamond False:** If $(w:\Diamond F)$ appears in **False**, then $F$ must be false at every accessible world. Look through **Relation** for all $(w, u_i)$ and add $(u_i : F)$ to **False** for each:

$$\frac{(w : \Diamond F) \in \mathbf{False},\quad (w,u_1),\ldots,(w,u_n) \in \mathbf{Relation}}{(u_1:F), \ldots, (u_n:F) \in \mathbf{False}} \quad (\Diamond_\text{false})$$

> **CRITICAL:** Do **NOT** mark $(w:\Diamond F)$ as expanded. It may need to be re-applied if additional relations are added.

### 10.3.5 Termination and Reading off a Model

The algorithm terminates when:
- All formulas in **True** and **False** have been fully expanded, AND
- Every $\Box$ formula in **True** and every $\Diamond$ formula in **False** has been applied to all relations in **Relation**

**If any branch is fully expanded and open:** $F$ is **satisfiable**. Read off the satisfying Kripke model:

$$W = \{w_i : (w_i : p) \in \mathbf{True} \cup \mathbf{False}\}$$
$$(w_i, w_j) \in R \iff (w_i, w_j) \in \mathbf{Relation}$$
$$V(w_i, p) = 1 \iff (w_i : p) \in \mathbf{True}$$

**If every branch is closed:** $F$ is **unsatisfiable**.

> **Tree Model Property:** Models produced by the tableau algorithm always have a tree structure. Every satisfiable modal formula has a tree model.

### 10.3.6 Initial Tableau Setup

- **To check satisfiability of $F$:** Initial tableau with $(w_0 : F)$ in **True**, everything else empty.
- **To check validity of $F$:** Use $F \text{ valid} \iff \neg F \text{ not satisfiable}$. Initial tableau with $(w_0 : F)$ in **False** and apply rules; $F$ is valid iff every branch is closed.

$$F \text{ is valid with respect to } \mathcal{C} \iff \neg F \text{ is NOT satisfiable with respect to } \mathcal{C}$$

---

## 10.4 Deciding Satisfiability w.r.t. a Class of Frames $\mathcal{C}$

To check satisfiability with respect to a specific class of frames (e.g., reflexive, transitive), after each application of $(\Diamond_\text{true})$ or $(\Box_\text{false})$ — which add new relations — **close** the **Relation** set under the additional constraints required by the frame class.

**Example — Transitive frames:** Whenever $(x,y)$ and $(y,z)$ are added to **Relation**, also add $(x,z)$ to ensure transitivity. This guarantees that any model read from an open tableau will be transitive.

**Example — Reflexive frames:** After adding world label $w_i$, also add $(w_i, w_i)$ to **Relation**.

> **Issue with transitive frames and looping:** Some satisfiable formulas on transitive frames can cause the tableau algorithm to loop (generating new worlds indefinitely). For example, $F = \Box(p \to \Diamond q) \wedge \Box(q \to \Diamond p) \wedge \Diamond p$ causes an infinite chain $w_0, w_1, w_2, \ldots$ alternating $p$ and $q$. A fix is to **reuse existing world labels** when the formula being added to a world already appears there (a *blocking* or *loop-checking* mechanism).

---

## 10.5 SGT Questions

### SGT 10.1 — Checking Satisfiability of Modal Formulas

**(i) $F_1 = \Diamond(p \vee q) \wedge \Box\neg p \wedge \Box(q \to \Diamond r)$**

Initial tableau: $(w_0 : F_1)$ in True.

Expanding $\wedge$: $(w_0:\Diamond(p\vee q))$, $(w_0:\Box\neg p)$, $(w_0:\Box(q\to\Diamond r))$ all in True.

Apply $(\Diamond_\text{true})$ to $(w_0:\Diamond(p\vee q))$: introduce fresh $w_1$. Add $(w_1:(p\vee q))$ to True and $(w_0,w_1)$ to Relation.

Apply $(\Box_\text{true})$ to $(w_0:\Box\neg p)$ with relation $(w_0,w_1)$: add $(w_1:\neg p)$ to True.

Apply $(\Box_\text{true})$ to $(w_0:\Box(q\to\Diamond r))$ with relation $(w_0,w_1)$: add $(w_1:(q\to\Diamond r))$ to True.

Expand $(w_1:(p\vee q))$ — branch:
- **Branch A:** $(w_1:p)$ in True. But $(w_1:\neg p)$ also in True, so $(w_1:p)$ would go to False — **closed** (contradiction).
- **Branch B:** $(w_1:q)$ in True. Apply $(\to_\text{true})$ to $(w_1:(q\to\Diamond r))$: branch on $q$ in False (contradiction since $q$ in True) OR $\Diamond r$ in True. Take $\Diamond r$ in True. Apply $(\Diamond_\text{true})$: introduce $w_2$, add $(w_1:r)$ to True and $(w_1,w_2)$ to Relation. This branch is **open**.

$F_1$ is **satisfiable**. A witness model: $W=\{w_0,w_1,w_2\}$, $R=\{(w_0,w_1),(w_1,w_2)\}$, $V(w_1,q)=1$, $V(w_2,r)=1$, all others 0.

**(ii) $F_2 = \Box(p \to \Diamond q) \wedge \Diamond(p \wedge \Box\neg q)$**

Initial tableau: $(w_0:F_2)$ in True.

Expand $\wedge$: $(w_0:\Box(p\to\Diamond q))$ and $(w_0:\Diamond(p\wedge\Box\neg q))$ in True.

Apply $(\Diamond_\text{true})$ to the diamond: introduce $w_1$, add $(w_1:(p\wedge\Box\neg q))$ to True and $(w_0,w_1)$ to Relation.

Apply $(\Box_\text{true})$ to $(w_0:\Box(p\to\Diamond q))$ with $(w_0,w_1)$: add $(w_1:(p\to\Diamond q))$ to True.

Expand $(w_1:(p\wedge\Box\neg q))$: add $(w_1:p)$ and $(w_1:\Box\neg q)$ to True.

Expand $(w_1:(p\to\Diamond q))$ — branch:
- **Branch A:** $(w_1:p)$ in False. But $(w_1:p)$ in True — **closed**.
- **Branch B:** $(w_1:\Diamond q)$ in True. Apply $(\Diamond_\text{true})$: introduce $w_2$, add $(w_2:q)$ to True and $(w_1,w_2)$ to Relation.

Apply $(\Box_\text{true})$ to $(w_1:\Box\neg q)$ with $(w_1,w_2)$: add $(w_2:\neg q)$ to True, which means $(w_2:q)$ in False. But $(w_2:q)$ in True — **closed**.

$F_2$ is **unsatisfiable**.

**(iii) $F_3 = \Diamond(p \wedge \Diamond q) \wedge \Box(\Diamond\neg q \wedge \Box r)$**

Apply $(\Diamond_\text{true})$ to $\Diamond(p\wedge\Diamond q)$: introduce $w_1$, add $(w_1:p)$, $(w_1:\Diamond q)$ to True, $(w_0,w_1)$ to Relation.

Apply $(\Box_\text{true})$ to $\Box(\Diamond\neg q \wedge \Box r)$ with $(w_0,w_1)$: add $(w_1:(\Diamond\neg q \wedge \Box r))$ to True. Expand: $(w_1:\Diamond\neg q)$ and $(w_1:\Box r)$ in True.

Apply $(\Diamond_\text{true})$ to $(w_1:\Diamond q)$: introduce $w_2$, add $(w_2:q)$ to True, $(w_1,w_2)$ to Relation.

Apply $(\Diamond_\text{true})$ to $(w_1:\Diamond\neg q)$: introduce $w_3$... OR apply to $w_2$ (re-use? — here need fresh label; use $w_3$), add $(w_3:\neg q)$ to True, $(w_1,w_3)$ to Relation.

Apply $(\Box_\text{true})$ to $(w_1:\Box r)$ with $(w_1,w_2)$: add $(w_2:r)$ to True. With $(w_1,w_3)$: add $(w_3:r)$ to True.

No contradictions arise. This branch is **open**.

$F_3$ is **satisfiable**. A witness model: $W=\{w_0,w_1,w_2,w_3\}$, $R=\{(w_0,w_1),(w_1,w_2),(w_1,w_3)\}$, $V(w_1,p)=1$, $V(w_2,q)=1$, $V(w_2,r)=1$, $V(w_3,r)=1$, all others 0.

---

### SGT 10.2 — Proving $A_K = \Box(p \to q) \to (\Box p \to \Box q)$ Valid

To show $A_K$ is valid over all frames, place $A_K$ in **False** in the initial tableau:

$(w_0 : \Box(p\to q) \to (\Box p \to \Box q))$ in False.

Apply $(\to_\text{false})$: $(w_0:\Box(p\to q))$ in True, $(w_0:(\Box p \to \Box q))$ in False.

Apply $(\to_\text{false})$: $(w_0:\Box p)$ in True, $(w_0:\Box q)$ in False.

Apply $(\Box_\text{false})$ to $(w_0:\Box q)$: introduce $w_1$, add $(w_1:q)$ to False and $(w_0,w_1)$ to Relation.

Apply $(\Box_\text{true})$ to $(w_0:\Box(p\to q))$ with $(w_0,w_1)$: add $(w_1:(p\to q))$ to True.

Apply $(\Box_\text{true})$ to $(w_0:\Box p)$ with $(w_0,w_1)$: add $(w_1:p)$ to True.

Expand $(w_1:(p\to q))$ — branch:
- **Branch A:** $(w_1:p)$ in False. But $(w_1:p)$ in True — **closed**.
- **Branch B:** $(w_1:q)$ in True. But $(w_1:q)$ in False — **closed**.

Both branches closed. Therefore $\neg A_K$ is unsatisfiable, so $A_K$ is **valid in all frames**. $\square$

---

### SGT 10.3 — Proving $A_4 = \Diamond\Diamond p \to \Diamond p$ Valid over Transitive Frames

To show $A_4$ is valid over all transitive frames, place $A_4$ in **False**:

$(w_0:\Diamond\Diamond p \to \Diamond p)$ in False.

Apply $(\to_\text{false})$: $(w_0:\Diamond\Diamond p)$ in True, $(w_0:\Diamond p)$ in False.

Apply $(\Diamond_\text{true})$ to $(w_0:\Diamond\Diamond p)$: introduce $w_1$, add $(w_1:\Diamond p)$ to True and $(w_0,w_1)$ to Relation.

**In a transitive frame:** also add $(w_0,w_1)$ — already have it. No immediate transitivity closure yet.

Apply $(\Diamond_\text{false})$ to $(w_0:\Diamond p)$ with $(w_0,w_1)$ in Relation: add $(w_1:p)$ to False.

Apply $(\Diamond_\text{true})$ to $(w_1:\Diamond p)$: introduce $w_2$, add $(w_2:p)$ to True and $(w_1,w_2)$ to Relation.

**Transitive closure:** $(w_0,w_1),(w_1,w_2) \in R \Rightarrow$ add $(w_0,w_2)$ to Relation.

Apply $(\Diamond_\text{false})$ to $(w_0:\Diamond p)$ with $(w_0,w_2)$: add $(w_2:p)$ to False. But $(w_2:p)$ in True — **closed**.

All branches closed. Therefore $\neg A_4$ is unsatisfiable over transitive frames, so $A_4$ is **valid over all transitive frames**. $\square$

---

### SGT 10.4 — Looping Issue with Transitive Frames

**Formula:** $F = \Box(p \to \Diamond q) \wedge \Box(q \to \Diamond p) \wedge \Diamond p$

**Issue:** Applying the tableau algorithm on transitive frames causes an infinite chain:
1. $\Diamond p$ gives $w_1$ with $p$ true
2. $\Box(p\to\Diamond q)$ applied at $w_0$ gives $(w_1:\Diamond q)$, spawning $w_2$ with $q$ true
3. Transitive closure adds $(w_0,w_2)$
4. $\Box(q\to\Diamond p)$ applied at $w_0$ gives $(w_2:\Diamond p)$, spawning $w_3$ with $p$ true
5. This repeats indefinitely: $w_4, w_5, \ldots$

**Suggested fix — loop-checking (blocking):** Before creating a new fresh world $w_{n+1}$ for a formula $(w_i:\Diamond F)$ or $(w_i:\Box F)$, check whether there already exists a world $w_j$ on the current branch that has the exact same set of formulas labelled to it. If so, **reuse** $w_j$ (set $(w_i,w_j)$ in Relation) instead of creating a new world. This blocking condition ensures termination while preserving completeness.

---

## Key Definitions

| Term | Definition |
|------|-----------|
| **Tableau** | A two-column (propositional) or three-column (modal) table with True, False, and (for modal) Relation columns |
| **Closed tableau** | A tableau where $\mathbf{True} \cap \mathbf{False} \neq \emptyset$ — a contradiction exists |
| **Open tableau** | A tableau that is not closed — no contradiction |
| **Branching rule** | A tableau rule that splits into two sub-tableaux (e.g., $\vee_\text{true}$, $\wedge_\text{false}$) |
| **Non-branching rule** | A tableau rule that adds formulas to the current tableau without splitting (e.g., $\wedge_\text{true}$, $\vee_\text{false}$) |
| **Tagged formula** | A pair $(w:F)$ linking a modal formula $F$ to a world label $w$ |
| **Fresh label** | A world label $u$ not appearing anywhere else in the current tableau branch |
| **$(\Diamond_\text{true})$ rule** | If $(w:\Diamond F)$ in True: add fresh $u$, put $(u:F)$ in True, add $(w,u)$ to Relation |
| **$(\Box_\text{false})$ rule** | If $(w:\Box F)$ in False: add fresh $u$, put $(u:F)$ in False, add $(w,u)$ to Relation |
| **$(\Box_\text{true})$ rule** | If $(w:\Box F)$ in True: for each $(w,u_i)$ in Relation, add $(u_i:F)$ to True. Do NOT mark as expanded. |
| **$(\Diamond_\text{false})$ rule** | If $(w:\Diamond F)$ in False: for each $(w,u_i)$ in Relation, add $(u_i:F)$ to False. Do NOT mark as expanded. |
| **Tree model property** | Every satisfiable modal formula is satisfiable in a model whose frame is a tree |
| **Loop-checking/Blocking** | A termination technique: reuse existing world labels instead of generating new ones when possible |
| **Transitive closure** | When checking satisfiability w.r.t. transitive frames: if $(x,y),(y,z)$ in Relation, add $(x,z)$ |

---

## Summary

- **LO 1 (Propositional tableaux):** A tableau systematically decomposes formulas into subformulas. Branching rules handle disjunction and implication in True, and conjunction in False. The algorithm terminates when all formulas are atomic; open = satisfiable, closed = unsatisfiable.
- **LO 2 (Tautology checking):** $F$ is valid iff $\neg F$ is unsatisfiable. Place $F$ in False and run the algorithm; validity holds iff every branch closes.
- **LO 3 (Modal tableaux):** Extend with tagged formulas $(w:F)$ and a Relation column. The eight propositional rules carry over with tagged formulas; four new rules handle $\Diamond$ and $\Box$.
- **LO 4 (Modal rules):** $(\Diamond_\text{true})$ and $(\Box_\text{false})$ introduce fresh world labels. $(\Box_\text{true})$ and $(\Diamond_\text{false})$ propagate across existing relations and must NOT be marked as expanded.
- **LO 5 (Reading off models):** From an open branch: worlds $=$ all labels in tagged formulas; $R =$ Relation column; $V(w,p)=1 \iff (w:p) \in \mathbf{True}$.
- **LO 6 (Frame-restricted satisfiability):** After each relation-adding rule, close Relation under the required frame property (e.g., add transitive edges). This ensures the extracted model belongs to the desired class.
- **LO 7 (Modal validity):** $F$ is valid w.r.t. class $\mathcal{C}$ iff place $F$ in False and every expanded branch is closed, using frame-specific closure of Relation.
- **LO 8 (Termination issues):** On transitive frames, some satisfiable formulas can cause infinite branching. Blocking/loop-checking (reusing world labels when formula sets repeat) resolves this.
