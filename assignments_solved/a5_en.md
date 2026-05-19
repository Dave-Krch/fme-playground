# Assignment 5 — Formal Methods and Specification

---

## Exercise 1 — Procedure Specification

### (a) First-order formula for the specification

The procedure `p(a, r, x)` modifies the first argument `a`. The output variable for `a` is denoted $a^*$:

$$\forall a, r, x, a^* \in \mathcal{N}. [r \geq 0 \wedge p(a, a^*, r, x)] \Rightarrow a^* = a + r \cdot x$$

Here $p(a, a^*, r, x)$ is a predicate expressing that calling the procedure with arguments $(a, r, x)$ results in a new value $a^*$ for the first parameter.

### (b) Program in SSA

Original program:
```
assume x ≥ 8 ∧ k ≥ 5
p(x, 2, k)
@ x ≥ 15
```

The procedure call `p(x, 2, k)` may change $x$. In SSA, we introduce a fresh variable $x_1$ for the new value of $x$:

```
assume x ≥ 8 ∧ k ≥ 5
assume p(x, x₁, 2, k)
@ x₁ ≥ 15
```

Here $p(x, x_1, 2, k)$ is a predicate — the procedure call becomes an **assume** statement.

### (c) Verification condition

$$[x \geq 8 \wedge k \geq 5 \wedge p(x, x_1, 2, k)] \Rightarrow x_1 \geq 15$$

### (d) Does the VC hold?

**Yes.** From the procedure specification (a), substituting $a \leftarrow x$, $r \leftarrow 2$, $x \leftarrow k$, and $a^* \leftarrow x_1$, with $r = 2 \geq 0$:

$$p(x, x_1, 2, k) \Rightarrow x_1 = x + 2 \cdot k$$

From the assumptions $x \geq 8$ and $k \geq 5$:
$$x_1 = x + 2k \geq 8 + 2 \cdot 5 = 8 + 10 = 18 \geq 15$$

Thus the VC holds.

### (e) Proof

1. From the procedure formula (a), instantiate with $a \leftarrow x$, $r \leftarrow 2$, $x \leftarrow k$, $a^* \leftarrow x_1$. The input condition $r \geq 0$ is $2 \geq 0$, which is true. From $p(x, x_1, 2, k)$ we conclude $x_1 = x + 2 \cdot k$.

2. From the assumption $x \geq 8$, we have $x \geq 8$.

3. From the assumption $k \geq 5$, we have $k \geq 5$, thus $2k \geq 10$.

4. Adding: $x_1 = x + 2k \geq 8 + 10 = 18$.

5. Since $18 \geq 15$, we have $x_1 \geq 15$. ■

---

## Exercise 2 — Loop Termination

### Trace table

Input: $i = 8$, $k = 0$

$\begin{array}{c|c|c|c}
i & k & k \bmod 10 & \text{Action taken} \\ \hline
8 & 0 & 0 & \text{A: } i \leftarrow i-1,\; k \leftarrow 6 \\
7 & 6 & 6 & \text{B: } k \leftarrow k+1 \\
7 & 7 & 7 & \text{B: } k \leftarrow k+1 \\
7 & 8 & 8 & \text{B: } k \leftarrow k+1 \\
7 & 9 & 9 & \text{B: } k \leftarrow k+1 \\
7 & 10 & 0 & \text{A: } i \leftarrow i-1,\; k \leftarrow 6 \\
6 & 6 & 6 & \text{B: } k \leftarrow k+1 \\
6 & 7 & 7 & \text{B: } k \leftarrow k+1 \\
6 & 8 & 8 & \text{B: } k \leftarrow k+1 \\
6 & 9 & 9 & \text{B: } k \leftarrow k+1 \\
6 & 10 & 0 & \text{A: } i \leftarrow i-1,\; k \leftarrow 6 \\
\vdots & \vdots & \vdots & \vdots \\
1 & 10 & 0 & \text{A: } i \leftarrow 0,\; k \leftarrow 6 \\
0 & 6 & — & \text{loop exits } (i = 0)
\end{array}$

### Loop variant

**Variant:** $v(i, k) = 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10)$

**Well-founded relation:** $<$ on $\mathbb{N}$ (the standard order on non-negative integers).

**Explanation:** The variant always decreases in each iteration:

- **Branch A** ($k \bmod 10 = 0$): $i$ decreases by $1$, $k$ becomes $6$.
 $v_{old} = 10i + 0 = 10i$.
 $v_{new} = 10(i-1) + (10 - 6) = 10i - 10 + 4 = 10i - 6$. Decrease by $6$. ✓

