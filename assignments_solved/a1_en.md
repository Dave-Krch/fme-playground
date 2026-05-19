# Assignment 1 — Formal Methods and Specification

---

## Exercise 1 — A Story of Imprecise Specification

A few years ago, a colleague was working on an internal tool for a logistics company. The ticket read:

> *"When the delivery status is 'in transit', send a notification to the customer every hour."*

He implemented it literally: a cron job that ran every 60 minutes and sent a push notification. The system went live.

At 3 AM, the operations team received angry calls. Customers were being woken up by hourly notifications at night, for deliveries that were still days away. Worse, the deliveries that had been "in transit" for over two weeks (stuck at customs) had accumulated over 300 unread notifications per customer.

The specification had not said anything about:
- time-of-day restrictions,
- maximum number of messages,
- or what happens when "in transit" lasts several weeks.

The fix required an emergency deployment, but the damage was done: thousands of users uninstalled the app. All because of **one missing sentence** in the specification.

---

## Exercise 2 — Input/Output Specifications

### Auxiliary definitions

We use the following types:
- $\mathbb{N}$ — natural numbers (including zero)
- $\mathbb{Z}$ — integers
- $\mathbb{N}^+$ — positive natural numbers $\{1, 2, 3, \dots\}$
- $\mathcal{A}$ — arrays with integer elements
- $\mathcal{S}$ — character strings

We write $a[i]$ for reading the $i$-th element of array $a$ (0-indexed), and $\mathrm{len}(a)$ for the length of an array or string $a$.

---

**Definition of a prime number:**

$$\forall p \in \mathbb{N}. \mathrm{prime}(p) :\Leftrightarrow p > 1 \wedge \forall d \in \mathbb{N}. [\exists k \in \mathbb{N}. d \cdot k = p] \Rightarrow [d = 1 \vee d = p]$$

**Definition of the product of a list/array:**

For an array $a$ of length $n$, the product is defined inductively:

- $\forall a \in \mathcal{A}. \mathrm{product}(a, 0) := 1$
- $\forall a \in \mathcal{A}, n \in \mathbb{N}^+. \mathrm{product}(a, n) := \mathrm{product}(a, n-1) \cdot a[n-1]$

(We will also write $\mathrm{product}(a)$ as shorthand for $\mathrm{product}(a, \mathrm{len}(a))$.)

**Definition of array permutation:**

$$\forall a, b \in \mathcal{A}, n \in \mathbb{N}. \mathrm{perm}(a, b, n) :\Leftrightarrow$$

$$\exists p: \{0, \dots, n-1\} \to \{0, \dots, n-1\}. p \text{ is bijective } \wedge \forall i \in \{0, \dots, n-1\}. b[i] = a[p(i)]$$

**Definition of substring:**

$$\forall s, s' \in \mathcal{S}, p \in \mathbb{N}. \mathrm{substr}(s, p, s') :\Leftrightarrow$$

$$0 \leq p \wedge p + \mathrm{len}(s') \leq \mathrm{len}(s) \wedge \forall i \in \{0, \dots, \mathrm{len}(s')-1\}. s'[i] = s[p+i]$$

**Definition of a palindrome:**

$$\forall s \in \mathcal{S}. \mathrm{palindrome}(s) :\Leftrightarrow \forall i \in \{0, \dots, \mathrm{len}(s)-1\}. s[i] = s[\mathrm{len}(s)-1-i]$$

---

### (a) Prime factorization of integers

**Input:** $x \in \mathbb{N}, x \geq 2$

**Output:** array $f$ of length $m \in \mathbb{N}^+$ such that

$$\mathrm{product}(f, m) = x \wedge \forall i \in \{0, \dots, m-1\}. \mathrm{prime}(f[i])$$

---

### (b) Finding the largest number in an array of integers

**Input:** array $a$ of length $n \in \mathbb{N}^+$

**Output:** $m \in \mathbb{Z}$ such that

$$[\exists i \in \{0, \dots, n-1\}. a[i] = m] \wedge [\forall i \in \{0, \dots, n-1\}. a[i] \leq m]$$

---

### (c) Reordering an array so that prime numbers precede non-primes

