

# Formal Methods and Specification (SS 2025/26)Lecture 2: Proofs

Stefan Ratschan

Czech Technical University in Prague

![Stylized line drawing of a lion holding a sword, representing the Czech Technical University in Prague.](64662465bba247703fdec49c8f3309f9_img.jpg)

Stylized line drawing of a lion holding a sword, representing the Czech Technical University in Prague.

![Logos of the Operational Program Prague (OPP A), the University of Economics and Business Prague (PRAHA PRAGUE), and the European Union (EVROPSKÁ UNIE).](5fb340ad68b0c71df0b56698b137e35b_img.jpg)

Logos of the Operational Program Prague (OPP A), the University of Economics and Business Prague (PRAHA PRAGUE), and the European Union (EVROPSKÁ UNIE).

Evropský sociální fond Praha & EU: Investujeme do vaší budoucnosti

### Checking Correctness of Programs

Example: You write a program for a friend

Specification:

- ▶ Input:  $x \in \mathbb{Z}$
- ▶ Output:  $\hat{x} \in \mathbb{Z}$ ,  $\hat{x} < x - |f(x)|$

Program:

```
if  $f(x) \geq 0$  then                      //  $f$  behaves like a mathematical function
    return  $x - f(x) - 1$ 
else
    return  $x + f(x) - 1$ 
```

Friend: Is it correct? For input  $-3$ , yes!  $8$ ?  $-2$ ? . . . testing

General situation in life

Alternatively: evidence in form of argument

Problems: Which arguments allowed? programming language specific

### Reduction to Logic

Specification:

- ▶ Input:  $x \in \mathbb{Z}$
- ▶ Output:  $\hat{x} \in \mathbb{Z}$ ,  $\hat{x} < x - |f(x)|$

Program:

```
if  $f(x) \geq 0$  then  
    return  $x - f(x) - 1$   
else  
    return  $x + f(x) - 1$ 
```

Program is correct iff *the verification condition*

$$\forall x \in \mathbb{Z}. \quad \begin{array}{c} f(x) \geq 0 \Rightarrow x - f(x) - 1 < x - |f(x)| \\ \wedge \\ \neg f(x) \geq 0 \Rightarrow x + f(x) - 1 < x - |f(x)| \end{array}$$

holds (independent of  $f$ , using properties of  $\mathbb{Z}$ ).

Similar problem:  $\mathbb{Z}$  is an infinite set

How to convince somebody with absolute **certainty**?

#### Proofs

Proof: **Absolutely certain evidence** that a formula holds, that **everybody** can check **always** and **everywhere**.

Proofs that are written formally,  
can even be checked for correctness by a computer  
(viz. proof-carrying code)

“formula  $\phi$  holds”:  $\phi$  is a **logical consequence** of  $\Phi$  ( $\Phi \models \phi$ )  
where  $\Phi$  are the things we already know.

Often we will not state  $\Phi$  explicitly, and write  $\models \phi$ .

Recall: validity of a formula

##### Proof by Equivalence

One can prove  $A \Leftrightarrow B$  by

proving that  $A$  is equivalent to  $B$  based on equivalence rules  
(see MI-TES, Section 1.5 in lecture notes)

Example:  $A \vee [B \wedge C]$  is equivalent to  $[A \vee B] \wedge [A \vee C]$

Then one can prove  $\phi$  by proving  $\phi \Leftrightarrow \top$

But unfortunately, this is **not enough** (the method is not complete).

Today: a **complete** proof method:

There is a proof for every formula that holds.

*outermost symbol*: root of syntax tree

$$[p \Rightarrow q] \Rightarrow [[p \wedge r] \Rightarrow [q \wedge r]]$$

##### Example

*You will find the animated version of the example in the video.  
In written form, you can find it on the next slide.*

##### Example

Example: Prove

$$[p \Rightarrow q] \Rightarrow [[p \wedge r] \Rightarrow [q \wedge r]]$$

