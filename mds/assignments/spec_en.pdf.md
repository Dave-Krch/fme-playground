

# Formal Methods and Specification (SS 2025/26)Specification: Certificates, Abbreviations, Definitions

Stefan Ratschan

Katedra číslicového návrhu  
Fakulta informačních technologií  
České vysoké učení technické v Praze

![Stylized line drawing of a lion holding a sword, representing the Czech Republic.](64662465bba247703fdec49c8f3309f9_img.jpg)

Stylized line drawing of a lion holding a sword, representing the Czech Republic.

![Logo of the Operational Program Prague (OPP A).](5fb340ad68b0c71df0b56698b137e35b_img.jpg)

Logo of the Operational Program Prague (OPP A).

![Logo of the Prague Development Fund (PRAHA PRAGUE PRAGA PRA G).](390120de4fe440c42fea8154fcaad334_img.jpg)

Logo of the Prague Development Fund (PRAHA PRAGUE PRAGA PRA G).

![Logo of the European Union (EVROPSKÁ UNIE).](0538daaa5583c23e17db3a12f2281a55_img.jpg)

Logo of the European Union (EVROPSKÁ UNIE).

Evropský sociální fond Praha & EU: Investujeme do vaší budoucnosti

## Algorithm Specification

Various software development methods

For all, at some point: **specification** of requirements

Specification of the **sorting** problem, as **precise** as possible

### Specification: Sorting Problem

**Input:** array  $a$  of length  $n$

**Output:** array  $b$  of length  $n$  s.t.

$$\forall i \in \{0, \dots, n-2\} . b[i] \leq b[i+1]$$
$$\forall i, j \in \{0, \dots, n-1\} . i \leq j \Rightarrow b[i] \leq b[j]$$

???

|         |   |   |   |   |   |
|---------|---|---|---|---|---|
| Input:  | 7 | 5 | 1 | 6 | 8 |
| Output: | 2 | 3 | 4 | 7 | 9 |

$$\forall i \in \{0, \dots, n-1\} \exists j \in \{0, \dots, n-1\} . a[i] = b[j]$$

|         |   |   |   |   |   |
|---------|---|---|---|---|---|
| Input:  | 4 | 4 | 4 | 4 | 4 |
| Output: | 2 | 3 | 4 | 7 | 9 |

$$\forall i \in \{0, \dots, n-1\} \exists j \in \{0, \dots, n-1\} . b[i] = a[j]$$

### Specification: Sorting Problem

**Input:** array  $a$  of length  $n$

**Output:** array  $b$  of length  $n$  s.t.

$$\forall i \in \{0, \dots, n-2\} . b[i] \leq b[i+1]$$

|         |   |   |   |   |   |
|---------|---|---|---|---|---|
| Input:  | 2 | 4 | 2 | 5 | 1 |
|         | 1 | 3 | 2 | 4 | 0 |
| Output: | 1 | 2 | 2 | 4 | 5 |

there is a permutation  $p$  of  $\{0, \dots, n-1\}$  s.t.

$$\forall i \in \{0, \dots, n-1\} . a[i] = b[p(i)].$$

where a function  $p$  is a permutation of a set  $S$  iff

$p : S \rightarrow S$ ,  $p$  is bijective.

Alternative equivalent possibility:

$$\forall i \in \{0, \dots, n-1\} . b[i] = a[p(i)].$$

### How to Check Results of Algorithms?

Out-sourcing of software development or of computation:

How to check correctness?

**Input:** array  $a$  of length  $n$

**Output:** array  $b$  of length  $n$  s.t.

- ▶  $\forall i \in \{0, \dots, n-2\} . b[i] \leq b[i+1]$
- ▶ there is a permutation  $p$  of  $\{0, \dots, n-1\}$  s.t.  
 $\forall i \in \{0, \dots, n-1\} . a[i] = b[p(i)]$ .

Additional output: permutation

**Input:** array  $a$  of length  $n$

**Output:** array  $b$ ,  $p$  of length  $n$  s.t.

- ▶  $\forall i \in \{0, \dots, n-2\} . b[i] \leq b[i+1]$
- ▶  $p$  is a permutation of  $\{0, \dots, n-1\}$  s.t.  
 $\forall i \in \{0, \dots, n-1\} . a[i] = b[p[i]]$ .

### How to Check Results of Algorithms?

Certificate [McConnell et al., 2011]: **additional output** that allows simple **verification** that the output is correct

“simple”: not precisely defined,  
especially this is **not** about computational complexity  
(see NP-hardness)

#### Example: Cyclicality of Graph

**Input:** graph with

- ▶ finite set of nodes  $V$
- ▶ edges  $E \subseteq V \times V$  (hence the graph is oriented)

**Output:**

- ▶ If input graph is cyclic: a cycle of the graph
- ▶ If input graph is acyclic: a topological ordering of the graph

