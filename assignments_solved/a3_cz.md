# Zadání 3 — Formální metody a specifikace

---

## Cvičení 1 — Důkazy v teorii polí

### (a) $\forall x \exists a . a[0] = x$ (ukázkový)

**Důkaz.** Nechť $x$ je libovolné, ale pevné. Dokazujeme $\exists a . a[0] = x$.

Pro existenční kvantifikátor musíme zvolit term pro $a$ a dokázat výslednou formuli. Zvolíme $a = \text{write}(b, 0, x)$, kde $b$ je libovolné pole.

Potřebujeme dokázat $\text{write}(b, 0, x)[0] = x$.

Z axiomu polí $\forall a, v, i, j . i = j \Rightarrow \text{write}(a, i, v)[j] = v$, volbou $a \leftarrow b$, $v \leftarrow x$, $i \leftarrow 0$, $j \leftarrow 0$. Dostáváme:

$$0 = 0  \Rightarrow \text{write}(b, 0, x)[0] = x$$

Protože $0 = 0$ platí (reflexivita rovnosti), vyvodíme $\text{write}(b, 0, x)[0] = x$. Hotovo. ■

---

### (b) $\forall i \exists a . a[i] = 2a[i + 1]$

**Důkaz.** Nechť $i$ je libovolné, ale pevné. Dokazujeme $\exists a . a[i] = 2a[i + 1]$.

Potřebujeme zkonstruovat pole $a$, které toto splňuje. Zvolíme:
$$a = \text{write}(\text{write}(b, i, 2c), i+1, c)$$
kde $b$ je libovolné pole a $c$ je libovolné celé číslo (např. $c = 0$). Toto pole má $2c$ na indexu $i$ a $c$ na indexu $i+1$.

Nyní dokážeme $a[i] = 2a[i+1]$.

**Levá strana — $a[i]$:**
$$a[i] = \text{write}(\text{write}(b, i, 2c), i+1, c)[i]$$

Protože $i \neq i+1$, z axiomu polí pro čtení na jiném indexu:
$$\text{write}(\text{write}(b, i, 2c), i+1, c)[i] = \text{write}(b, i, 2c)[i]$$

Protože $i = i$, z axiomu pro čtení na stejném indexu:
$$\text{write}(b, i, 2c)[i] = 2c$$

Takže $a[i] = 2c$.

**Pravá strana — $2a[i+1]$:**
$$a[i+1] = \text{write}(\text{write}(b, i, 2c), i+1, c)[i+1]$$

Protože $i+1 = i+1$, z axiomu pro čtení na stejném indexu:
$$\text{write}(\text{write}(b, i, 2c), i+1, c)[i+1] = c$$

Takže $a[i+1] = c$ a $2a[i+1] = 2c$.

Proto $a[i] = 2c = 2a[i+1]$. Hotovo. ■

---

### (c) $\exists a, i, x . \text{write}(a, i, x)[0] = x + 3$

**Důkaz.** Pro důkaz trojitého existenčního kvantifikátoru musíme zvolit konkrétní hodnoty pro $a$, $i$ a $x$.

Zvolíme:
- $i = 1$ (nebo libovolné $i \neq 0$)
- $x$: libovolné celé číslo
- $a = \text{write}(b, 0, x+3)$, kde $b$ je libovolné pole

Nyní dokážeme $\text{write}(\text{write}(b, 0, x+3), 1, x)[0] = x + 3$.

Protože $1 \neq 0$, z axiomu polí pro čtení na jiném indexu:
$$\text{write}(\text{write}(b, 0, x+3), 1, x)[0] = \text{write}(b, 0, x+3)[0]$$

Protože $0 = 0$, z axiomu pro čtení na stejném indexu:
$$\text{write}(b, 0, x+3)[0] = x + 3$$

Proto $\text{write}(a, i, x)[0] = x + 3$. Hotovo. ■

---

## Cvičení 2 — SSA a verifikační podmínky

### (a) **assume** $x \geq 0$ ; $y \leftarrow x$ ; $z \leftarrow y$ ; $@ z \geq 0$