We assume  $p \Rightarrow q$  and prove  $[p \wedge r] \Rightarrow [q \wedge r]$ . For proving  $[p \wedge r] \Rightarrow [q \wedge r]$  we assume  $p \wedge r$  and prove  $q \wedge r$ . For proving  $q \wedge r$  we prove both  $q$  and  $r$ :

- ▶ Proof of  $q$ : From the assumption  $p \wedge r$  we conclude both  $p$  and  $r$ . From the assumptions  $p$  and  $p \Rightarrow q$  follows  $q$ , which finishes this part of the proof.
- ▶ Proof of  $r$ : We already know that from the assumption  $p \wedge r$  follows  $r$ , which finishes also this part of the proof.

###### Proof Method

For proving something, we keep a **list** of things (assumptions) that we already **know**, and

- ▶ **add known** things (“to assume, to conclude”), and
- ▶ **simplify** the thing to **prove**,

until what we want to prove is known.

As usual we assume an infinite set of predicate and function symbols

We handle types intuitively.

###### Proof Rules—Propositional Logic without Negation

$A, B$  meta-variables, i.e., variables that represent arbitrary formulas

The rules refer to the **outer-most symbol** of a formula

How to work on things that we have to prove?

$A \wedge B$ : Separately prove  $A$  and  $B$

$A \vee B$ : Assume  $\neg A$  and prove  $B$  (or vice versa).

$A \Rightarrow B$ : Assume  $A$  and prove  $B$ .

How to generate new knowledge?

$A \wedge B$ : Conclude both  $A$  and  $B$

$A \vee B$ : Do a case distinction: first write "Case  $A$ :", assume  $A$  and finish the proof. Then write "Case  $B$ :", assume  $B$ , and finish the proof.

$A \Rightarrow B$ : If we also know  $A$  then we conclude  $B$ .

What about  $\Leftrightarrow$ ?

$A \Leftrightarrow B$ : shortcut for  $[A \Rightarrow B] \wedge [B \Rightarrow A]$

##### Proof Assistant

<https://fme-proof-assistant.netlify.app/>

prototype!

##### Proof by Contradiction

Example:

$$[p \wedge \neg p] \Rightarrow q$$

Holds, due to left-hand side  $p \wedge \neg p$

To prove the formula, we assume  $p \wedge \neg p$  and prove  $q$ . From the assumption  $p \wedge \neg p$  we know both  $p$  and  $\neg p$ , which is a . . . .

*Contradiction:* We know both a formula and its negation.

A contradiction finishes **every** proof (**successfully!**)

So: this proves what we originally wanted to prove!

Further example:  $p \Rightarrow [\neg p \Rightarrow q]$

Attention: The rules for proving conjunction and using disjunction in an assumption start two **separate** proofs.

##### Negations: Example

*You will find the animated version of the example in the video.  
In written form, you can find it on the next slide.*

##### Example

$$[[p \Rightarrow q] \wedge [p \Rightarrow \neg q]] \Rightarrow \neg p$$

We assume  $[p \Rightarrow q] \wedge [p \Rightarrow \neg q]$  and prove  $\neg p$ . For proving  $\neg p$  we assume  $p$  and try to find a contradiction.

From the assumption  $[p \Rightarrow q] \wedge [p \Rightarrow \neg q]$  we know the assumptions  $p \Rightarrow q$  and  $p \Rightarrow \neg q$ . From the assumptions  $p$  and  $p \Rightarrow q$  follows  $q$ , and from the assumptions  $p$  and  $p \Rightarrow \neg q$  follows  $\neg q$ . So we have both assumptions  $q$  and  $\neg q$ , which is a contradiction. This finishes the proof.

##### Proof Rules—Propositional Case

How to work on things that we have to prove?

$A \wedge B$ : Separately prove  $A$  and  $B$

$A \vee B$ : Assume  $\neg A$  and prove  $B$  (or vice versa).

