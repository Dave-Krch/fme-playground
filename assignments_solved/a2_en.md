# Assignment 2 — Formal Methods and Specification

---

## Exercise 1 — Predicate Logic Proofs

### (a) $[\forall x . P(x)] \Rightarrow [\forall x . P(f(x))]$ (example)

**Proof.** We assume $\forall x . P(x)$ and prove $\forall x . P(f(x))$.

For proving a universal quantifier, we introduce a new constant $a$ ("let $a$ be arbitrary but fixed") and prove $P(f(a))$.

From the assumption $\forall x . P(x)$, we choose the term $t = f(a)$ and conclude $P(f(a))$, which is exactly what we needed to prove. ■

---

### (b) $[\forall x . P(x) \wedge Q(x)] \Rightarrow [[\forall x . P(x)] \wedge [\forall x . Q(x)]]$

**Proof.** We assume $\forall x . P(x) \wedge Q(x)$ and prove $[\forall x . P(x)] \wedge [\forall x . Q(x)]$.

For proving a conjunction, we prove both parts separately.

**First part** — prove $\forall x . P(x)$: introduce a new constant $a$ ("let $a$ be arbitrary but fixed") and prove $P(a)$.

From the assumption $\forall x . P(x) \wedge Q(x)$, we choose $t = a$ and conclude $P(a) \wedge Q(a)$. From this conjunction we conclude $P(a)$, which finishes this part.

**Second part** — prove $\forall x . Q(x)$: again with new constant $a$, prove $Q(a)$.

From $\forall x . P(x) \wedge Q(x)$, choose $t = a$, conclude $P(a) \wedge Q(a)$, then conclude $Q(a)$. This finishes the second part, and the whole proof. ■

---

### (c) $[\exists x . P(f(x)) \vee Q(g(x))] \Rightarrow [[\exists x . P(x)] \vee [\exists x . Q(x)]]$

**Proof.** We assume $\exists x . P(f(x)) \vee Q(g(x))$ and prove $[\exists x . P(x)] \vee [\exists x . Q(x)]$.

From the assumption, whose outermost symbol is $\exists$, we choose a new constant $a$ and add $P(f(a)) \vee Q(g(a))$ as a known fact.

Now we do a case distinction on the disjunction $P(f(a)) \vee Q(g(a))$:

**Case $P(f(a))$:** We prove the goal $[\exists x . P(x)] \vee [\exists x . Q(x)]$ by the disjunction rule: assume $\neg[\exists x . P(x)]$ and prove $\exists x . Q(x)$. 

We prove a lemma: $\exists x . P(x)$ — by choosing the term $t = f(a)$ and proving $P(f(a))$, which is already known. 

Now $\exists x . P(x)$ contradicts $\neg[\exists x . P(x)]$, which is a contradiction. A contradiction proves $\exists x . Q(x)$ (our current goal). This finishes the case.

**Case $Q(g(a))$:** Symmetrically, we prove the disjunction by assuming $\neg[\exists x . Q(x)]$ and proving $\exists x . P(x)$. Lemma $\exists x . Q(x)$ (term $t = g(a)$, known $Q(g(a))$) contradicts $\neg[\exists x . Q(x)]$, proving $\exists x . P(x)$. Done.

Both cases succeed, the proof is finished. ■

---

### (d) $[\exists x . S \Rightarrow Q(x)] \Rightarrow [S \Rightarrow \exists x . Q(x)]$

**Proof.** We assume $\exists x . S \Rightarrow Q(x)$ and prove $S \Rightarrow \exists x . Q(x)$.

For proving an implication, we assume $S$ and prove $\exists x . Q(x)$.

From the assumption $\exists x . S \Rightarrow Q(x)$, we introduce a new constant $a$ such that $S \Rightarrow Q(a)$ is known.

From $S$ (known fact) and $S \Rightarrow Q(a)$, we conclude $Q(a)$.

For proving $\exists x . Q(x)$, we choose the term $t = a$ and need to prove $Q(a)$, which we already know. This finishes the proof. ■

---

