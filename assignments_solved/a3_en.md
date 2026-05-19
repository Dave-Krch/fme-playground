# Assignment 3 — Formal Methods and Specification

---

## Exercise 1 — Proofs in the Theory of Arrays

### (a) $\forall x \exists a . a[0] = x$ (example)

**Proof.** Let $x$ be arbitrary but fixed. We prove $\exists a . a[0] = x$.

For an existential quantifier, we need to choose a term for $a$ and prove the resulting formula. Choose $a = \text{write}(b, 0, x)$ where $b$ is an arbitrary array.

We need to prove $\text{write}(b, 0, x)[0] = x$.

From the array axiom $\forall a, v, i, j . i = j \Rightarrow \text{write}(a, i, v)[j] = v$, choose $a \leftarrow b$, $v \leftarrow x$, $i \leftarrow 0$, $j \leftarrow 0$. We obtain:

$$0 = 0  \Rightarrow \text{write}(b, 0, x)[0] = x$$

Since $0 = 0$ holds (by reflexivity of equality), we conclude $\text{write}(b, 0, x)[0] = x$. Done. ■

---

### (b) $\forall i \exists a . a[i] = 2a[i + 1]$

**Proof.** Let $i$ be arbitrary but fixed. We prove $\exists a . a[i] = 2a[i + 1]$.

We need to construct an array $a$ that satisfies this. Choose:
$$a = \text{write}(\text{write}(b, i, 2c), i+1, c)$$
where $b$ is any array and $c$ is any integer (e.g., $c = 0$). This array has $2c$ at index $i$ and $c$ at index $i+1$.

Now we prove $a[i] = 2a[i+1]$.

**Left side — $a[i]$:**
$$a[i] = \text{write}(\text{write}(b, i, 2c), i+1, c)[i]$$

Since $i \neq i+1$, from the array axiom for reading at a different index:
$$\text{write}(\text{write}(b, i, 2c), i+1, c)[i] = \text{write}(b, i, 2c)[i]$$

Since $i = i$, from the axiom for reading at the same index:
$$\text{write}(b, i, 2c)[i] = 2c$$

So $a[i] = 2c$.

**Right side — $2a[i+1]$:**
$$a[i+1] = \text{write}(\text{write}(b, i, 2c), i+1, c)[i+1]$$

Since $i+1 = i+1$, from the axiom for reading at the same index:
$$\text{write}(\text{write}(b, i, 2c), i+1, c)[i+1] = c$$

So $a[i+1] = c$, and $2a[i+1] = 2c$.

Therefore $a[i] = 2c = 2a[i+1]$. Done. ■

---

### (c) $\exists a, i, x . \text{write}(a, i, x)[0] = x + 3$

**Proof.** For proving the triple existential quantifier, we need to choose concrete values for $a$, $i$, and $x$.

Choose:
- $i = 1$ (or any $i \neq 0$)
- $x$: any integer
- $a = \text{write}(b, 0, x+3)$ where $b$ is any array

Now we prove $\text{write}(\text{write}(b, 0, x+3), 1, x)[0] = x + 3$.

Since $1 \neq 0$, from the array axiom for reading at a different index:
$$\text{write}(\text{write}(b, 0, x+3), 1, x)[0] = \text{write}(b, 0, x+3)[0]$$

Since $0 = 0$, from the axiom for reading at the same index:
$$\text{write}(b, 0, x+3)[0] = x + 3$$

Therefore $\text{write}(a, i, x)[0] = x + 3$. Done. ■

---

## Exercise 2 — SSA and Verification Conditions

### (a) **assume** $x \geq 0$ ; $y \leftarrow x$ ; $z \leftarrow y$ ; $@ z \geq 0$

**SSA:** The program already has each variable assigned at most once, so no renaming is needed (each variable gets a fresh name after assignment). In SSA form:

$$\text{assume } x \geq 0; y_1 \leftarrow x; z_1 \leftarrow y_1; @ z_1 \geq 0$$

**Verification condition:**
$$[x \geq 0 \wedge y_1 = x \wedge z_1 = y_1] \Rightarrow z_1 \geq 0$$

**Check:** From the assumptions, $z_1 = y_1 = x$ and $x \geq 0$, therefore $z_1 \geq 0$. **The VC holds.** ■