- **Branch B** ($k \bmod 10 \in \{1,\dots,8\}$): $i$ unchanged, $k$ increases by $1$.
 $(k+1) \bmod 10 = k \bmod 10 + 1$.
 $v_{old} = 10i + (10 - n)$ where $n = k \bmod 10$.
 $v_{new} = 10i + (10 - (n+1)) = 10i + 9 - n$.
 $v_{new} = v_{old} - 1$. Decrease by $1$. ✓

- **Branch B** ($k \bmod 10 = 9$): $i$ unchanged, $k$ increases by $1$, wrapping to $0$.
 $(k+1) \bmod 10 = 0$.
 $v_{old} = 10i + (10 - 9) = 10i + 1$.
 $v_{new} = 10i + ((10 - 0) \bmod 10) = 10i + 0 = 10i$.
 $v_{new} = v_{old} - 1$. Decrease by $1$. ✓

The variant is always a non-negative integer (since $i \geq 0$, $k \geq 0$), and it decreases in every iteration. Since $<$ is well-founded on $\mathbb{N}$, the loop must terminate.

### Assertions checking the variant

We add an auxiliary variable $v$ and assertions to the program:

$$
\begin{aligned}
& \text{assume } i \geq 7 \\
& \text{assume } k \geq 0 \\
& v \leftarrow \infty \\
& \text{while } i \neq 0 \text{ do} \\
& \quad @ v > 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10) \wedge 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10) \geq 0 \\
& \quad v \leftarrow 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10) \\
& \quad \text{if } k \bmod 10 = 0 \text{ then} \\
& \quad \quad i \leftarrow i - 1;\; k \leftarrow 6 \\
& \quad \text{else} \\
& \quad \quad k \leftarrow k + 1
\end{aligned}
$$

Here $\infty$ is a constant larger than any possible value of the variant (e.g., any number larger than $10 \cdot 7 + 10 = 80$).

---

## Exercise 3 — Bounded Model Checking

### (a) Unrolling the while loop twice

Original program:
```
i ← k
while i ≤ n do
 i ← 2i
@ i ≥ k
```

After two unrollings (the `assume` bounds the execution to at most 2 iterations):

```
i ← k
if i ≤ n then
 i ← 2i
 if i ≤ n then
 i ← 2i
 assume ¬(i ≤ n)
@ i ≥ k
```

This handles $0$, $1$, or $2$ iterations. For more than $2$ iterations, the assume fails (the check is inconclusive for deeper executions).

### (b) SSA with if-then-else (no basic path decomposition)

```
i₁ ← k
γ₁ ← [i₁ ≤ n]
i₂ ← 2·i₁
i₃ ← γ₁ ? i₂ : i₁
γ₂ ← [i₃ ≤ n]
i₄ ← 2·i₃
i₅ ← γ₂ ? i₄ : i₃
assume ¬(i₅ ≤ n)
@ i₅ ≥ k
```

Explanation: $\gamma_1, \gamma_2 \in \{\top, \perp\}$ are Boolean variables capturing the loop condition. The ternary operator `? : ` selects between the updated and unchanged value. After the last `if`, the `assume` enforces that no further iteration is possible.

### (c) Logical formula

The formula encodes the SSA constraints and the final assertion:

$$[i_1 = k \wedge [\gamma_1 \Leftrightarrow (i_1 \leq n)] \wedge i_2 = 2i_1 \wedge i_3 = (\gamma_1 ? i_2 : i_1)$$

$$\wedge [\gamma_2 \Leftrightarrow (i_3 \leq n)] \wedge i_4 = 2i_3 \wedge i_5 = (\gamma_2 ? i_4 : i_3)$$

$$\wedge \neg(i_5 \leq n)] \Rightarrow i_5 \geq k$$

Where $s = \phi ? t_1 : t_2$ is shorthand for $[\phi \Rightarrow s = t_1] \wedge [\neg\phi \Rightarrow s = t_2]$.

---

## Exercise 4 — String Search

### (a) Loop invariants

The program searches for substring $s$ within string $a$. Let's define auxiliary predicates:

- $R(i) \equiv \exists r \in \{0, \dots, i-1\}. \mathrm{substr}(a, r, s)$ (substring found before position $i$)

First attempt at invariants:

**Outer loop invariant** (at `@` after `for i ← …`):

$$found \Leftrightarrow R(i)$$

This says: at the start of outer iteration $i$, $found$ is true iff a match was found at some position $< i$.

**Inner loop invariant** (at the first `@` inside inner `for`):

$$p \Leftrightarrow (\forall m \in \{0, \dots, j-1\}. a[i+m] = s[m])$$

This says: at position $i$, characters $0$ through $j-1$ match so far. $p$ is true iff the prefix of $s$ of length $j$ matches.