![A directed graph with five nodes labeled A, B, C, D, and E. Node A has an incoming edge from the left. There are directed edges from A to B, B to C, C to D, and D to E. Additionally, there are two curved directed edges from A to D and from C to E, both pointing to the right.](d793cf7c174b89eb024d132f00679787_img.jpg)

A directed graph with five nodes labeled A, B, C, D, and E. Node A has an incoming edge from the left. There are directed edges from A to B, B to C, C to D, and D to E. Additionally, there are two curved directed edges from A to D and from C to E, both pointing to the right.

Here:

- ▶ a cycle of a graph  $(V, E)$  is a sequences  $a_1, \dots, a_n$  s.t.  
for all  $i \in \{1, \dots, n\}$ ,  $a_i \in V$ ,  $(a_i, a_{(i \bmod n)+1}) \in E$
- ▶ a topological ordering of a graph  $(V, E)$  is a permutation  $p$  of the edges  $V$  s.t.  
for all  $i, j \in \{1, \dots, |V|\}$ ,  $i < j$ ,

$(p(i), p(j)) \notin E$ .

#### Example: Solution of System of Linear Equations

Original specification (everything ranges over the real numbers):

**Input:**  $\exists x . Ax = b$

**Output:** either “yes” or “no”

Extended specification with certificate:

**Input:**  $\exists x . Ax = b$

**Output:** either “yes” or “no”,

- ▶ In case “yes”:  $x$ , s.t.  $Ax = b$
- ▶ In case “no”:  $y$  s.t.  $y^T A = \vec{0}^T$ ,  $y^T b = 1$

For such a  $y$ , existence of a solution  $x$  leads to the following contradiction:

$$(y^T A)x = \vec{0}^T x = 0, y^T (Ax) = y^T b = 1$$

Such a certificate exists for every unsolvable system  
(Farkas lemma)

We can check correct output using **assertions**.

## Certificates for Further Problems

[McConnell et al., 2011]

### Top-Down Formalization of Specifications

Specification of an algorithm that

for a given text, given as character string,

determines the **set of words** contained in the text.

See lecture notes, section “program specification”

Assumption: type  $\mathcal{S}$  of character strings with a function  $len$  denoting string length, and type  $\mathcal{N}$  of natural numbers including zero

**Input:**  $a \in \mathcal{S}$

**Output:**  $\{w \in \mathcal{S} \mid word(a, w)\}$

For formalizing the predicate *word* we should express:

- ▶ there is a position  $i$  where  $w$  is a substring of  $a$
- ▶ at that position,  $a$  contains a word

$$\forall a, w . word(a, w) :\Leftrightarrow \exists i . substr(a, i, w) \wedge wbound(a, i, len(w))$$

### Top-Down Formalization of Specifications

Formalization of the predicate *wbound*:

$$\forall a, p, l . wbound(a, p, l) :\Leftrightarrow \begin{aligned} & 0 \leq p \wedge p + l \leq len(a) \wedge \\ & [p = 0 \vee a[p - 1] = ' '] \wedge \\ & [p + l = len(a) \vee a[p + l] = ' '] \wedge \\ & \neg \exists k \in \{p, \dots, p + l - 1\} . a[k] = ' ' \end{aligned}$$

Formalization of the function *substr*:

$$\forall S \in \mathcal{S}, p \in \mathcal{N}, S' \in \mathcal{S} . substr(S, p, S') :\Leftrightarrow \begin{aligned} & 0 \leq p < len(S) \\ & p + len(S') \leq len(S) \\ & \forall i \in \{0, \dots, len(S') - 1\} . S'[i] = S[p + i] \end{aligned}$$

Now we could formalize strings ...

### Alternative: Bottom-Up

## Practical Issues

**Input:** set of natural numbers  $S$

**Output:**  $k$  s.t.  $k \in S \wedge \text{even}(k)$ , where  $\forall i . \text{even}(i) :\Leftrightarrow \exists j \in \mathbb{Z} . 2j = i$

o.k.? for input  $\{7, 5, 9\}$ ? What to do?

$$\begin{aligned} &[\exists i . i \in S \wedge \text{even}(i)] \Rightarrow [k \in S \wedge \text{even}(k)] \\ &\quad \wedge \\ &\neg[\exists i . i \in S \wedge \text{even}(i)] \Rightarrow k = -1 \end{aligned}$$

Abbreviation:

$$\mathbf{if} \ [\exists i . i \in S \wedge \text{even}(i)] \ \mathbf{then} \ [k \in S \wedge \text{even}(k)] \\ \mathbf{else} \ k = -1$$

In general:

$$\mathbf{if} \ P \ \mathbf{then} \ F \ \mathbf{else} \ G$$

abbreviation for

$$[P \Rightarrow F] \wedge [\neg P \Rightarrow G]$$

### Further Abbreviation

Variant (result of  $\alpha$ -conversion)

$$\begin{aligned} &[\exists k . k \in S \wedge \text{even}(k)] \Rightarrow [k \in S \wedge \text{even}(k)] \\ &\quad \wedge \\ &\neg[\exists k . k \in S \wedge \text{even}(k)] \Rightarrow k = -1 \end{aligned}$$