**Input:** array $a$ of length $n \in \mathbb{N}$

**Output:** array $b$ of length $n$ such that

$$\mathrm{perm}(a, b, n) \wedge \exists k \in \{0, \dots, n\}. [\forall i \in \{0, \dots, k-1\}. \mathrm{prime}(b[i])] \wedge [\forall i \in \{k, \dots, n-1\}. \neg \mathrm{prime}(b[i])]$$

(The value $k = 0$ means there are no primes; $k = n$ means all elements are primes. Both are valid outputs.)

---

### (d) Checking whether a string contains a palindromic substring of length 7

**Input:** string $s \in \mathcal{S}$

**Output:** $r \in \{\top, \perp\}$ such that

$$r = \top \Leftrightarrow \exists i \in \{0, \dots, \mathrm{len}(s)-7\}. \mathrm{palindrome}(\mathrm{substr}(s, i, \langle s[i], s[i+1], \dots, s[i+6] \rangle))$$

(Here $\langle s[i], \dots, s[i+6] \rangle$ denotes the substring of $s$ of length 7 starting at position $i$. Alternatively, one may define an auxiliary function $\mathrm{substr\_len}(s, i, l)$ and use it directly.)

---

## Exercise 3 — Proofs

All proofs follow the method from Lecture 2: we keep a list of known facts (assumptions) and a formula to prove, applying proof rules only to the **outermost symbol** until we succeed or find a contradiction.

---

### (a) $[\neg[[r \vee s] \Rightarrow q] \wedge [[r \vee s] \Rightarrow q]] \Rightarrow [[p \Rightarrow q] \wedge \neg[p \Rightarrow q]]$

**Proof.** We assume $\neg[[r \vee s] \Rightarrow q] \wedge [[r \vee s] \Rightarrow q]$ and prove $[p \Rightarrow q] \wedge \neg[p \Rightarrow q]$.

From the assumption, whose outermost symbol is $\wedge$, we conclude both $\neg[[r \vee s] \Rightarrow q]$ and $[[r \vee s] \Rightarrow q]$.

This is a contradiction (we know both a formula and its negation), which finishes the proof. ■

---

### (b) $[\neg p \Rightarrow p] \Rightarrow p$

**Proof.** We assume $\neg p \Rightarrow p$ and prove $p$.

We replace $p$ by its equivalent $\neg\neg p$. So we prove $\neg\neg p$.

For proving a negation, we assume $\neg p$ and try to find a contradiction.

From the assumption $\neg p$ and the known fact $\neg p \Rightarrow p$, we conclude $p$.

Now we know both $\neg p$ and $p$ — a contradiction. This finishes the proof. ■

---

### (c) $[p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]] \Rightarrow \neg p$

**Proof.** We assume $p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]$ and prove $\neg p$.

For proving a negation, we assume $p$ and try to find a contradiction.

From $p$ and the known fact $p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]$, we conclude $[q \vee r] \wedge \neg q \wedge \neg r$.

From this conjunction we conclude three facts: $q \vee r$, $\neg q$, and $\neg r$.

Now the known fact $q \vee r$ is a disjunction, so we do a case distinction:

- **Case $q$:** we know both $q$ and $\neg q$ — contradiction.
- **Case $r$:** we know both $r$ and $\neg r$ — contradiction.

Both cases lead to contradiction, which finishes the proof. ■

---

### (d) $q \Rightarrow [[p \wedge q] \vee [\neg p \wedge q]]$

**Proof.** We assume $q$ and prove $[p \wedge q] \vee [\neg p \wedge q]$.

For proving a disjunction, we assume $\neg[p \wedge q]$ and prove $\neg p \wedge q$.

For proving the conjunction $\neg p \wedge q$, we prove $\neg p$ and $q$ separately.
$q$ is already a known fact.

We prove $\neg p$: we assume $p$ and try to find a contradiction.
From $p$ and $q$, we conclude $p \wedge q$. This contradicts our assumption $\neg[p \wedge q]$. Thus $\neg p$ is proved.

Now from $\neg p$ and $q$, we conclude $\neg p \wedge q$, which is what we needed to prove. ■

---

### (e) $\neg[p \wedge q] \Rightarrow [\neg p \vee \neg q]$