**After the inner `if`** (if $a[i+j] \neq s[j]$, then $p \leftarrow \perp$):

$$p \Leftrightarrow (\forall m \in \{0, \dots, j\}. a[i+m] = s[m])$$

After processing position $j$, $p$ reflects whether all checked positions match.

**After inner loop** (after `if p then found ← ⊤`):

$$found \Leftrightarrow R(i+1)$$

Since $p$ at the end of the inner loop is true iff all positions $0, \dots, \mathrm{len}(s)-1$ match (meaning $\mathrm{substr}(a, i, s)$), we update $found$ accordingly.

### (b) Number of verification conditions

The program has:
- 1 outer `for` loop with 1 invariant
- 1 inner `for` loop with 1 invariant
- 1 assertion after the inner `if`
- 1 assertion after the inner loop body
- 1 final assertion

Counting basic paths:
- Entry path to outer invariant: 1
- Jump over outer loop: 1
- Outer invariant → inner invariant (including inner loop entry): 1
- Inner invariant → after inner `if` (2 branches): 2
- After inner `if` → back to inner invariant (re-enter): 1
- After inner `if` → inner invariant → leave inner loop: 1
- After inner loop → outer invariant (re-enter): 1
- After inner loop → (no more loop) → (go through the outer for increment): combine into re-enter outer path
- Leave outer for → final: 1

Total: approximately **12 verification conditions**.

(Exact count may vary depending on how one linearizes the nested for-loops.)

### (c) Verification conditions (informal check)

Given the invariants from (a), the VCs check as follows:

1. **Enter outer for** ($found = \perp$, $i = 0$): $found \Leftrightarrow R(0)$. $R(0)$ means substring exists before position 0 — impossible. Both false. ✓

2. **Outer inv → enter inner for** ($i \leq \mathrm{len}(a)-\mathrm{len}(s)$): $p \leftarrow \top$, $j \leftarrow 0$. Inner inv: $p \Leftrightarrow (\forall m \in \{0,\dots,-1\}. \dots)$. Empty range, vacuous truth. $p = \top$, both true. ✓

3. **Inner inv, $a[i+j] = s[j]$** → after inner `if` ($p$ unchanged $\top$): after assertion $p \Leftrightarrow (\forall m \in \{0,\dots,j\}. a[i+m] = s[m])$. By inner inv, first $j$ match, and $a[i+j] = s[j]$, so all $j+1$ match. $p = \top$, both true. ✓

4. **Inner inv, $a[i+j] \neq s[j]$** → after inner `if` ($p \leftarrow \perp$): after assertion $p \Leftrightarrow (\forall m \in \{0,\dots,j\}. a[i+m] = s[m])$. Since $a[i+j] \neq s[j]$, the RHS is false, and $p = \perp$, both false. ✓

5. **After inner `if` → re-enter inner for** ($j \leftarrow j+1$, $j \leq \mathrm{len}(s)-1$): inner inv holds for the new $j$ since the assertion after inner `if` establishes the property for $j$ positions, and $j_{new} = j_{old}+1$. The invariant requires matching for $0, \dots, j_{new}-1 = j_{old}$, which was just established. ✓

6. **After inner `if` → leave inner for** ($j \leftarrow j+1$, $j = \mathrm{len}(s)$): $p$ is now true iff all $\mathrm{len}(s)$ positions match at offset $i$, i.e., $\mathrm{substr}(a, i, s)$. ✓

7. **After inner for, update $found$** → after assertion. If $p = \top$ (match found at $i$), $found \leftarrow \top$. Then $found \Leftrightarrow R(i+1)$ (since match at $i$ is included). If $p = \perp$, $found$ unchanged, and $R(i+1) = R(i)$ (no match at $i$). Both cases preserve $found \Leftrightarrow R(i+1)$. ✓

8. **Re-enter outer for** ($i \leftarrow i+1$, $i \leq \mathrm{len}(a)-\mathrm{len}(s)$): outer invariant requires $found \Leftrightarrow R(i_{new}) = R(i_{old}+1)$. The after-inner-loop assertion gives $found \Leftrightarrow R(i_{old}+1)$. ✓

9. **Leave outer for** ($i \leftarrow i+1$, $i > \mathrm{len}(a)-\mathrm{len}(s)$): $i_{new} = i+1 > \mathrm{len}(a)-\mathrm{len}(s)$, so the last checked position was at $i_{old} = \mathrm{len}(a)-\mathrm{len}(s)$. $R(\mathrm{len}(a)-\mathrm{len}(s)+1)$ covers all possible starting positions, so the final assertion $found \Leftrightarrow \exists r. \mathrm{substr}(a, r, s)$ holds. ✓
