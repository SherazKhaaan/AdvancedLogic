# Week 3 — Argument Game Proof Theories and Dialogues

## Learning Objectives

- [x] **LO1**: Identify how to use argument game proof theories to establish whether an argument is in the **grounded labelling** of an argument framework
- [x] **LO2**: Identify how to use argument game proof theories to establish whether an argument is in a **preferred labelling** of an argument framework
- [x] **LO3**: Describe how argument game proof theories can be generalised to **dialogues** for distributed non-monotonic reasoning
- [x] **LO4**: Describe the importance of these dialogues given recent advances in **Artificial Intelligence**

> **Exam scope**: Only LO1 and LO2 are examinable. LO3 and LO4 are covered in lectures but will not appear in assessed questions.

---

## 1. Recap: Argumentation Frameworks and Labellings

An **Argumentation Framework (AF)** is a pair $\langle \text{Args}, \text{Attacks} \rangle$ where $\text{Args}$ is a set of arguments and $\text{Attacks} \subseteq \text{Args} \times \text{Args}$ is a binary attack relation. We write $X \to Y$ to mean argument $X$ attacks argument $Y$.

A **labelling** is a function $\mathcal{L} : \text{Args} \to \{\text{IN}, \text{OUT}, \text{UN}\}$ satisfying:

- $\mathcal{L}(X) = \text{IN}$ iff **every** attacker of $X$ is labelled OUT
- $\mathcal{L}(X) = \text{OUT}$ iff **at least one** attacker of $X$ is labelled IN
- $\mathcal{L}(X) = \text{UN}$ iff $X$ has **no** IN attackers and **at least one** UN attacker

The **grounded labelling** is the unique labelling with the smallest possible set of IN arguments (most sceptical). The **preferred labelling(s)** are labellings with the largest possible set of IN arguments (most credulous); there may be more than one.

---

## 2. Argument Games — Basic Idea

### 2.1 Motivation

Computing labellings directly requires examining the whole framework. **Argument game proof theories** provide an alternative: to check whether a specific argument $X$ is labelled IN, we play a two-player dialogue game rather than computing the full labelling.

### 2.2 The Two Players

- **PRO** (the Proponent): argues **in favour** of the argument under question. PRO wants to show the argument wins.
- **OPP** (the Opponent): argues **against** the argument. OPP wants to show it loses.

The game is a turn-taking exchange of attack moves. PRO always moves first.

### 2.3 Game Mechanics

1. **Initial move**: PRO asserts an argument $X$ (the argument whose status is being questioned).
2. **Subsequent moves**: Each player responds to the previous move by producing an argument that **attacks** the previous argument.
3. **Losing condition**: A player **loses** a dispute if they cannot produce an attacking argument (they are stuck with no moves).
4. **Backtracking**: If a player is stuck in one branch, they may **backtrack** to a previous move and try a different attack, creating a new dispute.
5. **Winning the game**: PRO wins the overall game if PRO has a **winning strategy** — a set of disputes such that PRO wins **every** dispute in the strategy, regardless of which dispute OPP forces.

### 2.4 Terminology

| Term | Definition |
|------|-----------|
| **Game tree** | The full tree of all possible moves from the initial move, with PRO nodes and OPP nodes alternating |
| **Dispute** | A path from the root to a leaf in the game tree |
| **Winning strategy** $T'$ | A subtree of the game tree such that PRO wins every dispute in $T'$ |

> **Exam note**: When asked to write a game tree, you must write the **complete** game tree showing all possible moves, not just one path.

---

## 3. Game Rules

The game rules differ depending on whether we are testing membership in the **grounded** or **preferred** labelling.

### 3.1 Grounded Game Rules

The grounded game has **three rules**:

> **Rule G1 (Turn-taking)**: Players alternate moves. PRO moves first.
>
> **Rule G2 (Attack)**: Each move must be an argument that **attacks** the previous argument.
>
> **Rule G3 (PRO no-repeat)**: **PRO cannot repeat an argument** that PRO has already used in the **same dispute**.