$A \Rightarrow B$ : Assume  $A$  and prove  $B$ .

$\neg A$ : Assume  $A$ , and try to find a contradiction

How to generate new knowledge?

$A \wedge B$ : Conclude both  $A$  and  $B$

$A \vee B$ : Do a case distinction: first write "Case  $A$ :", assume  $A$  and finish the proof. Then write "Case  $B$ :", assume  $B$ , and finish the proof.

$A \Rightarrow B$ : If we also know  $A$  then we conclude  $B$ .

### Quantifiers: Reminder

For us, the scope of quantifiers is as far to the right as possible!

So

$$\exists x . P(x) \wedge Q(x)$$

means

$$\exists x . [P(x) \wedge Q(x)],$$

and not

$$[\exists x . P(x)] \wedge Q(x).$$

#### Universal Quantifiers: Example

Assumptions:

Assumptions:

$$f(a) \geq 0$$

To prove:

$$a - f(a) - 1 < a - |f(a)|$$

Assumptions:

$$\neg f(a) \geq 0$$

To prove:

$$a + f(a) - 1 < a - |f(a)|$$

To prove:

$$\begin{aligned} & f(x) \geq 0 \Rightarrow x - f(x) - 1 < x - |f(x)| \\ \forall x \in \mathbb{Z}. \quad & \wedge \\ & \neg f(x) \geq 0 \Rightarrow x + f(x) - 1 < x - |f(x)| \\ & f(a) \geq 0 \Rightarrow a - f(a) - 1 < a - |f(a)| \\ & \wedge \\ & \neg f(a) \geq 0 \Rightarrow a + f(a) - 1 < a - |f(a)| \end{aligned}$$

where  $a$  is new (“arbitrary, but fixed”).

#### Universal Quantifiers: Example

$$\forall x \in \mathbb{Z}. \quad \begin{array}{c} f(x) \geq 0 \Rightarrow x - f(x) - 1 < x - |f(x)| \\ \wedge \\ \neg f(x) \geq 0 \Rightarrow x + f(x) - 1 < x - |f(x)| \end{array}$$

We want to prove the formula for an arbitrary integer, that is, an integer, about which we do **not know anything**. So we introduce a **new** constant  $a$ , and prove

$$\begin{array}{c} f(a) \geq 0 \Rightarrow a - f(a) - 1 < a - |f(a)| \\ \wedge \\ \neg f(a) \geq 0 \Rightarrow a + f(a) - 1 < a - |f(a)| \end{array}$$

To prove the conjunction we prove both

$$f(a) \geq 0 \Rightarrow a - f(a) - 1 < a - |f(a)|$$

and

$$\neg f(a) \geq 0 \Rightarrow a + f(a) - 1 < a - |f(a)|.$$

#### Universal Quantifiers: Example

To prove

$$f(a) \geq 0 \Rightarrow a - f(a) - 1 < a - |f(a)|,$$

we assume

$$f(a) \geq 0$$

and prove

$$a - f(a) - 1 < a - |f(a)|$$

which clearly holds under this assumption.

The proof of

$$\neg f(a) \geq 0 \Rightarrow a + f(a) - 1 < a - |f(a)|$$

is similar.

#### Existential Quantifiers: Example

$$\exists x \in \mathbb{N} . x \geq 4 \wedge 2 \nmid x$$

Proving that something exists that fulfills a certain property:  
find one object that fulfills this property.

For example, here: 7

Substitute, the result is  $7 \geq 4 \wedge 2 \nmid 7$

Clearly both  $7 \geq 4$  and  $2 \nmid 7$ .

##### Proof Rules Including Quantifiers 1

How to work on things that we have to prove?

$\exists x. A$ : Choose (by intuition, creativity, or any other special connection with god) a term  $t$ , and prove  $A[x \leftarrow t]$

$\forall x. A$ : Choose a **new** constant, e.g.,  $a$ , write/say “let  $a$  be arbitrary but fixed” and prove  $A[x \leftarrow a]$ , instead