### (e) $[\neg \exists x \exists y . T(x, y)] \Rightarrow [\forall x . \neg T(f(g(x)), f(x))]$

**Proof.** We assume $\neg \exists x \exists y . T(x, y)$ and prove $\forall x . \neg T(f(g(x)), f(x))$.

For proving a universal quantifier, we introduce a new constant $a$ ("let $a$ be arbitrary but fixed") and prove $\neg T(f(g(a)), f(a))$.

For proving a negation, we assume $T(f(g(a)), f(a))$ and try to find a contradiction.

From $T(f(g(a)), f(a))$, we can prove $\exists y . T(f(g(a)), y)$ by choosing $t = f(a)$ (since $T(f(g(a)), f(a))$ holds). Then we prove $\exists x \exists y . T(x, y)$ by choosing $t = f(g(a))$ for $x$ and reusing the $y = f(a)$.

But we already know $\neg \exists x \exists y . T(x, y)$ — this is a contradiction. This finishes the proof. ■

---

## Exercise 2 — Certificate for Palindrome (length 7)

### (a) Informal description (natural language)

When a string does **not** contain any palindromic substring of length 7, a certificate is a function (or array) that, for each possible starting position $i$ of a length-7 substring, provides a position $j$ within that substring (where $0 \le j \le 2$) at which the characters on opposite, symmetric ends differ. Concretely: the certificate gives, for each $i$, a witness index $j$ such that $s[i+j] \neq s[i+6-j]$. 

To check the certificate, one simply loops once over all starting positions $i$ from $0$ to $\mathrm{len}(s)-7$ and verifies that at the witness position, the two symmetric characters indeed differ. No nested loop is needed.

### (b) Formal description (predicate logic)

We extend the output specification from Assignment 1, Exercise 2d. Now the output is a pair $(r, c)$ where $r \in \{0, 1\}$ and $c$ is the certificate.

Let $\mathcal{C}$ be an array type (indexed by natural numbers, with natural number elements).

In the case $r = 0$ (no palindrome of length 7), the certificate $c$ must satisfy:

$$\forall i \in \{0, \dots, \mathrm{len}(s)-7\} . c[i] \in \{0, 1, 2\} \wedge s[i + c[i]] \neq s[i + 6 - c[i]]$$

In the case $r = 1$ (a palindrome exists), the certificate is simply the starting position:

$$c \in \{0, \dots, \mathrm{len}(s)-7\} \wedge \mathrm{palindrome}(\mathrm{substr}(s, c, \langle s[c], \dots, s[c+6] \rangle))$$

---

## Exercise 3 — Proofs in Logical Theories

### (a) In the theory of lists: $\forall l, x, y . l = \text{cons}(x, \text{cons}(y, \text{empty}())) \Rightarrow \text{first}(\text{rest}(l)) = y$

**Proof.** Let $l, x, y$ be arbitrary but fixed. We assume $l = \text{cons}(x, \text{cons}(y, \text{empty}()))$ and prove $\text{first}(\text{rest}(l)) = y$.

Using the assumption, we substitute $l$: we need to prove $\text{first}(\text{rest}(\text{cons}(x, \text{cons}(y, \text{empty}())))) = y$.

From the list axiom $\forall l, x . \text{rest}(\text{cons}(x, l)) = l$, choosing $l \leftarrow \text{cons}(y, \text{empty}()), x \leftarrow x$, we know:

$$\text{rest}(\text{cons}(x, \text{cons}(y, \text{empty}()))) = \text{cons}(y, \text{empty}())$$

So it suffices to prove $\text{first}(\text{cons}(y, \text{empty}())) = y$.

From the list axiom $\forall l, x . \text{first}(\text{cons}(x, l)) = x$, choosing $l \leftarrow \text{empty}(), x \leftarrow y$, we know $\text{first}(\text{cons}(y, \text{empty}())) = y$, which is what we wanted to prove. ■

---

### (b) In the theory of lists: $\exists l . \text{rest}(l) = \text{empty}()$