The rationale for G3: in the grounded labelling (the most sceptical semantics), we are conservative about what is IN. If PRO must repeat an argument, it indicates a cycle — we do not want to count cyclic support as IN in the grounded semantics.

### 3.2 Preferred Game Rules

The preferred game has **three rules** plus an **additional condition** on the winning strategy:

> **Rule P1 (Turn-taking)**: Players alternate moves. PRO moves first.
>
> **Rule P2 (Attack)**: Each move must be an argument that **attacks** the previous argument.
>
> **Rule P3 (OPP no-repeat)**: **OPP cannot repeat an argument** that OPP has already used in the **same dispute**.

Additionally, a winning strategy $T'$ in the preferred game must satisfy:

> **Condition WS**: No two PRO arguments in $T'$ **attack each other**.

The rationale: in the preferred labelling, we want the largest consistent set of IN arguments. OPP cannot repeat because that would make the game trivially hard to win. The no-mutual-attack condition on PRO's arguments ensures the winning strategy corresponds to a consistent (conflict-free) set — exactly what preferred labellings require.

### 3.3 Summary Comparison

| Property | Grounded Game | Preferred Game |
|----------|--------------|----------------|
| PRO cannot repeat in dispute | Yes (G3) | No |
| OPP cannot repeat in dispute | No | Yes (P3) |
| Extra condition on winning strategy | None | No two PRO args in $T'$ attack each other |
| Semantics established | Grounded labelling | Preferred labelling |

---

## 4. Winning Strategies

### 4.1 Definition

A **winning strategy** for PRO is a subtree $T'$ of the full game tree such that:

1. The root of $T'$ is PRO's initial argument.
2. Every **OPP node** in $T'$ has **all** its children included (OPP can play any move).
3. Every **PRO node** in $T'$ has **at least one** child included (PRO chooses one response).
4. Every **leaf** of $T'$ is a node at which **OPP is stuck** (OPP cannot move).
5. *(For preferred game only)*: No two PRO arguments in $T'$ attack each other.

Informally: a winning strategy is a plan for PRO that guarantees a win no matter what OPP does.

### 4.2 How to Identify a Winning Strategy

1. Write the complete game tree for the game.
2. Identify all disputes (root-to-leaf paths).
3. Check which disputes PRO wins (OPP is the last to be stuck, i.e., the leaf has no OPP move).
4. Find a subtree where every OPP node's children are all included and every PRO node has at least one child, such that all leaves are won by PRO.
5. For preferred game: additionally verify no two PRO arguments in the subtree attack each other.

---

## 5. Worked Examples

### 5.1 Simple Example: $A \leftrightarrow B$ (A attacks B, B attacks A)

**AF**: $A \to B$, $B \to A$

**Grounded labelling**: Both $A$ and $B$ are UN (odd even cycle — UN/UN is the grounded labelling).

**Preferred labellings**: $\{A = \text{IN}, B = \text{OUT}\}$ and $\{A = \text{OUT}, B = \text{IN}\}$.

**Grounded game — Is A IN?**

PRO plays A. OPP must attack A, so OPP plays B. PRO must attack B and cannot repeat A (Rule G3). PRO has no other attacker of B, so PRO is stuck. PRO **loses** the dispute. There are no other disputes. PRO has **no winning strategy** → A is **not** in the grounded labelling. Consistent with A = UN.

**Preferred game — Is A IN?**

PRO plays A. OPP plays B (only attacker of A). PRO plays A (attacks B). OPP cannot repeat B (Rule P3) and has no other attacker of A, so OPP is **stuck**. PRO **wins** this dispute. The single dispute forms the winning strategy. Check no-mutual-attack: PRO only played A once, so no two PRO args attack each other. PRO has a **winning strategy** → A **is** in a preferred labelling. Consistent with A = IN in the preferred labelling $\{A=\text{IN}, B=\text{OUT}\}$.

