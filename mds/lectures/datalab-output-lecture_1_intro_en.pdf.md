

# Formal Methods and Specification (SS 2025/26)Lecture 1: Introduction

Stefan Ratschan

Czech Technical University in Prague

![Coat of arms of the Czech Republic, featuring a lion holding a sword and a shield.](64662465bba247703fdec49c8f3309f9_img.jpg)

Coat of arms of the Czech Republic, featuring a lion holding a sword and a shield.

![Logos of the Operational Program Prague (OPP A), the City of Prague (PRAHA), and the European Union (EVROPSKÁ UNIE).](5fb340ad68b0c71df0b56698b137e35b_img.jpg)

Logos of the Operational Program Prague (OPP A), the City of Prague (PRAHA), and the European Union (EVROPSKÁ UNIE).

Evropský sociální fond Praha & EU: Investujeme do vaší budoucnosti

### Language/Jazyk

Bude-li přítomen aspoň jeden student, který neumí český,  
bude přednáška v angličtině

Budete mít k dispozici slajdy v obou jazycích

Budu vděčný za jakoukoli informaci o pravopisných chybách.

Zeptejte se!

Nebude-li přítomen žádný student, který neumí český,  
můžete si jazyk vybrat.

### Contact Information

Always:

- ▶ Stefan Ratschan
- ▶ <http://www.cs.cas.cz/~ratschan>  
(especially link “consultation hours”)
- ▶ [stefan.ratschan@fit.cvut.cz](mailto:stefan.ratschan@fit.cvut.cz)

Before/after MI-FME:

- ▶ FIT, A-1029

Rest of the time:

- ▶ Institute of Computer Science of the Czech Academy of Sciences
- ▶ Pod Vodárenskou věží 2
- ▶ Metro station Ládví

### Today

- ▶ Introduction
- ▶ Organizational Matters

Now: a catastrophic beginning.

#### Therac-25 (1985)

![Diagram of the Therac-25 therapy room layout.](a0167e3dcece9dcd8a378bcd98fb9cfa_img.jpg)

A 3D perspective diagram of a therapy room layout. The room contains a large Therac-25 unit on a turntable position monitor, a treatment table, and a control console. Various safety and monitoring components are labeled with lines pointing to their locations: TV camera, Beam on/off light, Door interlock switch, Room emergency switch, Display terminal, Motion enable switch (footswitch), TV monitor, Printer, Control console, Turntable position monitor, Room emergency switches, Motion power switch, and Therapy room intercom.

Diagram of the Therac-25 therapy room layout.

- ▶ Radiation therapy machine
- ▶ Original machine (therac-20) was almost entirely **mechanical**.
- ▶ Observation: a mechanical machine is dangerous (problems due to aging), so use **software** control
- ▶ Result: At least 6 **accidents** with over-radiation, some of them deadly.

#### Ariane 5 Flight 501 (1996)

![Ariane 5 rocket launch with ESA logo on the side.](afe5eb459b7c9cfe880b067777d876d8_img.jpg)

A photograph of the Ariane 5 rocket launching. The rocket is white with two boosters. A large plume of white smoke and fire is at the base. In the background, there is a tall white building with the ESA logo and the word 'ariane' on it. Two tall, thin lattice towers stand on either side of the launch pad. The sky is clear blue.

Ariane 5 rocket launch with ESA logo on the side.

![Ariane 5 rocket exploding in the sky.](8e592c58b5074d79831ff650c2c636df_img.jpg)

A photograph of the Ariane 5 rocket exploding in the sky. A thick, dark grey plume of smoke rises vertically from the ground. At the top of the plume, a large, bright, starburst-like explosion occurs, with many small sparks and fragments of the rocket scattered in the air. The sky is a deep blue, suggesting dusk or dawn.

Ariane 5 rocket exploding in the sky.

- ▶ The flight ended with an **explosion** directly after its start
- ▶ Costs: 290 M€
- ▶ Program delayed by one year

#### Northeast Blackout of 2003

![Satellite image of the United States at night, showing a massive power outage in the Northeast and parts of Canada. The image is labeled '003 / 45 / 7844' in the top right, and 'ISAT GeoStar 45 23:15 EST 14 Aug. 2003' in the bottom left.](73c3e4508cae529acf4e6c7fa70b361a_img.jpg)

A satellite image of North America at night, showing the extent of the 2003 Northeast blackout. The image is dark, with city lights visible in the western and southern United States. A large, dark area in the Northeast and parts of Canada indicates a lack of artificial light. Text in the top right corner reads '003 / 45 / 7844'. Text in the bottom left corner reads 'ISAT GeoStar 45' and '23:15 EST 14 Aug. 2003'.

