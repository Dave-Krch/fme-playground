

# Formal Methods and Specification - Assignment 1

We discussed examples for Exercise 2 during the first tutorial, and you can find further examples in the section “Program Specification” in the lecture notes. We also discussed examples for Exercise 3 during the second lecture.

## Exercise 1

Tell us an interesting story about a misunderstanding resulting from an imprecise program specification or comment. Solving this exercise does not directly result in any point, but the best story will be awarded with bonus points. Note that stories that you experienced personally are usually more interesting than stories copied from the internet.

## Exercise 2

Develop input/output specifications for the following problems:

- (a) prime factorization of integers
- (b) finding the largest number in an array of integers
- (c) for a given array of integers, computing an array of integers that contains the same elements as the input, but re-ordered in such a way that prime numbers occur before any non-primes
- (d) checking whether a given string contains a sub-string that is a palindrome of length 7 (here, you may define a palindrome simply as a string whose reverse is the same as the original string)

The solution should be formal, based on predicate logic and basic data structures and mathematical objects such as integers, lists, sets, arrays etc. You may (and indeed are encouraged to) use auxiliary definitions but you should explicitly define all auxiliary notions, with two exceptions:

- You are allowed to re-use definitions that we already discussed during the first exercise session and definitions you find in the chapter “program specification” in the lecture notes.
- You may assume basic operations on numbers and sets (e.g., the predicate  $<$  for checking whether an integer is smaller than another one, the function  $+$  for adding two integers, the function  $\cup$  for taking the union of two sets). However, do *not* use any pre-defined maximum operator. If you want to use a maximum function, you have to define it yourself.

Note that description of the problems above is deliberately imprecise, so you have a certain freedom in interpreting them. Moreover, even for a fixed interpretation, there are often several possible ways of describing it using logical formulas.

(4 × 2 points)

## Exercise 3

Prove the following formulas:

- (a)  $[\neg[[r \vee s] \Rightarrow q] \wedge [[r \vee s] \Rightarrow q]] \Rightarrow [[p \Rightarrow q] \wedge \neg[p \Rightarrow q]]$
- (b)  $[\neg p \Rightarrow p] \Rightarrow p$
- (c)  $[p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]] \Rightarrow \neg p$
- (d)  $q \Rightarrow [[p \wedge q] \vee [\neg p \wedge q]]$
- (e)  $\neg[p \wedge q] \Rightarrow [\neg p \vee \neg q]$
- (f)  $[[p \wedge q] \Rightarrow r] \Rightarrow [[p \Rightarrow r] \vee [q \Rightarrow r]]$
- (g)  $[p \wedge q] \Rightarrow \neg[\neg p \vee \neg q]$
- (h)  $\neg[p \Rightarrow q] \Rightarrow [q \Rightarrow p]$
- (i)  $[p \Rightarrow q] \vee [q \Rightarrow r]$

Use *only* the proof techniques from Lecture 2. Especially, do *not use* truth tables, equivalence rules or any other proof technique that you learned in other courses. The only exception is the following: You may use commutativity of conjunction and disjunction, and for a formula  $\phi$  you may use the fact that  $\phi$  and  $\neg\neg\phi$  are equivalent and hence can be replaced by each other. Remember that in the lecture we discussed an important technique how to use the equivalence between  $\phi$  and  $\neg\neg\phi$ . Also do not forget that there are various situations where lemmata can be useful (see the corresponding part of lecture which also contains an example on how to use lemmas).

As explained in the lecture, it is absolutely necessary that you apply proof rules *only* to the outer-most symbol of formulas. We will not accept any solution that violates this principle.

If you want, you may also try the proof assistant at <https://fme-proof-assistant.netlify.app>, which implements the proof techniques needed for this exercise. This application is the result of a diploma thesis and at this point it still has certain flaws. However, it is very useful for checking whether you apply the proof rules from the lecture correctly.

(9 × 0.5 points)