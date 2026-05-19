# Assignment 4 — Formal Methods and Specification

---

## Exercise 1 — Multiplication by Repeated Addition

### Input/Output specification

- **Input:** $x, y \in \mathbb{N}$ such that $y \geq 1$
- **Output:** $x \cdot y$

### Filled-in program

$$
\begin{aligned}
& \text{assume } y \geq 1 \\
& r \leftarrow x \\
& i \leftarrow 1 \\
& \text{while } i < y \text{ do} \\
& \quad @ r = x \cdot i \\
& \quad r \leftarrow r + x \\
& \quad i \leftarrow i + 1 \\
& @ r = x \cdot y \\
& \text{return } r
\end{aligned}
$$

The three filled positions:
1. `assume y ≥ 1` — checks the input requirement.
2. `@ r = x · i` — the loop invariant.
3. `@ r = x · y` — checks the output requirement (term `xy` turned into formula `r = x·y`).

### Trace table

Input: $x = 3$, $y = 5$

$\begin{array}{c|c|c}
\text{Iteration} & r \text{ at invariant } @ & i \text{ at invariant } @ \\ \hline
\text{entry (after init)} & 3 & 1 \\
\text{after 1st iteration body} & 6 & 2 \\
\text{after 2nd iteration body} & 9 & 3 \\
\text{after 3rd iteration body} & 12 & 4 \\
\text{after 4th iteration body} & 15 & 5 \\
\text{(loop exits)} & 15 & 5
\end{array}$

At exit: $r = 15 = 3 \cdot 5 = x \cdot y$. ✓

### Basic paths and correctness check

**Path 1** (entering the loop):
```
assume y ≥ 1
r ← x; i ← 1
assume i < y
@ r = x·i
```
With $r = x$, $i = 1$: $r = x = x \cdot 1 = x \cdot i$. Correct. ✓

**Path 2** (skipping the loop):
```
assume y ≥ 1
r ← x; i ← 1
assume ¬(i < y)
@ r = x·y
```
$i = 1$, $\neg(1 < y)$ means $y \leq 1$. With $y \geq 1$, we get $y = 1$. $r = x = x \cdot 1 = x \cdot y$. Correct. ✓

**Path 3** (re-entering, through body back to invariant):
```
assume r = x·i
assume i < y
r ← r + x; i ← i + 1
assume i < y
@ r = x·i
```
$r_{new} = r + x = x\cdot i + x = x\cdot(i+1) = x \cdot i_{new}$. Correct. ✓

**Path 4** (leaving the loop, through body to output):
```
assume r = x·i
assume i < y
r ← r + x; i ← i + 1
assume ¬(i < y)
@ r = x·y
```
$i_{new} = i+1$. From $\neg(i_{new} < y)$ and $i < y$: $i_{new} = y$, so $i = y-1$.
$r_{new} = r + x = x\cdot(y-1) + x = x \cdot y$. Correct. ✓

All four basic paths are correct. The algorithm is correct.

---

## Exercise 2 — Array Partitioning

### (a) Is there an assertion failure?

The program implements the standard two-pointer partitioning algorithm: it scans from the left for elements $> 0$ and from the right for elements $\leq 0$, swapping misplaced elements. When the loop exits ($l > u$), the array is correctly partitioned.

**No input causes an assertion failure.** The algorithm is correct — when $l > u$, all indices $< l$ contain elements $\leq 0$ and all indices $> u$ contain elements $> 0$. Since $l > u$, the partition point is $k = l$, and $\mathrm{part}(a, n, l)$ holds.

### (b) Check each of the 12 basic paths

All paths use only `perm(ain, a, n)` as the loop invariant.

- **Path 1** (entering, assume $l \leq u$, @ inv): YES. After $a_{in} \leftarrow a$, $l \leftarrow 0$, $u \leftarrow n-1$, no mutation to $a$, so $\mathrm{perm}(a_{in}, a, n)$ holds trivially.
- **Path 2** (skipping, assume $\neg(l \leq u)$, @ final): YES. Only reachable when $0 > n-1$, i.e., $n = 0$. For an empty array, both $\mathrm{perm}$ and $\exists k. \mathrm{part}$ hold with $k = 0$.
- **Path 3** (swap executed, @ inv): YES. Swapping two elements preserves permutation.
- **Path 4** (leaving, assume $\neg(l \leq u)$, @ final): NO. From $\mathrm{perm}(a_{in}, a, n)$ and $l > u$ alone, we cannot prove $\exists k. \mathrm{part}(a, n, k)$. The invariant is too weak.
- **Paths 5, 7, 9, 11** (no swap, @ inv): YES. No mutation to $a$, so $\mathrm{perm}$ preserved.
- **Paths 6, 8, 10** (swap executed, @ inv): YES. Swapping preserves permutation.