$A \wedge B$ : Separately prove  $A$  and  $B$

$A \vee B$ : Assume  $\neg A$  and prove  $B$  (or vice versa).

$A \Rightarrow B$ : Assume  $A$  and prove  $B$ .

$\neg A$ : Assume  $A$ , and try to find a contradiction

Remember the definition of the notion “term”.

Especially: Every constant is a term  
(as usual, we assume that there are infinitely many constants).

##### Proof Rules Including Quantifiers 2

How to generate new knowledge?

- $\exists x. A$ : Choose a **new** constant, e.g.,  $a$ , write/say "let  $a$  be such that  $A[x \leftarrow a]$ ", and add  $A[x \leftarrow a]$  to our knowledge base
- $\forall x. A$ : Choose (by intuition, creativity, or any other special connection with god) a term  $t$  and conclude  $A[x \leftarrow t]$
- $A \wedge B$ : Conclude both  $A$  and  $B$
- $A \vee B$ : Do a case distinction: first write "Case  $A$ :", assume  $A$  and finish the proof. Then write "Case  $B$ :", assume  $B$ , and finish the proof.
- $A \Rightarrow B$ : If we also know  $A$  then we conclude  $B$ .

##### Example

*You will find the animated version of the example in the video.  
In written form, you can find it on the next slide.*

##### Example

$$[[\forall x. P(x) \Rightarrow Q(x)] \wedge [\exists x. P(f(x))]] \Rightarrow [\exists x. Q(x)]$$

(scope of quantifiers as far as possible!)

We assume  $[\forall x. P(x) \Rightarrow Q(x)] \wedge [\exists x. P(f(x))]$ , that is, we assume both  $\forall x. P(x) \Rightarrow Q(x)$  and  $\exists x. P(f(x))$ . We want to prove  $\exists x. Q(x)$ .

Due to the assumption  $\exists x. P(f(x))$  we can choose a new constant  $a$  such that  $P(f(a))$ . Due to the assumption  $\forall x. P(x) \Rightarrow Q(x)$  we can conclude  $P(f(a)) \Rightarrow Q(f(a))$ . For proving  $\exists x. Q(x)$  we choose for  $x$  the term  $f(a)$ , and now it suffices to prove  $Q(f(a))$ , which follows from the assumptions  $P(f(a))$  and  $P(f(a)) \Rightarrow Q(f(a))$ . This finishes the proof.

##### Proof Rules for Equalities

For every term  $t$  we can assume  $t = t$ .

If we know  $t_1 = t_2$ ,

then we can always substitute  $t_1$  for  $t_2$  and vice versa.

Example:

If we know  $a = b$  and  $P(a)$ ,

then we can add the assumption  $P(b)$ .

But: see slide “Substitutions: Avoiding Name Clashes” below.

#### Using Equivalences

It is always allowed to **replace equivalences**  
(e.g.,  $A \wedge B$  is equivalent with  $B \wedge A$ )

Strictly necessary is only the equivalence between  **$A$**  and  **$\neg\neg A$**

Especially: When proving  $A$ , we can

1. replace it by  $\neg\neg A$ ,
2. apply the rule for proving negations,
3. and add  $\neg A$  as additional knowledge.

### Lemmas: Example:

*You will find the animated version of the example in the video.  
In written form, you can find it on the next slide.*

#### Lemmas: Example:

$$\left[ p \wedge [[p \vee q] \Rightarrow r] \right] \Rightarrow r$$

We assume  $p$ ,  $[p \vee q] \Rightarrow r$  and try to prove  $r$ .

Now it would be useful to know  $p \vee q$ ,  
which would be easy to prove from  $p$ .

This is possible, but needs a separate proof (lemma).

Proof of the lemma  $p \vee q$  from our current knowledge: For proving  $p \vee q$  we assume  $\neg q$ , and prove  $p$ . The formula  $p$  is already a known fact, so the proof of the lemma is finished.