---

### (b) **assume** $x \geq 0$ ; $z \leftarrow y$ ; $y \leftarrow x$ ; $@ z \geq 0$

**SSA:** $z$ is assigned first, then $y$. No variable name clash for the first assignment, so:

$$\text{assume } x \geq 0; z_1 \leftarrow y; y_1 \leftarrow x; @ z_1 \geq 0$$

**Verification condition:**
$$[x \geq 0 \wedge z_1 = y \wedge y_1 = x] \Rightarrow z_1 \geq 0$$

**Check:** The VC does NOT hold. The variable $y$ (initial value of $y$, now $y_1$) is unconstrained — it could be any integer, and $z_1 = y$. Nothing forces $z_1 \geq 0$.

**Counter-example:** $\{x \mapsto 5, y \mapsto -3, z_1 \mapsto -3, y_1 \mapsto 5\}$

The assumptions $5 \geq 0$, $-3 = -3$, and $5 = 5$ hold, but $z_1 = -3 \not\geq 0$. This corresponds to input $x = 5, y = -3$ — the program would fail the assertion. ■

---

### (c) $x \leftarrow x - k$ ; **assume** $k \leq x$ ; $@ x \geq 0$

**SSA:** $x$ is overwritten, so we rename all subsequent occurrences:

$$x_1 \leftarrow x - k; \text{assume } k \leq x_1; @ x_1 \geq 0$$

**Verification condition:**
$$[x_1 = x - k \wedge k \leq x_1] \Rightarrow x_1 \geq 0$$

**Check:** The VC does NOT hold. From $x_1 = x - k$ and $k \leq x_1$, we know $k \leq x - k$, i.e., $2k \leq x$. But $x_1$ could still be negative. For example, with $k = -5$ and $x = -8$:

$x_1 = -8 - (-5) = -3$, and $k \leq x_1$ is $-5 \leq -3$ (true), but $x_1 = -3 \not\geq 0$.

**Counter-example:** $\{x \mapsto -8, k \mapsto -5, x_1 \mapsto -3\}$ ■

---

### (d) **assume** $x \geq 0$ ; $y \leftarrow x$ ; **input** $x$ ; $@ x \geq 0$

**SSA:** The **input** creates a new value for $x$, so we rename subsequent $x$:

$$\text{assume } x \geq 0; y_1 \leftarrow x; \text{input } x_1; @ x_1 \geq 0$$

**Verification condition:**
$$[x \geq 0 \wedge y_1 = x] \Rightarrow x_1 \geq 0$$

**Check:** The VC does NOT hold. $x_1$ comes from user input and is unrelated to the assumptions about $x$ and $y_1$. Any negative $x_1$ would violate the assertion.

**Counter-example:** $\{x \mapsto 10, y_1 \mapsto 10, x_1 \mapsto -4\}$ — all assumptions hold, but $x_1 = -4 \not\geq 0$. ■

---

### (e) $y \leftarrow x$ ; **input** $x$ ; **assume** $x \geq 0$ ; $@ y \geq 0$

**SSA:** Both $y$ and $x$ receive new values:

$$y_1 \leftarrow x; \text{input } x_1; \text{assume } x_1 \geq 0; @ y_1 \geq 0$$

**Verification condition:**
$$[y_1 = x \wedge x_1 \geq 0] \Rightarrow y_1 \geq 0$$

**Check:** The VC does NOT hold. $y_1 = x$ (the original value), which is unconstrained. $x_1 \geq 0$ says nothing about the original $x$.

**Counter-example:** $\{x \mapsto -7, y_1 \mapsto -7, x_1 \mapsto 3\}$ — $x_1 = 3 \geq 0$ holds, but $y_1 = -7 \not\geq 0$. ■

---

### (f) $a[i] \leftarrow 7$ ; $i \leftarrow i + 1$ ; $a[i] \leftarrow 8$ ; $@ i \geq a[i]$

**SSA:** Array assignments use the $\text{write}$ function. Each assignment creates a new array version:

$$a_1 \leftarrow \text{write}(a, i, 7); i_1 \leftarrow i + 1; a_2 \leftarrow \text{write}(a_1, i_1, 8); @ i_1 \geq a_2[i_1]$$

