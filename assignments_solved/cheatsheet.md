# Proof Rules, Lemmas & Logical Theories — Cheatsheet

## 1. Proof Rules (Sequent Calculus, Lecture 2)

For proving, maintain a list of **known facts** (assumptions) and a formula **to prove**. Apply rules only to the **outermost symbol**. A **contradiction** (knowing both $A$ and $\neg A$) finishes any proof successfully.

### 1.1 Simplifying What to Prove

| Outermost symbol | Rule |
|---|---|
| $A \wedge B$ | Separately prove $A$ and $B$ |
| $A \vee B$ | Assume $\neg A$ and prove $B$ (or vice versa) |
| $A \Rightarrow B$ | Assume $A$ and prove $B$ |
| $\neg A$ | Assume $A$ and try to find a contradiction |
| $\forall x. A$ | Introduce a **new** constant $a$ ("let $a$ be arbitrary but fixed"), prove $A[x \leftarrow a]$ |
| $\exists x. A$ | Choose a term $t$, prove $A[x \leftarrow t]$ |

### 1.2 Generating New Knowledge

| Outermost symbol | Rule |
|---|---|
| $A \wedge B$ | Conclude both $A$ and $B$ |
| $A \vee B$ | Case distinction: prove separately in **Case $A$** and **Case $B$** |
| $A \Rightarrow B$ | If we also know $A$, conclude $B$ |
| $\forall x. A$ | Choose a term $t$, conclude $A[x \leftarrow t]$ |
| $\exists x. A$ | Introduce a **new** constant $a$, add $A[x \leftarrow a]$ |
| $\neg A$ (known) | No direct rule — use lemmas (see below) |

### 1.3 Equality Rules

- For any term $t$, we can assume $t = t$ (reflexivity)
- If we know $t_1 = t_2$, substitute $t_1$ for $t_2$ (and vice versa)
- **Avoid name clashes:** substituting must not create new bound variables. Rename bound variables first if needed.

### 1.4 Helpful Technique: Double Negation

The equivalence $A \leftrightarrow \neg\neg A$ is explicitly allowed. Usage pattern:

1. Replace goal $A$ with $\neg\neg A$
2. Apply the rule for proving negation: assume $\neg A$, find contradiction
3. Now $\neg A$ is available as **known knowledge** — you can try to prove $A$ as a lemma

---

## 2. Equivalence Rules (Lecture Notes, Section 1.5)

Two formulas are **equivalent** ( $\Leftrightarrow$ ) if they have the same meaning. Equivalent formulas can be **replaced** by each other in any context. $A, B, C$ are placeholders for arbitrary formulas.

### 2.1 Basic Equivalences

| Rule | Description |
|---|---|
| $A \wedge \top \Leftrightarrow A$ | Identity for $\wedge$ |
| $A \wedge \perp \Leftrightarrow \perp$ | Annihilation for $\wedge$ |
| $A \vee \top \Leftrightarrow \top$ | Annihilation for $\vee$ |
| $A \vee \perp \Leftrightarrow A$ | Identity for $\vee$ |
| $A \wedge B \Leftrightarrow B \wedge A$ | Commutativity of $\wedge$ |
| $A \vee B \Leftrightarrow B \vee A$ | Commutativity of $\vee$ |
| $A \wedge [B \vee C] \Leftrightarrow [A \wedge B] \vee [A \wedge C]$ | Distributivity of $\wedge$ over $\vee$ |
| $A \vee [B \wedge C] \Leftrightarrow [A \vee B] \wedge [A \vee C]$ | Distributivity of $\vee$ over $\wedge$ |

### 2.2 Equivalences with Negation

| Rule | Description |
|---|---|
| $\neg\top \Leftrightarrow \perp$ | Negation of true |
| $\top \Leftrightarrow \neg\perp$ | True as negation of false |
| $\neg\neg A \Leftrightarrow A$ | Double negation |
| $A \wedge \neg A \Leftrightarrow \perp$ | Contradiction |
| $A \vee \neg A \Leftrightarrow \top$ | Excluded middle |
| $\neg[A \wedge B] \Leftrightarrow \neg A \vee \neg B$ | De Morgan |
| $\neg[A \vee B] \Leftrightarrow \neg A \wedge \neg B$ | De Morgan |

### 2.3 Equivalences with Quantifiers