**SSA:** Program již má každou proměnnou přiřazenou nejvýše jednou, takže přejmenování není nutné. V SSA tvaru:

$$\text{assume } x \geq 0; y_1 \leftarrow x; z_1 \leftarrow y_1; @ z_1 \geq 0$$

**Verifikační podmínka:**
$$[x \geq 0 \wedge y_1 = x \wedge z_1 = y_1] \Rightarrow z_1 \geq 0$$

**Ověření:** Z předpokladů $z_1 = y_1 = x$ a $x \geq 0$, proto $z_1 \geq 0$. **VP platí.** ■

---

### (b) **assume** $x \geq 0$ ; $z \leftarrow y$ ; $y \leftarrow x$ ; $@ z \geq 0$

**SSA:**
$$\text{assume } x \geq 0; z_1 \leftarrow y; y_1 \leftarrow x; @ z_1 \geq 0$$

**Verifikační podmínka:**
$$[x \geq 0 \wedge z_1 = y \wedge y_1 = x] \Rightarrow z_1 \geq 0$$

**Ověření:** VP NEPLATÍ. Proměnná $y$ (původní hodnota) není nijak omezena — může být libovolné celé číslo a $z_1 = y$. Nic neříká, že $z_1 \geq 0$.

**Protipříklad:** $\{x \mapsto 5, y \mapsto -3, z_1 \mapsto -3, y_1 \mapsto 5\}$

Předpoklady $5 \geq 0$, $-3 = -3$ a $5 = 5$ platí, ale $z_1 = -3 \not\geq 0$. Tomu odpovídá vstup $x = 5, y = -3$ — program by selhal na aserci. ■

---

### (c) $x \leftarrow x - k$ ; **assume** $k \leq x$ ; $@ x \geq 0$

**SSA:** $x$ je přepsáno, přejmenujeme všechna následující použití:

$$x_1 \leftarrow x - k; \text{assume } k \leq x_1; @ x_1 \geq 0$$

**Verifikační podmínka:**
$$[x_1 = x - k \wedge k \leq x_1] \Rightarrow x_1 \geq 0$$

**Ověření:** VP NEPLATÍ. Z $x_1 = x - k$ a $k \leq x_1$ víme $k \leq x - k$, tj. $2k \leq x$. Ale $x_1$ může být přesto záporné. Např. pro $k = -5$ a $x = -8$:

$x_1 = -8 - (-5) = -3$, a $k \leq x_1$ je $-5 \leq -3$ (pravda), ale $x_1 = -3 \not\geq 0$.

**Protipříklad:** $\{x \mapsto -8, k \mapsto -5, x_1 \mapsto -3\}$ ■

---

### (d) **assume** $x \geq 0$ ; $y \leftarrow x$ ; **input** $x$ ; $@ x \geq 0$

**SSA:** **input** vytvoří novou hodnotu pro $x$, přejmenujeme následující $x$:

$$\text{assume } x \geq 0; y_1 \leftarrow x; \text{input } x_1; @ x_1 \geq 0$$

**Verifikační podmínka:**
$$[x \geq 0 \wedge y_1 = x] \Rightarrow x_1 \geq 0$$

**Ověření:** VP NEPLATÍ. $x_1$ pochází z uživatelského vstupu a nesouvisí s předpoklady o $x$ a $y_1$. Jakékoli záporné $x_1$ poruší aserci.

**Protipříklad:** $\{x \mapsto 10, y_1 \mapsto 10, x_1 \mapsto -4\}$ — všechny předpoklady platí, ale $x_1 = -4 \not\geq 0$. ■

---

### (e) $y \leftarrow x$ ; **input** $x$ ; **assume** $x \geq 0$ ; $@ y \geq 0$

**SSA:** $y$ i $x$ dostávají nové hodnoty:

$$y_1 \leftarrow x; \text{input } x_1; \text{assume } x_1 \geq 0; @ y_1 \geq 0$$