**Verification condition:**
$$[a_1 = \text{write}(a, i, 7) \wedge i_1 = i + 1 \wedge a_2 = \text{write}(a_1, i_1, 8)] \Rightarrow i_1 \geq a_2[i_1]$$

**Check:** The VC does NOT hold. From $a_2 = \text{write}(a_1, i_1, 8)$ and the array axiom for reading at the same index, we get $a_2[i_1] = 8$. The VC requires $i_1 \geq 8$, i.e., $i+1 \geq 8$, but $i$ is unconstrained.

**Counter-example:** $\{i \mapsto 0, i_1 \mapsto 1, a_2[i_1] \mapsto 8\}$ — all assumptions hold, but $1 \not\geq 8$. ■

---

## Exercise 3 — Loop Program (Sum of Array Elements)

Program:

$$
\begin{aligned}
& x_0 \leftarrow x \\
& i \leftarrow 0 \\
& \text{while } i < n \text{ do} \\
& \quad @ x = x_0 + \sum_{k=0}^{i-1} a[k] \\
& \quad x \leftarrow x + a[i] \\
& \quad i \leftarrow i + 1 \\
& @ x = x_0 + \sum_{k=0}^{n-1} a[k]
\end{aligned}
$$

All variables range over $\mathbb{N}$ (including zero). The empty sum $\sum_{k=0}^{-1} \dots = 0$.

### (a) Basic paths

The loop invariant is `@ I` where $I \equiv x = x_0 + \sum_{k=0}^{i-1} a[k]$.

The final assertion is $F \equiv x = x_0 + \sum_{k=0}^{n-1} a[k]$.

Four basic paths:

**Path 1** (entering the loop from the start):

$$
\begin{aligned}
& x_0 \leftarrow x \\
& i \leftarrow 0 \\
& \text{assume } i < n \\
& @ I
\end{aligned}
$$

**Path 2** (skipping the loop entirely — $i < n$ is false initially):

$$
\begin{aligned}
& x_0 \leftarrow x \\
& i \leftarrow 0 \\
& \text{assume } \neg(i < n) \\
& @ F
\end{aligned}
$$

**Path 3** (re-entering the loop — from invariant through body back to invariant):

$$
\begin{aligned}
& \text{assume } I \\
& \text{assume } i < n \\
& x \leftarrow x + a[i] \\
& i \leftarrow i + 1 \\
& @ I
\end{aligned}
$$

**Path 4** (leaving the loop — from invariant through body to final assertion):

$$
\begin{aligned}
& \text{assume } I \\
& \text{assume } i < n \\
& x \leftarrow x + a[i] \\
& i \leftarrow i + 1 \\
& \text{assume } \neg(i < n) \\
& @ F
\end{aligned}
$$

---

### (b) Verification conditions

**Path 1 — SSA:**
$$x_{01} \leftarrow x; i_1 \leftarrow 0; \text{assume } i_1 < n; @ x = x_{01} + \sum_{k=0}^{i_1-1} a[k]$$

$$VC_1: [x_{01} = x \wedge i_1 = 0 \wedge i_1 < n]  \Rightarrow x = x_{01} + \sum_{k=0}^{i_1-1} a[k]$$

With $i_1 = 0$, the sum is $\sum_{k=0}^{-1} a[k] = 0$. With $x_{01} = x$, the right side is $x + 0 = x$. So $VC_1$ simplifies to $x = x$, which is trivially true.

**Path 2 — SSA:**
$$x_{01} \leftarrow x; i_1 \leftarrow 0; \text{assume } \neg(i_1 < n); @ x = x_{01} + \sum_{k=0}^{n-1} a[k]$$

$$VC_2: [x_{01} = x \wedge i_1 = 0 \wedge i_1 \geq n]  \Rightarrow x = x_{01} + \sum_{k=0}^{n-1} a[k]$$

$i_1 = 0$ and $i_1 \geq n$ implies $n = 0$ (since $n \in \mathbb{N}$). The sum $\sum_{k=0}^{-1} a[k] = 0$, and $x = x_{01} + 0 = x$. Holds.

**Path 3 — SSA:**
$$\text{assume } x = x_0 + \sum_{k=0}^{i-1} a[k]; \text{assume } i < n; x_1 \leftarrow x + a[i]; i_1 \leftarrow i + 1; @ x_1 = x_0 + \sum_{k=0}^{i_1-1} a[k]$$