**Proof.** We assume $\neg[p \wedge q]$ and prove $\neg p \vee \neg q$.

For proving a disjunction, we assume $\neg\neg p$ and prove $\neg q$.

Using the equivalence of $\neg\neg p$ and $p$, we now assume $p$ and prove $\neg q$.

For proving $\neg q$, we assume $q$ and try to find a contradiction.

From $p$ and $q$ we conclude $p \wedge q$. But we already know $\neg[p \wedge q]$.

This is a contradiction, which finishes the proof. ■

---

### (f) $[[p \wedge q] \Rightarrow r] \Rightarrow [[p \Rightarrow r] \vee [q \Rightarrow r]]$

**Proof.** We assume $[p \wedge q] \Rightarrow r$ and prove $[p \Rightarrow r] \vee [q \Rightarrow r]$.

For proving a disjunction, we assume $\neg[p \Rightarrow r]$ and prove $q \Rightarrow r$.

For proving $q \Rightarrow r$, we assume $q$ and prove $r$.

We now know: $[p \wedge q] \Rightarrow r$, $\neg[p \Rightarrow r]$, $q$. We need to prove $r$.

We prove a lemma: $p$.

*Proof of lemma $p$:* We replace $p$ by $\neg\neg p$. We assume $\neg p$ and try to find a contradiction. We prove $p \Rightarrow r$ in a sub-lemma.

- *Proof of sub-lemma $p \Rightarrow r$:* We assume $p$ and prove $r$. From $\neg p$ (the assumption of the lemma) and $p$, we have a contradiction, which finishes the sub-proof.

So $p \Rightarrow r$ holds. But we already know $\neg[p \Rightarrow r]$.
This is a contradiction, so the lemma $p$ is proved. □

Now from the lemma, we know $p$. Together with $q$, we conclude $p \wedge q$.
From $p \wedge q$ and the known fact $[p \wedge q] \Rightarrow r$, we conclude $r$.
This finishes the proof. ■

---

### (g) $[p \wedge q] \Rightarrow \neg[\neg p \vee \neg q]$

**Proof.** We assume $p \wedge q$ and prove $\neg[\neg p \vee \neg q]$.

From $p \wedge q$ we conclude both $p$ and $q$.

For proving a negation, we assume $\neg p \vee \neg q$ and try to find a contradiction.

The known fact $\neg p \vee \neg q$ is a disjunction, so we do a case distinction:

- **Case $\neg p$:** we know both $p$ and $\neg p$ — contradiction.
- **Case $\neg q$:** we know both $q$ and $\neg q$ — contradiction.

Both cases lead to contradiction, which finishes the proof. ■

---

### (h) $\neg[p \Rightarrow q] \Rightarrow [q \Rightarrow p]$

**Proof.** We assume $\neg[p \Rightarrow q]$ and prove $q \Rightarrow p$.

For proving an implication, we assume $q$ and prove $p$.

We now know $\neg[p \Rightarrow q]$ and $q$. We need to prove $p$.

We prove a lemma: $p \Rightarrow q$.

*Proof of lemma $p \Rightarrow q$:* We assume $p$ and prove $q$. Since $q$ is already a known fact, the proof of the lemma is done. □

Now we know $p \Rightarrow q$. But we also know $\neg[p \Rightarrow q]$.

This is a contradiction. A contradiction finishes the current proof, so $p$ is proved. ■

---

### (i) $[p \Rightarrow q] \vee [q \Rightarrow r]$

**Proof.** We are to prove the disjunction.

For proving a disjunction, we assume $\neg[p \Rightarrow q]$ and prove $q \Rightarrow r$.

For proving $q \Rightarrow r$, we assume $q$ and prove $r$.

We now know $\neg[p \Rightarrow q]$ and $q$. We need to prove $r$.

We prove a lemma: $p \Rightarrow q$.

*Proof of lemma $p \Rightarrow q$:* We assume $p$ and prove $q$. Since $q$ is already a known fact, the proof of the lemma is done. □

Now we know $p \Rightarrow q$. But we also know $\neg[p \Rightarrow q]$.

This is a contradiction, which finishes the current proof. Hence $r$ is proved. ■

---