**Verifikační podmínka:**
$$[y_1 = x \wedge x_1 \geq 0] \Rightarrow y_1 \geq 0$$

**Ověření:** VP NEPLATÍ. $y_1 = x$ (původní hodnota), která není omezená. $x_1 \geq 0$ neříká nic o původním $x$.

**Protipříklad:** $\{x \mapsto -7, y_1 \mapsto -7, x_1 \mapsto 3\}$ — $x_1 = 3 \geq 0$ platí, ale $y_1 = -7 \not\geq 0$. ■

---

### (f) $a[i] \leftarrow 7$ ; $i \leftarrow i + 1$ ; $a[i] \leftarrow 8$ ; $@ i \geq a[i]$

**SSA:** Přiřazení do pole používají funkci $\text{write}$. Každé přiřazení vytvoří novou verzi pole:

$$a_1 \leftarrow \text{write}(a, i, 7); i_1 \leftarrow i + 1; a_2 \leftarrow \text{write}(a_1, i_1, 8); @ i_1 \geq a_2[i_1]$$

**Verifikační podmínka:**
$$[a_1 = \text{write}(a, i, 7) \wedge i_1 = i + 1 \wedge a_2 = \text{write}(a_1, i_1, 8)] \Rightarrow i_1 \geq a_2[i_1]$$

**Ověření:** VP NEPLATÍ. Z $a_2 = \text{write}(a_1, i_1, 8)$ a axiomu polí pro čtení na stejném indexu dostáváme $a_2[i_1] = 8$. VP požaduje $i_1 \geq 8$, tj. $i+1 \geq 8$, ale $i$ není omezeno.

**Protipříklad:** $\{i \mapsto 0, i_1 \mapsto 1, a_2[i_1] \mapsto 8\}$ — všechny předpoklady platí, ale $1 \not\geq 8$. ■

---

## Cvičení 3 — Program s cyklem (součet prvků pole)

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

Všechny proměnné nabývají hodnot z $\mathbb{N}$ (včetně nuly). Prázdný součet $\sum_{k=0}^{-1} \dots = 0$.

### (a) Základní cesty

Invariant cyklu je `@ I`, kde $I \equiv x = x_0 + \sum_{k=0}^{i-1} a[k]$. Závěrečná aserce je $F \equiv x = x_0 + \sum_{k=0}^{n-1} a[k]$.

**Cesta 1** (vstup do cyklu ze začátku):

$$
\begin{aligned}
& x_0 \leftarrow x \\
& i \leftarrow 0 \\
& \text{assume } i < n \\
& @ I
\end{aligned}
$$

**Cesta 2** (přeskočení cyklu — $i < n$ je nepravdivé na začátku):

$$
\begin{aligned}
& x_0 \leftarrow x \\
& i \leftarrow 0 \\
& \text{assume } \neg(i < n) \\
& @ F
\end{aligned}
$$

**Cesta 3** (opětovný vstup do cyklu — z invariantu přes tělo cyklu zpět k invariantu):

$$
\begin{aligned}
& \text{assume } I \\
& \text{assume } i < n \\
& x \leftarrow x + a[i] \\
& i \leftarrow i + 1 \\
& @ I
\end{aligned}
$$

**Cesta 4** (opuštění cyklu — z invariantu přes tělo cyklu k závěrečné aserci):

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

### (b) Verifikační podmínky

**Cesta 1 — SSA:**
$$x_{01} \leftarrow x; i_1 \leftarrow 0; \text{assume } i_1 < n; @ x = x_{01} + \sum_{k=0}^{i_1-1} a[k]$$

$$VC_1: [x_{01} = x \wedge i_1 = 0 \wedge i_1 < n]  \Rightarrow x = x_{01} + \sum_{k=0}^{i_1-1} a[k]$$

S $i_1 = 0$ je součet $\sum_{k=0}^{-1} a[k] = 0$. S $x_{01} = x$ je pravá strana $x + 0 = x$. $VC_1$ se zjednoduší na $x = x$, což je triviálně pravda.