$$VC_3: \left[ x = x_0 + \sum_{k=0}^{i-1} a[k]  \wedge i < n  \wedge x_1 = x + a[i]  \wedge i_1 = i + 1 \right]  \Rightarrow x_1 = x_0 + \sum_{k=0}^{i_1-1} a[k]$$

$x_1 = x + a[i] = x_0 + \sum_{k=0}^{i-1} a[k] + a[i]$. The sum $\sum_{k=0}^{i_1-1} a[k] = \sum_{k=0}^{i} a[k] = \sum_{k=0}^{i-1} a[k] + a[i]$.
Thus $x_0 + \sum_{k=0}^{i_1-1} a[k] = x_0 + \sum_{k=0}^{i-1} a[k] + a[i] = x_1$. Holds.

**Path 4 — SSA:**
$$\text{assume } x = x_0 + \sum_{k=0}^{i-1} a[k]; \text{assume } i < n; x_1 \leftarrow x + a[i]; i_1 \leftarrow i + 1; \text{assume } \neg(i_1 < n); @ x_1 = x_0 + \sum_{k=0}^{n-1} a[k]$$

$$VC_4: \left[ x = x_0 + \sum_{k=0}^{i-1} a[k]  \wedge i < n  \wedge x_1 = x + a[i]  \wedge i_1 = i + 1  \wedge i_1 \geq n \right]  \Rightarrow x_1 = x_0 + \sum_{k=0}^{n-1} a[k]$$

From $i < n$ and $i+1 = i_1 \geq n$, we get $i_1 = n$ (since integers). Thus $i = n-1$.

$x_1 = x + a[n-1] = x_0 + \sum_{k=0}^{n-2} a[k] + a[n-1] = x_0 + \sum_{k=0}^{n-1} a[k]$. Holds.

---

### (c) All four verification conditions hold.

### (d) Not needed — all VCs already hold. No modification required.

---

## Exercise 4 — For-Loop Program

$$
\begin{aligned}
& \text{assume } n \neq 0 \\
& k \leftarrow n \\
& \text{if } n < 0 \text{ then} \\
& \quad l \leftarrow 1 \\
& \text{else} \\
& \quad l \leftarrow -1 \\
& \text{for } i \leftarrow 0 \text{ to } |n| - 1 \text{ do} \\
& \quad @ k = n + li \wedge l = -\mathrm{sgn}(n) \\
& \quad k \leftarrow k + l \\
& \quad @ k = n + l(i+1) \wedge l = -\mathrm{sgn}(n) \\
& @ k = 0
\end{aligned}
$$

All variables range over $\mathbb{Z}$. $\text{sgn}(n) = 1$ if $n > 0$, $\text{sgn}(n) = -1$ if $n < 0$.

### (a) Basic paths

The for-loop linearization (Lecture 5) gives the following paths. Let:
- $I_1 \equiv k = n + li \wedge l = -\text{sgn}(n)$ (first invariant)
- $I_2 \equiv k = n + l(i+1) \wedge l = -\text{sgn}(n)$ (second invariant)
- $F \equiv k = 0$ (final assertion)

The for loop `for i ← 0 to |n|-1`:
- Enter from outside: assume $i = 0 \wedge 0 \leq |n|-1$
- Jump over: assume $\neg(0 \leq |n|-1)$
- Re-enter: $i \leftarrow i + 1$; assume $i \leq |n|-1$
- Leave: $i \leftarrow i + 1$; assume $\neg(i \leq |n|-1)$
- Additionally, inside the loop body we may assume $0 \leq i \leq |n|-1$.

**Path 1** (start via `if` branch $n < 0$ to first loop invariant):

$$
\begin{aligned}
& \text{assume } n \neq 0 \\
& k \leftarrow n \\
& \text{assume } n < 0 \\
& l \leftarrow 1 \\
& \text{assume } i = 0 \wedge 0 \leq |n| - 1 \\
& @ I_1
\end{aligned}
$$

**Path 2** (start via `else` branch $n > 0$ to first loop invariant):