| Rule | Condition |
|---|---|
| $\exists x \exists y. A \Leftrightarrow \exists y \exists x. A$ | |
| $\forall x \forall y. A \Leftrightarrow \forall y \forall x. A$ | |
| $\neg\forall x. A \Leftrightarrow \exists x. \neg A$ | |
| $\neg\exists x. A \Leftrightarrow \forall x. \neg A$ | |
| $\forall x. A \wedge B \Leftrightarrow [\forall x. A] \wedge [\forall x. B]$ | |
| $\exists x. A \vee B \Leftrightarrow [\exists x. A] \vee [\exists x. B]$ | |
| $\forall x. A \vee B \Leftrightarrow [\forall x. A] \vee B$ | $B$ does not contain $x$ |
| $\exists x. A \wedge B \Leftrightarrow [\exists x. A] \wedge B$ | $B$ does not contain $x$ |
| $\exists x. P \Leftrightarrow P$ | $x$ does not occur freely in $P$ |
| $\forall x. P \Leftrightarrow P$ | $x$ does not occur freely in $P$ |
| $A \Leftrightarrow B$ | $A$ and $B$ are the same up to renaming of bound variables ($\alpha$-conversion) |

### 2.4 Equivalence as a Relation

| Rule | Description |
|---|---|
| $A \Leftrightarrow A$ | Reflexivity |
| If $A \Leftrightarrow B$, then $B \Leftrightarrow A$ | Symmetry |
| If $A \Leftrightarrow B$ and $B \Leftrightarrow C$, then $A \Leftrightarrow C$ | Transitivity |

### 2.5 Implication and Equivalence as Shortcuts

**Definitions:**
- $A \Rightarrow B$ is a shortcut for $\neg A \vee B$
- $A \Leftrightarrow B$ is a shortcut for $[A \Rightarrow B] \wedge [B \Rightarrow A]$

**Consequences:**

| Rule | Description |
|---|---|
| $A \Rightarrow \top \Leftrightarrow \top$ | Implication to true |
| $\perp \Rightarrow B \Leftrightarrow \top$ | From false, anything follows |
| $[A \wedge B] \Rightarrow A \Leftrightarrow \top$ | Simplification |
| $A \Rightarrow [A \vee B] \Leftrightarrow \top$ | Weakening |
| $A \Rightarrow B \Leftrightarrow \neg B \Rightarrow \neg A$ | Contraposition |
| $A \Leftrightarrow B \Leftrightarrow \neg B \Leftrightarrow \neg A$ | Contraposition of $\Leftrightarrow$ |

### 2.6 Case Distinction

- $A \Leftrightarrow [B \Rightarrow A] \wedge [\neg B \Rightarrow A]$
- $A \Leftrightarrow [B_1 \Rightarrow A] \wedge \dots \wedge [B_n \Rightarrow A]$, if $B_1 \vee \dots \vee B_n \Leftrightarrow \top$

---

## 3. Lemmas

A **lemma** is a separate proof of an intermediate formula, performed using the currently known facts. Once proved, the lemma's conclusion is added to the known facts of the main proof.

### 2.1 When to Use Lemmas

| Situation | Strategy |
|---|---|
| We know $\neg A$ | Prove $A$ as a lemma → contradiction finishes the proof |
| We know $A \Rightarrow B$ | Prove $A$ as a lemma → from $A$ and $A \Rightarrow B$, conclude $B$ |
| We need case split | Lemma: $A \vee \neg A$ (always provable without assumptions) |
| We need an existential witness | Prove existence of a specific witness as a lemma |
| We know $\neg\forall x. A$ | Prove $\forall x. A$ as a lemma → contradiction |

### 2.2 Lemma Rules

- A lemma has its **own proof** — it does not affect the main proof except for its conclusion
- A lemma can contain **nested sub-lemmas**
- In a lemma, all previously known facts from the enclosing proof are available
- A contradiction inside a lemma finishes **that lemma** (not the enclosing proof)

---

## 4. Logical Theories

A **logical theory** = **signature** (types + function/predicate symbols) + **axioms**. The axioms are assumed as known facts when proving within the theory.

### 3.1 Pairs $\mathcal{P}[\mathcal{T}]$

**Signature:** $\mathrm{pair}: \mathcal{T} \times \mathcal{T} \to \mathcal{P}[\mathcal{T}]$, $\mathrm{fst}: \mathcal{P}[\mathcal{T}] \to \mathcal{T}$, $\mathrm{snd}: \mathcal{P}[\mathcal{T}] \to \mathcal{T}$

**Axioms:**
- $\forall x, y. \mathrm{fst}(\mathrm{pair}(x, y)) = x$
- $\forall x, y. \mathrm{snd}(\mathrm{pair}(x, y)) = y$
- $\forall p. \mathrm{pair}(\mathrm{fst}(p), \mathrm{snd}(p)) = p$

### 3.2 Lists $\mathcal{L}[\mathcal{T}]$