Now we add the formula  $p \vee q$  to our know facts. From the formula  $[p \vee q] \Rightarrow r$  we know  $r$  which finishes the main proof.

#### Lemmas in General

We also have only **one** formula to prove,  
but we may have **several assumptions**.

If we would like to **prove** some **additional knowledge**,  
we can prove it in a separate proof (from the current knowledge)  
we can add to our knowledge.

A lemma may recursively contain further lemmas etc

#### Usage of Lemmas: Typical Examples

If we know  $A \Rightarrow B$ , then

prove  $A$  using a lemma, and

then from  $A$  and  $A \Rightarrow B$  we can conclude  $B$ .

$A \vee \neg A$  (case distinction) can be proved without any assumptions,  
so we can add formulas of this form **always**.

If we know  $\neg A$ ,

prove  $A$  using a lemma,

contradiction, successful proof.

In all cases:

$A$  and  $B$  can be complicated formulas!

Attention: A lemma is a **separate** proof!

Except for the formula that we prove in the lemma,

**no** influence on the main proof.

##### Example:

*You will find the animated version of the example in the video.  
In written form, you can find it on the next slide.*

##### Example:

$$[\neg\forall x . P(f(g(x)))] \Rightarrow [\exists x . \neg P(x)]$$

We assume  $\neg\forall x . P(f(g(x)))$  and prove  $\exists x . \neg P(x)$ . For proving an existential quantifier, we would need a term to substitute, but we do not know anything useful. So we add a double negation, resulting in  $\neg\neg\exists x . \neg P(x)$ , and apply the rule for proving negations which adds  $\neg\exists x . \neg P(x)$  as additional knowledge.

Now we have two formulas with negations as knowledge. If we would know  $\forall x . P(f(g(x)))$ , we would arrive at a contradiction. Hence we prove this formula in a lemma.

##### Lemma: Proof of $\forall x . P(f(g(x)))$

In the lemma, we apply the proof rule for universal quantifiers, and prove  $P(f(g(a)))$  for a new, arbitrary, but fixed constant  $a$ . Now we do not have any proof rule left to apply. So we again add a double negation, and then add  $\neg P(f(g(a)))$  as additional knowledge.

At this point, we are still proving the lemma. We know  $\neg \exists x . \neg P(x)$  and  $\neg P(f(g(a)))$ . The latter formula contains the term  $f(g(a))$  that we may use to prove an existential quantifier. Hence, we prove  $\exists x . \neg P(x)$  using a another lemma within the current lemma.

##### Example continued

The proof of this second lemma immediately succeeds by choosing  $f(g(a))$  for  $x$ . Hence we can add  $\exists x . \neg P(x)$  to the knowledge in the original lemma. This creates a contradiction which proves this original lemma. As a consequence, we can add  $\forall x . P(f(g(x)))$  to the main proof. This again creates a contradiction which finishes the proof.

#### Substitution: Avoiding Name Clashes

Attention: Substitutions must **not** result in **new bound variables!**

Example: If we want to prove

$$\exists x \forall y . p(x, z)$$

we cannot choose  $f(y)$  for the variable  $x$ , since

$[\forall y . p(x, y)][x \leftarrow f(y)]$  is  $\forall y . p(f(y), z)$ ,  
where  $y$  is suddenly bound by a quantifier .

Example: If we know both  $\exists x . P(x, y)$  and  $y = f(x)$ , then  
we are not allowed to substitute  $f(x)$  for  $y$  in  $\exists x . P(x, y)$   
since the result  $\exists x . P(x, f(x))$  would have a new bound variable.

Solution: Rename the bound variable:  $\exists z . P(z, y)$  (always allowed)

#### Unsuccessful Proofs: Examples

$$p \vee [\neg p \wedge q]$$

Counter-example:  $\{p \mapsto \perp, q \mapsto \perp\}$