$$
\begin{aligned}
& \text{assume } n \neq 0 \\
& k \leftarrow n \\
& \text{assume } \neg(n < 0) \\
& l \leftarrow -1 \\
& \text{assume } i = 0 \wedge 0 \leq |n| - 1 \\
& @ I_1
\end{aligned}
$$

**Path 3** (start via `if` branch, jumping over the for loop):

$$
\begin{aligned}
& \text{assume } n \neq 0 \\
& k \leftarrow n \\
& \text{assume } n < 0 \\
& l \leftarrow 1 \\
& \text{assume } \neg(0 \leq |n| - 1) \\
& @ F
\end{aligned}
$$

**Path 4** (start via `else` branch, jumping over the for loop):

$$
\begin{aligned}
& \text{assume } n \neq 0 \\
& k \leftarrow n \\
& \text{assume } \neg(n < 0) \\
& l \leftarrow -1 \\
& \text{assume } \neg(0 \leq |n| - 1) \\
& @ F
\end{aligned}
$$

**Path 5** (loop body: from first invariant to second invariant):

$$
\begin{aligned}
& \text{assume } 0 \leq i \leq |n| - 1 \wedge I_1 \\
& k \leftarrow k + l \\
& @ I_2
\end{aligned}
$$

**Path 6** (re-entering for loop from second invariant):

$$
\begin{aligned}
& \text{assume } 0 \leq i \leq |n| - 1 \wedge I_2 \\
& i \leftarrow i + 1 \\
& \text{assume } i \leq |n| - 1 \\
& @ I_1
\end{aligned}
$$

**Path 7** (leaving for loop from second invariant):

$$
\begin{aligned}
& \text{assume } 0 \leq i \leq |n| - 1 \wedge I_2 \\
& i \leftarrow i + 1 \\
& \text{assume } \neg(i \leq |n| - 1) \\
& @ F
\end{aligned}
$$

---

### (b) Correctness check

**Path 1:** $n < 0$, so $\text{sgn}(n) = -1$, $-\text{sgn}(n) = 1 = l$. $k = n$, $i = 0$.
$I_1$: $k = n + l \cdot 0 = n$. $l = 1 = -\text{sgn}(n)$. Both hold. ✓

**Path 2:** $n > 0$ (from $\neg(n<0)$ and $n \neq 0$), so $\text{sgn}(n) = 1$, $l = -1 = -\text{sgn}(n)$. $k = n$, $i = 0$.
$I_1$: $k = n + (-1) \cdot 0 = n$. $l = -1 = -\text{sgn}(n)$. Both hold. ✓

**Path 3:** $n < 0$, $l = 1$. $\neg(0 \leq |n|-1)$ means $|n|-1 < 0$, so $|n| = 0$, $n = 0$. But we have $n \neq 0$ — contradiction. The path is **vacuously correct** (unreachable). ✓

**Path 4:** $n > 0$, $l = -1$. $\neg(0 \leq |n|-1)$ means $|n| = 0$, so $n = 0$ — contradiction with $n \neq 0$. **Vacuously correct.** ✓

**Path 5:** $k = n + li$, $k \leftarrow k + l$. New $k_1 = n + li + l = n + l(i+1)$. $l$ unchanged. $I_2$ holds. ✓

**Path 6:** $k = n + l(i+1)$. $i \leftarrow i + 1$, so $i_1 = i + 1$. Assume $i_1 \leq |n|-1$ (re-entry check, satisfied). $I_1$ requires $k = n + l \cdot i_1$. We have $k = n + l(i+1) = n + l \cdot i_1$. And $l = -\text{sgn}(n)$. Both hold. ✓

**Path 7:** $k = n + l(i+1)$. $i \leftarrow i+1$, $i_1 = i+1$. Assume $i_1 > |n|-1$ (leave check). From loop body assumption $0 \leq i \leq |n|-1$, we have $i_1 = i+1 \leq |n|$. With $i_1 > |n|-1$, we get $i_1 = |n|$.

$F$ requires $k = 0$:
$$k = n + l \cdot |n| = n + (-\text{sgn}(n)) \cdot |n| = n - \text{sgn}(n) \cdot |n|$$

Since $\text{sgn}(n) \cdot |n| = n$ (by definition of sign and absolute value, with $n \neq 0$):
$$k = n - n = 0 \quad \checkmark$$

All basic paths are correct. ■