**Cesta 2 — SSA:**
$$x_{01} \leftarrow x; i_1 \leftarrow 0; \text{assume } \neg(i_1 < n); @ x = x_{01} + \sum_{k=0}^{n-1} a[k]$$

$$VC_2: [x_{01} = x \wedge i_1 = 0 \wedge i_1 \geq n]  \Rightarrow x = x_{01} + \sum_{k=0}^{n-1} a[k]$$

$i_1 = 0$ a $i_1 \geq n$ implikuje $n = 0$ (protože $n \in \mathbb{N}$). Součet $\sum_{k=0}^{-1} a[k] = 0$ a $x = x_{01} + 0 = x$. Platí.

**Cesta 3 — SSA:**
$$\text{assume } x = x_0 + \sum_{k=0}^{i-1} a[k]; \text{assume } i < n; x_1 \leftarrow x + a[i]; i_1 \leftarrow i + 1; @ x_1 = x_0 + \sum_{k=0}^{i_1-1} a[k]$$

$$VC_3: \left[ x = x_0 + \sum_{k=0}^{i-1} a[k]  \wedge i < n  \wedge x_1 = x + a[i]  \wedge i_1 = i + 1 \right]  \Rightarrow x_1 = x_0 + \sum_{k=0}^{i_1-1} a[k]$$

$x_1 = x + a[i] = x_0 + \sum_{k=0}^{i-1} a[k] + a[i]$. Součet $\sum_{k=0}^{i_1-1} a[k] = \sum_{k=0}^{i} a[k] = \sum_{k=0}^{i-1} a[k] + a[i]$.
Tedy $x_0 + \sum_{k=0}^{i_1-1} a[k] = x_0 + \sum_{k=0}^{i-1} a[k] + a[i] = x_1$. Platí.

**Cesta 4 — SSA:**
$$\text{assume } x = x_0 + \sum_{k=0}^{i-1} a[k]; \text{assume } i < n; x_1 \leftarrow x + a[i]; i_1 \leftarrow i + 1; \text{assume } \neg(i_1 < n); @ x_1 = x_0 + \sum_{k=0}^{n-1} a[k]$$

$$VC_4: \left[ x = x_0 + \sum_{k=0}^{i-1} a[k]  \wedge i < n  \wedge x_1 = x + a[i]  \wedge i_1 = i + 1  \wedge i_1 \geq n \right]  \Rightarrow x_1 = x_0 + \sum_{k=0}^{n-1} a[k]$$

Z $i < n$ a $i+1 = i_1 \geq n$ dostáváme $i_1 = n$ (celá čísla). Tedy $i = n-1$.

$x_1 = x + a[n-1] = x_0 + \sum_{k=0}^{n-2} a[k] + a[n-1] = x_0 + \sum_{k=0}^{n-1} a[k]$. Platí.

---

### (c) Všechny čtyři verifikační podmínky platí.

### (d) Není potřeba — všechny VP již platí. Žádná úprava není nutná.

---

## Cvičení 4 — Program s cyklem for

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

Všechny proměnné nabývají hodnot z $\mathbb{Z}$. $\text{sgn}(n) = 1$ pro $n > 0$, $\text{sgn}(n) = -1$ pro $n < 0$.

### (a) Základní cesty

Označme:
- $I_1 \equiv k = n + li \wedge l = -\text{sgn}(n)$ (první invariant)
- $I_2 \equiv k = n + l(i+1) \wedge l = -\text{sgn}(n)$ (druhý invariant)
- $F \equiv k = 0$ (závěrečná aserce)

For cyklus `for i ← 0 to |n|-1`:
- Vstup zvenku: assume $i = 0 \wedge 0 \leq |n|-1$
- Přeskočení: assume $\neg(0 \leq |n|-1)$
- Opětovný vstup: $i \leftarrow i + 1$; assume $i \leq |n|-1$
- Opuštění: $i \leftarrow i + 1$; assume $\neg(i \leq |n|-1)$
- Navíc uvnitř těla cyklu můžeme předpokládat $0 \leq i \leq |n|-1$.