### (c) Explicit partition point

When the loop exits ($l > u$), the partition point is $k = l$. All elements $< l$ are $\leq 0$ and all elements $\geq l$ are $> 0$.

Replace the final assertion with:
```
@ perm(ain, a, n) ∧ part(a, n, l)
```

### (d) Strengthened loop invariant

To make all 12 basic paths correct, extend the loop invariant to track partitioning progress:

```
@ perm(ain, a, n)
 ∧ (∀i ∈ {0, …, l-1}. a[i] ≤ 0)
 ∧ (∀i ∈ {u+1, …, n-1}. a[i] > 0)
 ∧ l ≤ u + 1
```

With this strengthened invariant, path 4 (leaving) works: when $\neg(l \leq u)$, i.e., $l > u$, the array is partitioned with $k = l$. All other paths are preserved because the added conditions are maintained by the program logic (incrementing $l$ past $\leq 0$ elements, decrementing $u$ past $> 0$ elements, swapping preserves the properties).

---

## Exercise 3 — Function Specification

### (a) First-order formula for the specification

Using the output-variable form:

$$\forall a \in \mathcal{A}, k \in \mathcal{N}, r \in \mathcal{N}. [k \geq 1 \wedge r = s(a, k)] \Rightarrow r = 1 + \sum_{i=0}^{k-1} a[i]$$

Or equivalently, using the function-name form:

$$\forall a \in \mathcal{A}, k \in \mathcal{N}. k \geq 1 \Rightarrow s(a, k) = 1 + \sum_{i=0}^{k-1} a[i]$$

### (b) Extended program and verification condition

Program with the function assumption:
```
assume ∀a ∈ A, k ∈ N. k ≥ 1 ⇒ s(a, k) = 1 + Σ_{i=0}^{k-1} a[i]
assume ∀i. p[i] ≥ 4
r₁ ← s(p, 3)
@ r₁ ≥ 9
```

**Verification condition:**

$$[\forall a \in \mathcal{A}, k \in \mathcal{N}. k \geq 1 \Rightarrow s(a, k) = 1 + \sum_{i=0}^{k-1} a[i] \wedge \forall i. p[i] \geq 4 \wedge r_1 = s(p, 3)] \Rightarrow r_1 \geq 9$$

### (c) Does the VC hold?

**Yes.** From the function spec with $a = p$, $k = 3$ (and $3 \geq 1$): $s(p, 3) = 1 + p[0] + p[1] + p[2]$. From $\forall i. p[i] \geq 4$: $p[0] \geq 4$, $p[1] \geq 4$, $p[2] \geq 4$. So $s(p, 3) \geq 1 + 4 + 4 + 4 = 13$. Since $r_1 = s(p, 3)$, we have $r_1 \geq 13 \geq 9$. Holds.

### (d) Proof

- From the function formula, substituting $a \leftarrow p$, $k \leftarrow 3$, and noting $3 \geq 1$, we obtain: $s(p, 3) = 1 + \sum_{i=0}^{2} p[i] = 1 + p[0] + p[1] + p[2]$.
- From $\forall i. p[i] \geq 4$, instantiating $i = 0, 1, 2$, we get: $p[0] \geq 4$, $p[1] \geq 4$, $p[2] \geq 4$.
- Summing: $s(p, 3) = 1 + p[0] + p[1] + p[2] \geq 1 + 4 + 4 + 4 = 13$.
- Since $r_1 = s(p, 3)$ (from the assignment), we have $r_1 \geq 13$.
- From $13 \geq 9$, we conclude $r_1 \geq 9$. ■

---

## Exercise 4 — Object-Oriented to Procedural Notation

Translate the dot-notation to standard function/predicate notation:

```
x ← init(0, 1)
y ← x       // y is a copy of x
x ← add(x, 7)     // x.add(7) modifies x only
y ← add(y, 1)     // y.add(1) modifies y only
assume fst(x) = snd(y)
@ snd(x) < 10
```