---

### 5.2 Main Running Example: $A \leftarrow B \leftarrow C \leftarrow D$, $E \to B$, $F \to A$

**AF**:

$$D \to C \to B \to A, \quad E \to B, \quad F \to A$$

Grounded labelling (from Week 2): $D = \text{IN}$, $C = \text{OUT}$, $B = \text{IN}$, $A = \text{OUT}$, $E = \text{OUT}$, $F = \text{IN}$.

**Grounded game — Is A in the grounded labelling?**

PRO plays A. The game tree has three disputes:

- **D1**: PRO A → OPP F → PRO ? (PRO must attack F; F has no attackers) → PRO **stuck** → PRO loses D1.
- **D2**: PRO A → OPP B → PRO E (attacks B) → OPP ? (E has no attackers) → OPP **stuck** → PRO wins D2.
- **D3**: PRO A → OPP B → PRO C (attacks B, but C must attack B — wait: PRO needs to attack B so PRO plays D or E)

Let us be precise. Attackers of A: F, B. So OPP can play F or B.

- If OPP plays F: PRO must attack F. F has no attackers. PRO is **stuck** → PRO loses.
- If OPP plays B: PRO must attack B. Attackers of B: C, E. PRO can play C or E (both are separate disputes by backtracking).
  - If PRO plays E: OPP must attack E. E has no attackers. OPP **stuck** → PRO wins.
  - If PRO plays C: OPP must attack C. Attacker of C: D. OPP plays D. PRO must attack D. D has no attackers. PRO **stuck** → PRO loses.

Three disputes total:
- D1: A → F → PRO stuck. PRO **loses**.
- D2: A → B → E → OPP stuck. PRO **wins**.
- D3: A → B → C → D → PRO stuck. PRO **loses**.

For a winning strategy PRO would need to win **all** disputes in the strategy, including D1. But D1 cannot be won. **No winning strategy exists** → A is **not** in the grounded labelling. Consistent with A = OUT.

---

### 5.3 Preferred Game Example: Is D in a preferred labelling?

**AF**: $A \to B \to C \to D$, $E \to C$ (Example from 3.2 transcript)

*(Here the arrows go the other direction from the running example — read carefully from the source.)*

Actually using the example from the transcript more carefully:

**AF** (from 3.2 Video): Arguments $A, B, C, D$ with $A \to B$, $B \to C$, $C \to D$, and additional attacks.

The key result from the transcript:

**Preferred game — PRO can win by repeating A**. In a preferred game, PRO is allowed to repeat. When OPP uses an argument and PRO can counter it with A again (because P3 restricts OPP not PRO), the game can terminate in PRO's favour. Specifically if OPP has used A's attacker before in the dispute, OPP cannot use it again (P3), so OPP gets stuck.

The result: PRO has a winning strategy → argument is in a preferred labelling.

**Grounded game — Same AF**: PRO cannot repeat (G3). PRO gets stuck when forced to cycle. PRO has **no winning strategy** → argument is **not** in the grounded labelling.

---

### 5.4 Odd Loop Example (Grounded Game Fails)

Consider an AF with an odd cycle, e.g., $A \to B \to C \to A$.

In the grounded game, the game will cycle indefinitely (or PRO gets stuck due to no-repeat rule G3) and PRO cannot win. This is correct: arguments in odd cycles are not in the grounded labelling (they end up UN).

---

### 5.5 LGT Sheet Example: Triangle AF

**AF**: $A \to B$, $B \to C$, $C \to A$ (symmetric triangle — each attacks the next)

This is the LGT (Large Group Teaching) sheet example.

**Preferred labellings**: Three preferred labellings exist: $\{A=\text{IN}, B=\text{OUT}, C=\text{UN}\}$... actually for a triangle with no extra arguments:

There are three preferred labellings: $\{A=\text{IN}, B=\text{OUT}, C=\text{IN}\}$ — wait, for the triangle $A \to B \to C \to A$:
- $A=\text{IN}, B=\text{OUT}, C=\text{UN}$: C is attacked by B(OUT) and attacks A — check: C has no IN attacker (B=OUT), and B is OUT so C's only attacker is B=OUT, so C should be IN. Recalculate.

Triangle: $A \to B$, $B \to C$, $C \to A$.

Attackers: A attacked by C; B attacked by A; C attacked by B.

Labelling $\ell_1$: $A=\text{IN}, B=\text{OUT}, C=\text{IN}$: Check C=IN: attacker of C is B=OUT ✓. Check A=IN: attacker of A is C=IN — contradiction (A cannot be IN if attacker C is IN). Invalid.

Labelling: $A=\text{IN}, B=\text{OUT}, C=?$: C's attacker is B=OUT, so C=IN. But C=IN attacks A, so A should be OUT. Contradiction. No labelling with A=IN.

For the triangle, all three arguments end up UN in the grounded labelling. In preferred labellings, exactly one is IN, one is OUT, one is... let's see:

If $A=\text{IN}$: B must be OUT (A attacks B). C's attacker B=OUT, so C=IN. But C attacks A, so A must be OUT. Contradiction.