**Proof.** For proving an existential quantifier, we need to exhibit a term $t$ and prove $\text{rest}(t) = \text{empty}()$.

Choose $t = \text{cons}(0, \text{empty}())$ (or any element instead of $0$).

From the list axiom $\forall l, x . \text{rest}(\text{cons}(x, l)) = l$, choosing $l \leftarrow \text{empty}(), x \leftarrow 0$, we know $\text{rest}(\text{cons}(0, \text{empty}())) = \text{empty}()$. Done. ■

---

### (c) In the theory of arrays: $\forall a, i, j, x . \text{write}(a, i, x)[j] = x \Rightarrow [i = j \vee a[j] = x]$

**Proof.** Let $a, i, j, x$ be arbitrary but fixed. We assume $\text{write}(a, i, x)[j] = x$ and prove $i = j \vee a[j] = x$.

For proving a disjunction, we assume $\neg[i = j]$ (that is, $i \neq j$) and prove $a[j] = x$.

From the array axiom $\forall a, v, i, j . \neg[i = j] \Rightarrow \text{write}(a, i, v)[j] = a[j]$, we choose $a \leftarrow a$, $v \leftarrow x$, $i \leftarrow i$, $j \leftarrow j$ and obtain:

$$i \neq j  \Rightarrow \text{write}(a, i, x)[j] = a[j]$$

Since we are assuming $i \neq j$, we conclude $\text{write}(a, i, x)[j] = a[j]$.

Now we know both $\text{write}(a, i, x)[j] = x$ (our initial assumption) and $\text{write}(a, i, x)[j] = a[j]$. By the properties of equality (symmetry and transitivity), we conclude $a[j] = x$, which is what we wanted to prove. ■

---

### (d) Prove $\forall k . 0 + k = k$ from Peano axioms **without** the induction axiom, using induction as learned in school

**Proof.** We prove the statement by mathematical induction on $k$.

**Base case $k = 0$:** We need to prove $0 + 0 = 0$. From the Peano axiom $\forall x . x + 0 = x$, choosing $x = 0$, we obtain $0 + 0 = 0$. ✓

**Inductive step:** Assume the induction hypothesis $0 + k = k$ (for an arbitrary $k$). We prove $0 + (k+1) = k + 1$.

From the Peano axiom $\forall x, y . x + (y+1) = (x+y) + 1$, choosing $x = 0$, $y = k$:
$$0 + (k+1) = (0 + k) + 1$$

By the induction hypothesis $0 + k = k$, we substitute and obtain:
$$(0 + k) + 1 = k + 1$$

Therefore $0 + (k+1) = k + 1$. ✓

By the principle of mathematical induction, $\forall k . 0 + k = k$ holds. ■

---

### (e) Prove $\forall k . 0 + k = k$ from the Peano axioms (including induction axiom)

**Proof.** We use the Peano induction axiom. For a formula $F(k)$ with a single free variable $k$, the axiom states:

$$[F(0) \wedge [\forall k . F(k) \Rightarrow F(k+1)]] \Rightarrow \forall k . F(k)$$

We choose $F(k)$ to be $0 + k = k$. We prove the conjunction on the left-hand side.

**First conjunct — $F(0)$:** Prove $0 + 0 = 0$. From the Peano axiom $\forall x . x + 0 = x$, choosing $x = 0$, we obtain $0 + 0 = 0$. ✓

**Second conjunct — $\forall k . F(k) \Rightarrow F(k+1)$:** Let $k$ be arbitrary. Assume $F(k)$, i.e., $0 + k = k$, and prove $F(k+1)$, i.e., $0 + (k+1) = k + 1$.

From the Peano axiom $\forall x, y . x + (y+1) = (x+y) + 1$, choosing $x = 0$, $y = k$:
$$0 + (k+1) = (0 + k) + 1$$

By the induction hypothesis $0 + k = k$, we substitute:
$$(0 + k) + 1 = k + 1$$

Therefore $0 + (k+1) = k + 1$. ✓

Both conjuncts are proved. By the induction axiom, we conclude $\forall k . 0 + k = k$. ■