**Signature:** $\mathrm{cons}: \mathcal{T} \times \mathcal{L}[\mathcal{T}] \to \mathcal{L}[\mathcal{T}]$, $\mathrm{first}: \mathcal{L}[\mathcal{T}] \to \mathcal{T}$, $\mathrm{rest}: \mathcal{L}[\mathcal{T}] \to \mathcal{L}[\mathcal{T}]$, $\mathrm{empty}: \to \mathcal{L}[\mathcal{T}]$

**Abbreviation:** $\langle x_1, \dots, x_n \rangle$ means $\mathrm{cons}(x_1, \mathrm{cons}(x_2, \dots, \mathrm{cons}(x_n, \mathrm{empty}())))$

**Axioms:**
- $\forall l, x. \mathrm{first}(\mathrm{cons}(x, l)) = x$
- $\forall l, x. \mathrm{rest}(\mathrm{cons}(x, l)) = l$
- $\forall l. \neg[l = \mathrm{empty}()] \Rightarrow \mathrm{cons}(\mathrm{first}(l), \mathrm{rest}(l)) = l$
- $\forall l, x. \neg[\mathrm{cons}(x, l) = \mathrm{empty}()]$

> $\mathrm{first}(\mathrm{empty}())$ and $\mathrm{rest}(\mathrm{empty}())$ are **unspecified**.

### 3.3 Arrays $\mathcal{A}[\mathcal{T}]$

**Signature:** $a[i]$ (read), $\mathrm{write}: \mathcal{A}[\mathcal{T}] \times \mathcal{N} \times \mathcal{T} \to \mathcal{A}[\mathcal{T}]$ (write)

> $\mathrm{write}(a, i, v)$ creates a **new** array (functional style — no mutation).

**Axioms:**
- **(RS)** $\forall a, v, i, j. i = j \Rightarrow \mathrm{write}(a, i, v)[j] = v$
- **(RD)** $\forall a, v, i, j. i \neq j \Rightarrow \mathrm{write}(a, i, v)[j] = a[j]$
- **(Equality)** $\forall a, b. [\forall i. a[i] = b[i]] \Leftrightarrow a = b$

> The equality axiom is crucial: to prove $a = b$, instead prove $\forall i. a[i] = b[i]$.

### 3.4 Natural Numbers $\mathcal{N}$ (Peano Arithmetic)

**Axioms:**
- $\forall x. \neg[x + 1 = 0]$
- $\forall x, y. x + 1 = y + 1 \Rightarrow x = y$
- $\forall x. x + 0 = x$
- $\forall x, y. x + (y + 1) = (x + y) + 1$
- $\forall x. x \cdot 0 = 0$
- $\forall x, y. x \cdot (y + 1) = x \cdot y + x$

**Induction axiom** (for each formula $F$ with one free variable):

$$[F[x \leftarrow 0] \wedge [\forall x. F \Rightarrow F[x \leftarrow x + 1]]] \Rightarrow \forall x. F$$

**Defined helpers:**
- $\forall x, y. x \leq y\ :\Leftrightarrow\ \exists k. x + k = y$
- $2, 3, 4, \dots$ are abbreviations for $1+1, 1+1+1, \dots$

> Peano arithmetic is **undecidable** (Gödel). Presburger arithmetic (without multiplication) is decidable.

### 3.5 Set Theory (Basic Rules)

**Set construction:** $\{x \mid A\}$ where $A$ is a formula.

**Operations:** $\cap, \cup, \setminus, \times$ (Cartesian product), $\mathcal{P}$ (power set)

**Key equivalences:**
- $S \subseteq T \ \Leftrightarrow\ \forall x. x \in S \Rightarrow x \in T$
- $S = T \ \Leftrightarrow\ \forall x. x \in S \Leftrightarrow x \in T$
- $a \in \{x \mid A\} \ \Leftrightarrow\ A[x \leftarrow a]$

**Notation shortcuts:**
- $\{a\}$ for $\{x \mid x = a\}$
- $\forall x \in S. A$ for $\forall x. x \in S \Rightarrow A$
- $\exists x \in S. A$ for $\exists x. x \in S \wedge A$
- $x \notin A$ for $\neg[x \in A]$

---

## 5. Proof Strategy Tips

1. **Order matters:** apply rules that introduce new constants (universal-to-prove, existential-in-knowledge) **first**, before rules that require choosing terms (existential-to-prove, universal-in-knowledge)
2. **Stuck with $\neg A$ as knowledge?** Prove $A$ as a lemma to get a contradiction
3. **Need a term for $\exists$?** Look at your known facts for suitable terms
4. **Array equality?** Use the third array axiom: prove element-wise equality instead
5. **Case split?** Lemma $A \vee \neg A$ is always available
6. **Substitution safety:** always rename bound variables to avoid name clashes before substituting