Satellite image of the United States at night, showing a massive power outage in the Northeast and parts of Canada. The image is labeled '003 / 45 / 7844' in the top right, and 'ISAT GeoStar 45 23:15 EST 14 Aug. 2003' in the bottom left.

- ▶ Power outage in a large part of the U.S.A. 14.-16.8.2003
- ▶ More than 10 direct fatalities
- ▶ Huge costs

### So What?

Connection with computer science?

- ▶ Each of the Therac-25 overdoses: result of a **software bug** originally excluded by a mechanical device
- ▶ Ariane 5 explosion:
  - ▶ result of an **overflow** in float to integer conversion
  - ▶ two redundant computers (same problem in both)
  - ▶ software function not need by Ariane 5 (different from version 4)
- ▶ Northeast blackout: **software bug** → alarm system failure, with earlier alarm response, no blackout

Bugs are often simple, but

- ▶ often occur in complicated unexpected situations, and
- ▶ may have catastrophic consequences because of complex chain reactions.

Difficult to **detect** by **testing**.

### Cyber-Physical Systems (CPS)

Integration of computation and physical processes

![Diagram of a car illustrating a Cyber-Physical System (CPS) architecture.](562f471e8153729557e6a4ee6343c32c_img.jpg)

The diagram shows a silver sedan with various electronic components and external connections labeled. On the left, a radio tower is labeled 'FM-RDS FM-SWIFT' and 'GPRS-Data GPRS'. A yellow lightning bolt points from the tower to the car's 'Board Monitor'. The car's interior contains a 'Digital Map on CD', 'CD-Reader', 'Dyno', 'Board Monitor', 'Voice Hand Device Interface', and 'Application Processing'. A 'Data-Bus' connects the 'Board Monitor', 'FM/DAB Radio', 'ABS', and 'Application Processing'. On the right, another radio tower is labeled 'GSM-Server GSM-SMS GSM-GPRS UNITS'. A yellow lightning bolt points from this tower to the car's 'Mobile Phone Transceiver'. A 'GPS' satellite is shown in the sky, with a yellow lightning bolt pointing to the car's 'Digital Map on CD'.

Diagram of a car illustrating a Cyber-Physical System (CPS) architecture.

“Cost of electronics in cars expected to move beyond 50% soon”  
[Emb, 2005].

Most computing power today not in desktop computers

### Safety Critical System

Not only,  
    increasingly tight integration  
        of computation and physical processes,  
but also  
    integration into daily **human life**

Extreme example: software implantable medical devices

**Malfunction** can **endanger** human life

**Correct** design essential

This course will contribute to this goal.

### Course Topic

*Formal Methods and Specification*

Formal? **mathematical precision**

### Example

```
result:= "sorted"  
for i:= 1 to 9 do  
  if a[i]>a[i+1] then  
    result:= "unsorted"  
  endif  
endfor  
return result
```

Is this algorithm **correct**?

- ▶ If we want to decide whether to cook dumplings or cabbage today?
- ▶ If we want to connect an iPad to the internet?
- ▶ If we want to know whether the array *a* is sorted?

Correctness of algorithm only makes sense  
in combination with a **specification**

### Specification

```
result:= "sorted"  
for i:= 1 to 9 do  
  if a[i]>a[i+1] then  
    result:= "unsorted"  
  endif  
endfor  
return result
```

Let's try to write a specification:

- ▶ Input: array  $a$  of length 10
- ▶ Output: if for all  $i$  in  $\{1, \dots, 9\}$ ,  $a[i] \leq a[i + 1]$  then "sorted"  
else "unsorted"

Attention: **side effects!** timing

For now, algorithms without side effects: **I/O specification**

### Well-Formed Specifications

Example:

- ▶ In: natural number  $k$
- ▶ Out: a prime factor of  $k$

I/O specifications

- ▶ may allow **more than one** correct **output** for one input, but
- ▶ must have **at least one output** for each input.

### Specification Languages

Specifications are usually written in **natural languages**  
(Czech, English, etc.)

English “eventually” vs. Czech “eventuálně”

anonymous Czech car manufacturer:

“each car will be connected to at most one mobile phone”

Problem:

- ▶ imprecision, ambiguity
- ▶ difficult to understand for computers

Solution: **artificial languages, predicate logic**

Further examples: see assignments

More precisely: first-order predicate logic +  
set theory, theory of integers, lists, arrays etc.

## Course Objective

### *Formal Methods and Specification*