**Translation key:**
| OO notation | Procedural notation |
|---|---|
| `init(n)` | $\mathrm{init}(n)$ |
| `x.add(v)` | $x \leftarrow \mathrm{add}(x, v)$ |
| `x.fst()` | $\mathrm{fst}(x)$ |
| `x.snd()` | $\mathrm{snd}(x)$ |

Each method call that modifies the object produces a new value assigned back to the variable.

---

## Exercise 5 — Counting Non-Zero Elements

### (a) Which verification conditions hold?

The current program has assertions:
- $@ \top$ (loop invariant) — trivially true, too weak to be useful.
- $@ r = |\{k \in \{1, \dots, i\} \mid a[k] \neq 0\}|$ — **incorrect**. $r$ only counts negative elements (when $a[i] < 0$), not all non-zeros.
- $@ s = |\{k \in \{1, \dots, i\} \mid a[k] \neq 0\}|$ — **incorrect**. $s$ only counts positive elements (when $a[i] > 0$), not all non-zeros.
- $@ r + s = |\{k \in \{1, \dots, n\} \mid a[k] \neq 0\}|$ — would be correct if $r$ counted negatives and $s$ counted positives, but the intermediate assertions are wrong.

**VCs that fail:** The basic paths from the first invariant through the `if a[i] < 0` branch to the second assertion fail, because $r$ (which only increments for negatives) does not equal the count of all non-zeros. Similarly for the paths through `if a[i] > 0` and the third assertion.

**VCs that hold:** The re-entering path from the third assertion through the for-loop increment back to $\top$ holds trivially ($\top$ is always true). The leaving path to the final assertion would hold IF the intermediate assertions were correct.

### (b) Corrected assertions

Define:
- $R(i) = |\{k \in \{1, \dots, i\} \mid a[k] < 0\}|$ (count of negatives up to $i$)
- $S(i) = |\{k \in \{1, \dots, i\} \mid a[k] > 0\}|$ (count of positives up to $i$)

Note that $R(i) + S(i) = |\{k \in \{1, \dots, i\} \mid a[k] \neq 0\}|$.

Corrected program:

$$
\begin{aligned}
& \text{assume } r = 0 \wedge s = 0 \\
& \text{for } i \leftarrow 1 \text{ to } n \text{ do} \\
& \quad @ r = R(i-1) \wedge s = S(i-1) \\
& \quad \text{if } a[i] < 0 \text{ then } r \leftarrow r + 1 \\
& \quad @ r = R(i) \wedge s = S(i-1) \\
& \quad \text{if } a[i] > 0 \text{ then } s \leftarrow s + 1 \\
& \quad @ r = R(i) \wedge s = S(i) \\
& @ r + s = |\{k \in \{1, \dots, n\} \mid a[k] \neq 0\}|
\end{aligned}
$$

**Informal check of all VCs:**

- **Entering for-loop** ($r=0, s=0, i=1$, also need $1 \leq n$ or jump-over): $R(0) = 0, S(0) = 0$, so $r = R(0) \wedge s = S(0)$ holds. ✓
- **Top → middle** (through first `if`): if $a[i] < 0$, $r' = r+1 = R(i-1)+1 = R(i)$, $s$ unchanged $= S(i-1)$. If $a[i] \geq 0$, $r' = r = R(i-1) = R(i)$ and $s = S(i-1)$. In both cases $r = R(i) \wedge s = S(i-1)$ holds. ✓
- **Middle → bottom** (through second `if`): similar reasoning. If $a[i] > 0$, $s' = s+1 = S(i-1)+1 = S(i)$. If $a[i] \leq 0$, $s' = s = S(i-1) = S(i)$. In both cases $r = R(i) \wedge s = S(i)$ holds. ✓
- **Re-entering for** (bottom → top, after $i \leftarrow i+1$, assume $i \leq n$): $i_{new} = i+1$. $r = R(i) = R((i+1)-1) = R(i_{new}-1)$. $s = S(i) = S(i_{new}-1)$. Matches the top invariant. ✓
- **Leaving for** (bottom → final, after $i \leftarrow i+1$, assume $i > n$, i.e., $i = n+1$): $r = R(n)$, $s = S(n)$. Then $r + s = R(n) + S(n) = |\{k \in \{1, \dots, n\} \mid a[k] \neq 0\}|$. ✓
- **Jumping over for** (initial $r = s = 0$, $1 > n$, so $n = 0$): $r + s = 0 = |\{k \in \{1, \dots, 0\} \mid a[k] \neq 0\}|$. ✓

All verification conditions hold. No changes to the program itself were needed — only the assertions were corrected.