Actually for a pure 3-cycle, all arguments are UN in both grounded and all admissible labellings (stable extension doesn't exist). The grounded labelling has all UN.

**Preferred game — PRO initially plays A (from LGT sheet)**:

The complete preferred game tree (12 disputes, from LGT answers):

PRO: A

OPP can play: C (the only attacker of A)

PRO must attack C: attacker of C is B. PRO plays B.

OPP must attack B: attacker of B is A. OPP plays A.

PRO must attack A: attacker of A is C. PRO plays C.

OPP must attack C: attacker of C is B. But OPP already played A, not B — OPP can play B (hasn't repeated).

OPP plays B.

PRO must attack B: attacker of B is A. PRO plays A.

OPP must attack A: C. OPP plays C.

And so on — the game cycles.

The full game tree from the LGT answers has **12 disputes d1–d12**. The **winning strategy** consists of disputes **d3 and d9**.

**Additional efficiency rules** identified in the LGT answers:

> **Efficiency Rule 1**: PRO cannot move argument $X$ if $X$ attacks or is attacked by an argument $Y$ already moved by PRO in the same dispute.

> **Efficiency Rule 2**: OPP cannot move argument $X$ against PRO's argument $Y$ if $Y$ attacks $X$.

These rules reduce the size of the game tree without changing which arguments are winning.

---

### 5.6 SGT Sheet Example — AF1 (Week 3 Questions Q1)

**AF1**: $B \to C \leftarrow D \leftarrow G \leftarrow H$, $E \to C$

More precisely: $B$ attacks $C$; $D$ attacks $C$; $G$ attacks $D$; $H$ attacks $G$; $E$ attacks $C$.

**Step 1 — Logic program** (from solutions):

$$[b]: b$$
$$[c \leftarrow \neg b]: c$$
$$[d \leftarrow \neg c]: d$$
$$[e \leftarrow \neg d]: e \quad \text{(Wait — this is E)}$$

From the Week3Solutions.pdf:

$$[b]: b \qquad [c \leftarrow \neg b]: c \qquad [d \leftarrow \neg c]: d \qquad [e \leftarrow \neg d]: e$$
$$[g \leftarrow \neg d]: g \qquad [h \leftarrow \neg g]: h$$

Actually from the solutions file, the logic program is:

- $B = [b]: b$ — b is a fact
- $C = [c \leftarrow \neg b]: c$ — c is supported but defeated by b
- $D = [d \leftarrow \neg c]: d$ — d unless c
- $E = [b \leftarrow \neg d]: b$ — wait, E attacks C not derives b

Let me record the exact answers from the solutions:

**Grounded labelling of AF1**: $B = \text{IN}$, $C = \text{OUT}$, $D = \text{IN}$, $E = \text{OUT}$, $G = \text{OUT}$, $H = \text{IN}$

**Grounded game — Is H in the grounded labelling?** (root = H)

Game tree (from solutions):

```
PRO: H
  OPP: G
    PRO: D
      OPP: C
        PRO: B    ← OPP has no move (B has no attackers) → PRO wins
```

Single dispute: $H \to G \to D \to C \to B$, OPP stuck at B. PRO wins. **Winning strategy**: this single dispute.

H is in the grounded labelling. ✓

---

### 5.7 SGT Sheet Example — AF2 (Week 3 Questions Q2)

**AF2**: Complex framework with arguments $A, B, C, D, E, F, G, H, K$.

Sub-framework: $K \leftarrow H \leftarrow G$ (isolated chain); complex $A, B, C, D, E, F$ sub-graph.

**Grounded labelling of AF2** (from solutions):

$A = \text{UN}$, $B = \text{UN}$, $C = \text{UN}$, $D = \text{UN}$, $E = \text{UN}$, $F = \text{UN}$, $G = \text{IN}$, $H = \text{OUT}$, $K = \text{IN}$

**Two preferred labellings** exist for AF2 (details in solutions).

**Preferred game — Is F in a preferred labelling?**

Two winning strategies exist (from solutions). Each dispute in the winning strategy is itself a separate winning strategy — indicating $F$ is in at least one preferred labelling.

---

## 6. Formal Definitions (Exam-Ready)

### 6.1 Grounded Game (Formal)

Given AF $\langle \text{Args}, \text{Attacks} \rangle$ and argument $X_0 \in \text{Args}$:

A **grounded dispute** is a finite sequence $X_0, X_1, X_2, \ldots, X_n$ such that:

- **G1**: The sequence alternates between PRO moves (even indices) and OPP moves (odd indices).
- **G2**: For each $i > 0$: $X_i$ attacks $X_{i-1}$ (i.e., $(X_i, X_{i-1}) \in \text{Attacks}$).
- **G3**: No PRO argument appears twice in the sequence (PRO no-repeat).

A **grounded game tree** is the full tree of all grounded disputes rooted at $X_0$.

PRO has a **winning strategy** in the grounded game for $X_0$ iff there exists a subtree $T'$ of the game tree such that:

1. The root of $T'$ is $X_0$.
2. Every OPP node has all its children in $T'$.
3. Every PRO node has at least one child in $T'$.
4. Every leaf of $T'$ is a node where OPP has no move.

**Theorem**: $X_0$ is in the **grounded labelling** iff PRO has a winning strategy in the grounded game for $X_0$.

### 6.2 Preferred Game (Formal)

A **preferred dispute** is a finite sequence $X_0, X_1, X_2, \ldots, X_n$ such that:

- **P1**: The sequence alternates PRO/OPP with PRO first.
- **P2**: Each move attacks the previous move.
- **P3**: No OPP argument appears twice in the sequence (OPP no-repeat).

A **preferred winning strategy** $T'$ is a subtree satisfying conditions 1–4 above AND:

- **WS**: No two PRO arguments in $T'$ attack each other.

**Theorem**: $X_0$ is in **some preferred labelling** iff PRO has a winning strategy in the preferred game for $X_0$.

---

## 7. Generalisation to Dialogues (LO3 — Not Examined)

### 7.1 From Games to Dialogues

An argument game is played between two agents (PRO and OPP) with shared knowledge. A **dialogue** generalises this to multiple agents with **distributed knowledge** — each agent knows only part of the relevant information.

### 7.2 Locutions

A **locution** is a formalised utterance in a dialogue. Three types:

| Locution | Meaning |
|----------|---------|
| $\text{claim}(q)$ | Agent asserts claim $q$ |
| $\text{why}(q)$ | Agent challenges claim $q$, asking for justification |
| $\text{argue}(X, q)$ | Agent presents argument $X$ with conclusion $q$ to support or rebut |

### 7.3 Dialogue Protocols

A **dialogue protocol** specifies which locutions are permissible in response to each locution. Dialogue protocols are formal generalisations of game rules:

- Argument game rules specify legal moves (attacks) between two agents.
- Dialogue protocols specify legal locution sequences across multiple agents.

### 7.4 Locution Graphs

A **locution graph** is the directed graph of locutions exchanged during a dialogue. This graph **implicitly defines an Argumentation Framework**: arguments are the $\text{argue}(\cdot)$ locutions and attacks arise when one argument's conclusion undermines another's premise.

### 7.5 Soundness and Completeness Results

Two key theoretical results connect dialogues to non-monotonic reasoning:

> **SC1**: A claim $p$ is winning in the locution graph iff $p$ is the conclusion of a **justified argument** in the AF implicitly defined by the locution graph.

> **SC2**: Argument $X$ is a **winning argument** in the AF iff the conclusion $q$ of $X$ is **non-monotonically inferred** from the knowledge base.

> **Combined SC**: A dialogue establishes $\text{claim}(q)$ iff $q$ is a **non-monotonic inference** from all the knowledge exchanged during the dialogue.

### 7.6 Distributed Non-Monotonic Reasoning

The combined SC result has a profound implication:

- Multiple agents, each with partial knowledge, can engage in a structured dialogue.
- The dialogue protocol ensures that only relevant knowledge is exchanged.
- The conclusion of the dialogue (whatever claim is established) is exactly the non-monotonic inference that would follow from pooling all the agents' knowledge bases together.
- This is **distributed non-monotonic reasoning**: agents reason jointly without needing a central knowledge repository.

---

## 8. Applications and Value Alignment (LO4 — Not Examined)

### 8.1 The Value Alignment Problem

The **value alignment problem** is the challenge of ensuring that AI systems make decisions that are aligned with human values and preferences. As AI systems become more capable and autonomous, ensuring alignment is increasingly critical.

**Challenge**: Human values are complex, context-dependent, and often conflicting. Different people or cultures may have different values.

### 8.2 Dialogue-Based Decision Support — Holiday Example

In the lecture, a PAL (Personalised AI assistant) LLM helps a user decide between three holiday destinations: **Abadi**, **Malaga**, and **Lanzarote**.

The LLM considers multiple criteria (cost, carbon footprint, weather, personal preference). Arguments for and against each destination are constructed. The system uses non-monotonic reasoning through a dialogue to identify which destination is supported by the strongest set of consistent arguments.

**Result**: Malaga is selected as the final recommendation, based on the structured argumentation process.

**Carbon footprint comparison**: The dialogue includes arguments comparing the environmental impact of each destination (e.g., shorter flight distance reduces carbon emissions). This illustrates how quantitative reasoning can be incorporated into argumentation.

### 8.3 SocrAPPs

**SocrAPPs** (Socratic Applications) are LLM-based tools designed to conduct **Socratic dialogues** — structured question-and-answer exchanges that guide users to examine their beliefs and reach conclusions through critical reasoning.

The goal is to **inculcate critical thinking** in users rather than simply providing answers. The LLM takes the role of a Socratic interlocutor, asking probing questions.

**Example**: An abortion debate dialogue, where the SocrAPP guides the participant through a structured argumentation exchange, exposing assumptions and inconsistencies in their reasoning.

### 8.4 Connection to Argumentation Theory

SocrAPPs instantiate the dialogue theory formalised in LO3:

- The LLM and user exchange locutions following a dialogue protocol.
- The locution graph implicitly defines an AF.
- The soundness and completeness results guarantee that what is established in the dialogue corresponds to what is non-monotonically inferable from the exchanged knowledge.
- This provides a formal foundation for why Socratic dialogue is an effective reasoning tool.

---

## 9. Key Definitions

| Term | Definition |
|------|-----------|
| **Argumentation Framework (AF)** | Pair $\langle \text{Args}, \text{Attacks} \rangle$; a directed graph of arguments and attack relations |
| **Grounded labelling** | Unique labelling with smallest IN set (most sceptical); every argument with all IN attackers is IN |
| **Preferred labelling** | Labelling(s) with largest IN set (most credulous); no other labelling has strictly more IN arguments |
| **PRO (Proponent)** | Player arguing in favour of the initial argument; moves first |
| **OPP (Opponent)** | Player arguing against the initial argument; responds to PRO |
| **Game tree** | Full tree of all possible move sequences from the initial argument |
| **Dispute** | A root-to-leaf path in the game tree |
| **Winning strategy** | Subtree $T'$ of the game tree where PRO wins every dispute; PRO nodes have one child in $T'$, OPP nodes have all children in $T'$ |
| **Grounded game Rule G3** | PRO cannot repeat an argument in the same dispute |
| **Preferred game Rule P3** | OPP cannot repeat an argument in the same dispute |
| **Preferred WS condition** | No two PRO arguments in the winning strategy attack each other |
| **Locution** | A formalised utterance in a dialogue: claim$(q)$, why$(q)$, argue$(X, q)$ |
| **Locution graph** | Directed graph of locutions exchanged in a dialogue, implicitly defining an AF |
| **Dialogue protocol** | Rules specifying permissible locution responses; generalises game rules |
| **SC1** | $p$ is winning in locution graph $\Leftrightarrow$ $p$ is conclusion of justified argument in induced AF |
| **SC2** | $X$ winning in AF $\Leftrightarrow$ conclusion of $X$ is non-monotonically inferred from KB |
| **Combined SC** | Dialogue establishes claim$(q)$ $\Leftrightarrow$ $q$ is non-monotonic inference from all exchanged knowledge |
| **Distributed non-monotonic reasoning** | Multiple agents with partial knowledge pool information via dialogue to jointly derive non-monotonic inferences |
| **Value alignment problem** | Challenge of ensuring AI decisions align with human values |
| **SocrAPPs** | LLM-based applications conducting Socratic dialogues to foster critical thinking |

---

## 10. Summary

| Learning Objective | Key Insight | Examined? |
|-------------------|-------------|-----------|
| **LO1**: Grounded game | PRO has winning strategy in grounded game $\Leftrightarrow$ argument is in grounded labelling. Rule: **PRO cannot repeat** in a dispute. | **Yes** |
| **LO2**: Preferred game | PRO has winning strategy in preferred game (no mutual PRO attacks) $\Leftrightarrow$ argument is in some preferred labelling. Rule: **OPP cannot repeat** in a dispute. | **Yes** |
| **LO3**: Dialogues | Argument games generalise to multi-agent dialogues via locutions and protocols. Combined SC: dialogue establishes $q$ iff $q$ is non-monotonically inferable from exchanged knowledge. | No |
| **LO4**: AI relevance | Dialogue-based reasoning addresses the value alignment problem; SocrAPPs apply this to Socratic AI tools. | No |

### Key Distinctions to Remember

1. **Grounded vs Preferred**: In the grounded game, **PRO** cannot repeat. In the preferred game, **OPP** cannot repeat. (The more restrictive player corresponds to the more conservative/credulous semantics respectively.)

2. **Winning strategy vs single dispute**: A winning strategy is a **subtree** covering all OPP choices, not just one path. PRO must be able to respond to every possible OPP move.

3. **Preferred extra condition**: Even if PRO wins every dispute in a subtree, the subtree is only a valid preferred winning strategy if **no two PRO arguments in it attack each other**.

4. **Exam procedure**: Always write the **complete game tree** first, then identify the winning strategy (or lack thereof) as a subtree.

### Further Reading

- Modgil, S. & Caminada, M. (2009). Proof theories and algorithms for abstract argumentation frameworks. In *Argumentation in Artificial Intelligence*, Springer.