Answer grand question:

*How to use **mathematical precision** when*

- ▶ ***specifying** the behavior of programs, and*
- ▶ ***writing software**, that fulfills its specification?*

But: Is this not just a **theoretical** exercise, that is **irrelevant** in **practice**?

### First Course Benefit: Brain Upgrade

The human brain developed a few **thousand years ago**, with a **different purpose**:

![A painting depicting three prehistoric men hunting a large bear with spears in a forest setting.](93afce28d7dec5b2202789b31b4ef8ab_img.jpg)

A painting depicting three prehistoric men hunting a large bear with spears in a forest setting. The bear is standing on its hind legs, and the men are using their spears to attack it. The scene is set in a dense forest with trees and foliage.

A painting depicting three prehistoric men hunting a large bear with spears in a forest setting.

Due to this historical purpose, the brain module easily makes **mistakes** in modern, and especially computer science life.

By training mathematical precision when reasoning about programs, we **improve** our **brain** for computer science life

Concrete benefit for you: You will be **better software engineers**.

#### Second Benefit: Practical Techniques

**Industry** uses formal methods more and more

**Semi-automatic partial correctness proofs** of computer chips  
routinely used in **hardware** industry [Kropf, 1999]  
motivated by Intel FDIV bug

**Software:** Prove “**small**” properties [D’Silva et al., 2008]

- ▶ correctness of compiler transformations
- ▶ absence of division by zero
- ▶ absence of array overflow
- ▶ ...

especially in so-called safety-critical applications

The course will help you to  
understand and use methods that are **used in practice**

#### Examples