$$\neg[p \vee [\neg p \wedge q]]$$

Counter-example:  $\{p \mapsto \top, q \mapsto \perp\}$

$$p \wedge \neg p$$

Not only has a counter-example, but we can even prove negation.

$$\forall x . P(x)$$

Counter-example:

over the integers, when interpreting  $P(x)$  as “ $x$  is even”,  
the formula certainly does not hold.

#### Unsuccessful Proofs . . .

What follows from this? nothing!

The formula either

- ▶ holds and we were not able to prove it, or it
- ▶ does not hold.

For showing that a formula does not hold we may

- ▶ find a **counter-example**, or
- ▶ even try to **prove** the **negation** of the formula.

### Completeness

![A color photograph of Kurt Gödel and Albert Einstein standing outdoors. Gödel, on the right, is wearing a dark suit and glasses. Einstein, on the left, has white hair and is wearing a blue sweater. They are both looking towards the camera.](6358c7a33847eab5f30f9de43cd97e02_img.jpg)

A color photograph of Kurt Gödel and Albert Einstein standing outdoors. Gödel, on the right, is wearing a dark suit and glasses. Einstein, on the left, has white hair and is wearing a blue sweater. They are both looking towards the camera.

Kurt Gödel, 1906 (Brno) — 1978 (Princeton): Ph.D. thesis, 33 pages:

For every formula  $\phi$  s.t.  $\models \phi$ , there is proof.

Then: ... in a different proof system [Gödel, 1929].

But it also holds for our system  
(when including the equivalence between  $A$  and  $\neg\neg A$ )

### Different Methods, Literature

Our method: informal version of *sequent calculus* [Ebbinghaus et al., 1984].

There are various **variants**

Alternative proof system: Hilbert calculus

Useful for studying the basics of mathematics, but  
not for everyday proving.

### Further Lectures

- ▶ Data structures: sets, lists, arrays etc.
- ▶ Program correctness
- ▶ Program correctness
- ▶ Program correctness
- ▶ Program correctness
- ▶ ...

#### Quantifiers, Equality, Equivalence: Example

$\forall x \in \mathbb{N}. x$  is divisible by 2  $\Rightarrow 6x$  is divisible by 4

We want to prove the implication for an arbitrary natural number, that is, a natural number, about which we do **not know anything**. So we introduce a **new constant  $a$** , and prove

$a$  is divisible by 2  $\Rightarrow 6a$  is divisible by 4,

that is, we assume that  $a$  is divisible by 2 and prove:  $6a$  is divisible by 4.

The definition of divisibility is:

$\forall x, y . x$  is divisible by  $y \Leftrightarrow \exists k \in \mathbb{N}. yk = x$

Especially:

- ▶  $a$  is divisible by 2  $\Leftrightarrow \exists k \in \mathbb{N}. 2k = a$
- ▶  $6a$  is divisible by 4  $\Leftrightarrow \exists k \in \mathbb{N}. 4k = 6a$

So we can replace the formulas in the proof by equivalent ones

#### Quantifiers, Equality, Equivalence: Example Continued

From the assumption

$$\exists k \in \mathbb{N} . 2k = a$$

we prove

$$\exists k \in \mathbb{N} . 4k = 6a.$$

We give a name to the symbol  $k$  in the assumptions, e.g.,  $k'$ , so we know

$$2k' = a.$$

We can write this equivalently as

$$12k' = 6a$$

$$4 \ 3 \ k' = 6a$$

For proving the existential quantifier we choose for  $k$  the term  $3k'$ , so it suffices to prove

$$4 \ 3 \ k' = 6a$$

which we already know. This finishes the proof.

#### Literature I

H.-D. Ebbinghaus, J. Flum, and W. Thomas. *Mathematical Logic*. Springer Verlag, 1984.

Kurt Gödel. *Über die Vollständigkeit des Logikkalküls*. PhD thesis, Universität Wien, 1929.