**Cesta 1** (začátek přes `if` větev $n < 0$ k prvnímu invariantu):

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

**Cesta 2** (začátek přes `else` větev $n > 0$ k prvnímu invariantu):

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

**Cesta 3** (začátek přes `if` větev, přeskočení for cyklu):

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

**Cesta 4** (začátek přes `else` větev, přeskočení for cyklu):

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

**Cesta 5** (tělo cyklu: z prvního invariantu k druhému):

$$
\begin{aligned}
& \text{assume } 0 \leq i \leq |n| - 1 \wedge I_1 \\
& k \leftarrow k + l \\
& @ I_2
\end{aligned}
$$

**Cesta 6** (opětovný vstup do for cyklu z druhého invariantu):

$$
\begin{aligned}
& \text{assume } 0 \leq i \leq |n| - 1 \wedge I_2 \\
& i \leftarrow i + 1 \\
& \text{assume } i \leq |n| - 1 \\
& @ I_1
\end{aligned}
$$

**Cesta 7** (opuštění for cyklu z druhého invariantu):

$$
\begin{aligned}
& \text{assume } 0 \leq i \leq |n| - 1 \wedge I_2 \\
& i \leftarrow i + 1 \\
& \text{assume } \neg(i \leq |n| - 1) \\
& @ F
\end{aligned}
$$

---

### (b) Ověření správnosti

**Cesta 1:** $n < 0$, takže $\text{sgn}(n) = -1$, $-\text{sgn}(n) = 1 = l$. $k = n$, $i = 0$.
$I_1$: $k = n + l \cdot 0 = n$. $l = 1 = -\text{sgn}(n)$. Obě platí. ✓

**Cesta 2:** $n > 0$ (z $\neg(n<0)$ a $n \neq 0$), takže $\text{sgn}(n) = 1$, $l = -1 = -\text{sgn}(n)$. $k = n$, $i = 0$.
$I_1$: $k = n + (-1) \cdot 0 = n$. $l = -1 = -\text{sgn}(n)$. Obě platí. ✓

**Cesta 3:** $n < 0$, $l = 1$. $\neg(0 \leq |n|-1)$ znamená $|n|-1 < 0$, takže $|n| = 0$, $n = 0$. Ale máme $n \neq 0$ — spor. Cesta je **vakuózně správná** (nedosažitelná). ✓

**Cesta 4:** $n > 0$, $l = -1$. $\neg(0 \leq |n|-1)$ znamená $|n| = 0$, takže $n = 0$ — spor s $n \neq 0$. **Vakuózně správná.** ✓

**Cesta 5:** $k = n + li$, $k \leftarrow k + l$. Nové $k_1 = n + li + l = n + l(i+1)$. $l$ se nezměnilo. $I_2$ platí. ✓

**Cesta 6:** $k = n + l(i+1)$. $i \leftarrow i + 1$, takže $i_1 = i + 1$. Předpoklad $i_1 \leq |n|-1$ (kontrola opětovného vstupu, splněna). $I_1$ požaduje $k = n + l \cdot i_1$. Máme $k = n + l(i+1) = n + l \cdot i_1$. A $l = -\text{sgn}(n)$. Obě platí. ✓

**Cesta 7:** $k = n + l(i+1)$. $i \leftarrow i+1$, $i_1 = i+1$. Předpoklad $i_1 > |n|-1$ (kontrola opuštění). Z předpokladu těla cyklu $0 \leq i \leq |n|-1$ máme $i_1 = i+1 \leq |n|$. S $i_1 > |n|-1$ dostáváme $i_1 = |n|$.

$F$ požaduje $k = 0$:
$$k = n + l \cdot |n| = n + (-\text{sgn}(n)) \cdot |n| = n - \text{sgn}(n) \cdot |n|$$

Protože $\text{sgn}(n) \cdot |n| = n$ (z definice znaménka a absolutní hodnoty, s $n \neq 0$):
$$k = n - n = 0 \quad \checkmark$$

Všechny základní cesty jsou správné. ■