Attention:  $k \neq k$ !

Abbreviation:

$k \in S \wedge \text{even}(k)$ , if such a  $k$  exists, and  $k = -1$ , otherwise.

In general:

$P$ , if such a  $k$  exists, and  $G$ , otherwise

Abbreviation for

$$\begin{aligned} &[\exists k . P] \Rightarrow P \\ &\quad \wedge \\ &\neg[\exists k . P] \Rightarrow G \end{aligned}$$

### Further Abbreviations

$$\exists x . P(x) \wedge [\neg \exists y . P(y) \wedge x \neq y]$$

Abbreviations:  $\exists!x . P(x)$ ,  $\overset{1}{\exists}x . P(x)$

## Definitions

It would be very annoying if we would have to write

$\exists y . 2y = x$  every time we want to express that  $x$  is a even number

Similar to functions/procedure in programming languages, in logic, we can **introduce new terms**, so that we

- ▶ do not have to write the same formula many times
- ▶ can write formulas in a clear and structured way
- ▶ avoid redundancy.

Example:  $\forall x . even(x) :\Leftrightarrow \exists y . 2y = x$

Usage:  $even(7)$  means  $\exists y . 2y = 7$

### Definition of Predicates

$$\forall v_1 \dots v_n . p(v_1, \dots, v_n) :\Leftrightarrow \phi$$

where

- ▶  $p$  is a new predicate of arity  $n$ ,
- ▶  $\phi$  is a formula, that
  - ▶ does not contain  $p$  (exception: recursive definitions), and
  - ▶ does not contain variables different from  $v_1, \dots, v_n$ .

Examples of bad definitions:

- ▶  $\forall x . good(x) :\Leftrightarrow good(x)$
- ▶  $\forall x . good(x) :\Leftrightarrow longer(x, y)$

Then  $p(t_1, \dots, t_n)$  stands for  $\phi[v_1 \leftarrow t_1, \dots, v_n \leftarrow t_n]$   
and we can replace one side by the other

see Lecture 2:

rule for universal quantifiers in known facts and  
replacing equivalences.

### Explicit Definition of Function Symbols

Example:  $\forall x . \text{double}(x) := 2x$

In general:

$$\forall v_1 \dots v_n . f(v_1, \dots, v_n) := t$$

where

- ▶  $f$  is a new function symbol of arity  $n$ ,
- ▶  $t$  is a term that
  - ▶ does not contain  $f$  (exception: recursive definitions), and
  - ▶ does not contain any variable different from  $v_1, \dots, v_n$ .

Then  $f(t_1, \dots, t_n)$  stands for  $t[v_1 \leftarrow t_1, \dots, v_n \leftarrow t_n]$   
and we can replace one side by the other.

see next lecture:

rule for universal quantifiers in known facts and  
replacing equal terms.

### Implicit Definition of Function Symbols

Example:

$$\forall x_1, x_2, y . y = GCD(x_1, x_2) :\Leftrightarrow$$

$y$  divides  $x_1$ ,  $y$  divides  $x_2$ ,  $\neg\exists y' . y' > y, y'$  divides  $x_1, y'$  divides  $x_2$

In general:

$$\forall v_1 \dots v_n, v . v = f(v_1, \dots, v_n) :\Leftrightarrow \phi$$

- ▶  $f$  is a new function symbol of arity  $n$ ,
- ▶  $\phi$  is a formula that
  - ▶ does not contain  $f$ , and
  - ▶ does not contain any variable different from  $v, v_1, \dots, v_n$ .

Replacing is a little bit more complicated

(first introduce a new existential quantifier, then replace)

#### Example Continued

$$\forall x_1, x_2, y . y = GCD(x_1, x_2) :\Leftrightarrow$$

$y$  divides  $x_1$ ,  $y$  divides  $x_2$ ,  $\neg\exists y' . y' > y, y'$  divides  $x_1, y'$  divides  $x_2$

Usage:  $2GCD(x + y, xy + 1) = 10$

$$\exists v . 2v = 10 \wedge v = GCD(x + y, xy + 1)$$

$$\exists v . 2v = 10 \wedge v \text{ divides } x + y, v \text{ divides } xy + 1,$$

$\neg\exists y' . y' > v, y'$  divides  $x + y, y'$  divides  $xy + 1$

### Definitions in Practice

In mathematics and computer science texts  
definitions are used in an **informal** way ...

... which is o.k.:

Our formal discussion helps to  
read, write, and use informal definitions correctly.

Alternative symbols denoting definitions:  $\Leftrightarrow, \doteq, \hat{=}, \dots$

Missing: recursive definitions (often used for lists, natural numbers etc.)

## Literature

R.M. McConnell, K. Mehlhorn, S. Näher, and P. Schweitzer. Certifying algorithms. *Computer Science Review*, 5(2):119 – 161, 2011. ISSN 1574-0137.