- ▶ **Airbus** uses automatic methods for program verification  
<http://www.absint.de/astree/>
- ▶ **Microsoft's** "Windows Driver Kit" includes formal verification tool "Static Driver Verifier"
- ▶ Formal verification of **stock trading** algorithms <https://www.imandra.ai/>
- ▶ **BTC Embedded Systems AG**  
<https://www.btc-es.de>
- ▶ **Amazon** web services  
<https://doi.org/10.1145/2699417>
- ▶ **Facebook**  
[https://doi.org/10.1007/978-3-319-17524-9\\_1](https://doi.org/10.1007/978-3-319-17524-9_1)

Current research continuously **increases applicability**,  
in the near future, they will be in common usage.

#### Third Benefit: Formal Methods are Beautiful

![A complex, colorful, and highly detailed fractal image, resembling a mandala or a complex geometric pattern, centered on the slide.](33e70787e71eebc7a200bc7fe352dadc_img.jpg)

A complex, colorful, and highly detailed fractal image, resembling a mandala or a complex geometric pattern, centered on the slide. The image features a central circular motif surrounded by multiple layers of intricate, symmetrical designs. The colors are vibrant and varied, including shades of orange, yellow, pink, and blue. The overall effect is one of mathematical beauty and complexity.

A complex, colorful, and highly detailed fractal image, resembling a mandala or a complex geometric pattern, centered on the slide.

### What this Course is Not About

Complete formalization of the software development process:

- ▶ B-method
- ▶ Z-notation

Despite many years of research still only used in very specific applications.

But: the course will also help you to understand such methods.

### Prerequisites: Logic

Incomplete list of important notions: term, arity, formula, sentence, free/bound variables, substitution, interpretation, variable assignment,  $\models$ , validity, satisfiability, equivalence, equivalence rules

See

- ▶ basic undergraduate courses
- ▶ MI-TES: lecture “basics of practical logic”
- ▶ Lectures notes “Formal Modeling and Automatic Analysis of Complex Systems” (chapter 1, except for section “a complete proof calculus”)

Conventions:

- ▶ Tautology:  $\top$ , contradiction:  $\perp$
- ▶ Quantifiers:  $\exists x . p(x) \wedge q(x)$  means  $\exists x . [p(x) \wedge q(x)]$  and not  $[\exists x . p(x)] \wedge q(x)$
- ▶ Substitution of term  $t$  for the variable  $x$ :  $A[x \leftarrow t]$   
(e.g.,  $x + y[y \leftarrow x^2]$  is  $x + x^2$ )

#### Notation

FIT-notation

In practice: wide variety!

One of most important capabilities

ability to understand concepts independently from notation.

### Proofs

Simple proofs: equivalence rules

Example:  $\perp \Rightarrow A$  is equivalent to  $\top$

(lecture notes, section 1.5)

Not everything can be proved on the basis of equivalence rules  
(the method is not complete).

Next lecture: **general proof method**

### Preliminary Lecture Plan

1. Introduction
2. A complete proof method
3. Logical theories and modeling of data structures
4. Correctness of Linear Program
5. Correctness of Programs With Control Structures
6. Function, procedures, object-oriented programming
7. Symbolic execution
8. ...

2-3: theoretical basis

4-: formal methods

### Course Organization

*Aurea prima sata est aetas, quae vindice nullo,  
sponte sua, sine lege fidem rectumque colebat.  
Poena metusque aberant nec verba minantia fixo  
aere legebantur, nec supplex turba timebat  
iudicis ora sui, sed erant sine vindice tuti.*

... education in the past ...

### Traditional Education

Conditions:

- ▶ external **memory** (i.e., books) **expensive** and difficult to access
- ▶ **few** important **documents** (Homer, Platon, Ovid, bible)
- ▶ almost **no changes** of scientific **knowledge** during one's lifetime
- ▶ **computer**= a **person** that computes
- ▶ people in the hands of **one ruler** (emperor, leader, party, etc.),  
truth controlled by **authorities** (church, party, etc.)

Those conditions are **gone**,  
but traditional education not.

### Year 2025/26

- ▶ external memory (i.e., books) expensive and difficult to access  
*All books written before 1900 can be stored on **one disk***
- ▶ limited amount of important documents (Homer, Platon, Ovid, bible)  
*Without even leaving our bed we have access to **more information** than our grand-parents during their whole life*
- ▶ almost no changes of scientific knowledge during one's lifetime  
*Common technologies become **obsolete** within a few years, **innovation** is key to prosperity (innovation society)*
- ▶ computer = a person that computes  
*One mobile **phone** computes **faster** than all computers (i.e., people) of the 19th century*
- ▶ people in the hands of one ruler (emperor, leader, party, etc.), truth controlled by authorities (church, party, etc.)  
*Every **individual** bears **responsibility** for society, the political system needs **independent thinking, critical and courageous** people*

### Consequences

- ▶ The problem is not information access, but abundance of information.  
*Instead of primitive memorization, we need the ability to process and organize information.*
- ▶ Whatever knowledge we learn, we will need something else.  
*Key is the ability to orient oneself in new situations*
- ▶ Almost everything becomes outdated in short time.  
*We must concentrate on a few essential things that will stay relevant through our whole life, on abilities instead of knowledge.*
- ▶ It does not make sense to blindly learn algorithms  
(e.g., integration, solving linear systems of equations).  
*Concentrate on things beyond the abilities of computers: creativity, innovation, independent (but still precise) thinking*
- ▶ We need abilities of *understanding, discussion, and communication*

Concrete consequences for this course?

### Final Exam

**Open book:** You will be allowed to use any book, printouts of slides etc.

- ▶ “o.k., so I’ll just relax during the semester,  
at the exam I will anyway be able to find the answers in books”
- ▶ “na cvika asi budu muset, ale **na přednášku kašlu**”

*This will not work!*

Two types of problems to solve at the final exam:

- ▶ Problems of the same or similar type  
as the assignments during the semester.
- ▶ Problem, that test whether the student is able to apply the theory from the lecture on his/her own in a **new** context

... which is difficult!

But if you work during the whole semester, then less so.

### Work During the Semester

Typical didactical steps:

1. small example in lecture
2. theory (generalization of the example)
3. homework: bigger example
4. tutorial: check, discuss

Everything builds on the **lecture!**

If step 1 is unclear to you, you will not be able to do the rest!

Especially, you will have problems with doing your homework

There will be no time during tutorials to  
explain the lecture to people who did not attend it

Ask, ask, ask, ask, ask ... !

### Sources

Slides online (courses)

I will **not** follow a certain textbook

On the slides of the individual lectures I will provide references to sources

Lecture notes (under development)

You will not need them if you attend the lectures

If you miss some lectures, you will need additional sources

Lectures slides will not contain all information,

I will draw examples, figures etc. onto the whiteboard

Be careful with Wikipedia!

- ▶ A large part of the lecture material is not there.  
(this is a master's course, where more advanced things are taught)
- ▶ If it is there, it often contains mistakes, bad explanations etc.

### Help Needed

Feedback!

During the semester, I can still react ...

### Literature I

Study of worldwide trends and R&D programmes in embedded systems in view of maximising the impact of a technology platform in the area. Report for the European Commission, 2005.

V. D'Silva, D. Kroening, and G. Weissenbacher. A survey of automated techniques for formal software verification. *IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems*, 27(7):1165–1178, July 2008.

Thomas Kropf. *Introduction to Formal Hardware Verification*. Springer, 1999.