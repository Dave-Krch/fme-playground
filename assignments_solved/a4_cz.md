# Zadání 4 — Formální metody a specifikace

---

## Cvičení 1 — Násobení opakovaným sčítáním

### Vstupně/výstupní specifikace

- **Vstup:** $x, y \in \mathbb{N}$ takové, že $y \geq 1$
- **Výstup:** $x \cdot y$

### Doplněný program

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

Tři doplněné pozice:
1. `assume y ≥ 1` — kontroluje vstupní požadavek.
2. `@ r = x · i` — invariant cyklu.
3. `@ r = x · y` — kontroluje výstupní požadavek (term `xy` převeden na formuli `r = x·y`).

### Tabulka průchodu

Vstup: $x = 3$, $y = 5$

$\begin{array}{c|c|c}
\text{Iterace} & r \text{ v invariantu } @ & i \text{ v invariantu } @ \\ \hline
\text{vstup (po inicializaci)} & 3 & 1 \\
\text{po 1. těle cyklu} & 6 & 2 \\
\text{po 2. těle cyklu} & 9 & 3 \\
\text{po 3. těle cyklu} & 12 & 4 \\
\text{po 4. těle cyklu} & 15 & 5 \\
\text{(cyklus končí)} & 15 & 5
\end{array}$

Na konci: $r = 15 = 3 \cdot 5 = x \cdot y$. ✓

### Základní cesty a kontrola správnosti

**Cesta 1** (vstup do cyklu):
```
assume y ≥ 1
r ← x; i ← 1
assume i < y
@ r = x·i
```
S $r = x$, $i = 1$: $r = x = x \cdot 1 = x \cdot i$. Správně. ✓

**Cesta 2** (přeskočení cyklu):
```
assume y ≥ 1
r ← x; i ← 1
assume ¬(i < y)
@ r = x·y
```
$i = 1$, $\neg(1 < y)$ znamená $y \leq 1$. S $y \geq 1$ dostáváme $y = 1$. $r = x = x \cdot 1 = x \cdot y$. Správně. ✓

**Cesta 3** (opětovný vstup, přes tělo cyklu zpět k invariantu):
```
assume r = x·i
assume i < y
r ← r + x; i ← i + 1
assume i < y
@ r = x·i
```
$r_{nové} = r + x = x\cdot i + x = x\cdot(i+1) = x \cdot i_{nové}$. Správně. ✓

**Cesta 4** (opuštění cyklu, přes tělo cyklu k výstupu):
```
assume r = x·i
assume i < y
r ← r + x; i ← i + 1
assume ¬(i < y)
@ r = x·y
```
$i_{nové} = i+1$. Z $\neg(i_{nové} < y)$ a $i < y$: $i_{nové} = y$, takže $i = y-1$.
$r_{nové} = r + x = x\cdot(y-1) + x = x \cdot y$. Správně. ✓

Všechny čtyři základní cesty jsou správné. Algoritmus je správný.

---

## Cvičení 2 — Rozdělení pole

### (a) Lze najít vstup způsobující selhání aserce?

Program implementuje standardní algoritmus rozdělení pole pomocí dvou ukazatelů: zleva hledá prvky $> 0$ a zprava prvky $\leq 0$, přičemž zaměňuje prvky na špatných místech. Když cyklus skončí ($l > u$), pole je správně rozděleno.

**Žádný vstup nezpůsobí selhání aserce.** Algoritmus je správný — když $l > u$, všechny indexy $< l$ obsahují prvky $\leq 0$ a všechny indexy $> u$ obsahují prvky $> 0$. Protože $l > u$, bod rozdělení je $k = l$ a $\mathrm{part}(a, n, l)$ platí.

### (b) Kontrola všech 12 základních cest

Všechny cesty používají pouze `perm(ain, a, n)` jako invariant cyklu.

- **Cesta 1** (vstup, assume $l \leq u$, @ inv): ANO. Po $a_{in} \leftarrow a$, $l \leftarrow 0$, $u \leftarrow n-1$, bez mutace $a$, takže $\mathrm{perm}(a_{in}, a, n)$ triviálně platí.
- **Cesta 2** (přeskočení, assume $\neg(l \leq u)$, @ final): ANO. Dosažitelná jen když $0 > n-1$, tj. $n = 0$. Pro prázdné pole obě $\mathrm{perm}$ i $\exists k. \mathrm{part}$ platí s $k = 0$.
- **Cesta 3** (prohození provedeno, @ inv): ANO. Prohození dvou prvků zachovává permutaci.
- **Cesta 4** (opuštění, assume $\neg(l \leq u)$, @ final): NE. Ze samotného $\mathrm{perm}(a_{in}, a, n)$ a $l > u$ nelze dokázat $\exists k. \mathrm{part}(a, n, k)$. Invariant je příliš slabý.
- **Cesty 5, 7, 9, 11** (bez prohození, @ inv): ANO. Žádná mutace $a$, $\mathrm{perm}$ zachováno.
- **Cesty 6, 8, 10** (prohození provedeno, @ inv): ANO. Prohození zachovává permutaci.

### (c) Explicitní bod rozdělení

Když cyklus skončí ($l > u$), bod rozdělení je $k = l$. Všechny prvky $< l$ jsou $\leq 0$ a všechny prvky $\geq l$ jsou $> 0$.

Závěrečnou aserci nahradíme:
```
@ perm(ain, a, n) ∧ part(a, n, l)
```

### (d) Posílený invariant cyklu

Aby byly všechny cesty správné, rozšíříme invariant cyklu o informaci o průběhu rozdělování:

```
@ perm(ain, a, n)
 ∧ (∀i ∈ {0, …, l-1}. a[i] ≤ 0)
 ∧ (∀i ∈ {u+1, …, n-1}. a[i] > 0)
 ∧ l ≤ u + 1
```

S tímto posíleným invariantem cesta 4 (opuštění) funguje: když $\neg(l \leq u)$, tj. $l > u$, pole je rozděleno s $k = l$. Všechny ostatní cesty jsou zachovány, protože přidané podmínky jsou udržovány logikou programu.

---

## Cvičení 3 — Specifikace funkce

### (a) Formule predikátové logiky pro specifikaci

Ve tvaru s výstupní proměnnou:

$$\forall a \in \mathcal{A}, k \in \mathcal{N}, r \in \mathcal{N}. [k \geq 1 \wedge r = s(a, k)] \Rightarrow r = 1 + \sum_{i=0}^{k-1} a[i]$$

Nebo ekvivalentně ve tvaru se jménem funkce:

$$\forall a \in \mathcal{A}, k \in \mathcal{N}. k \geq 1 \Rightarrow s(a, k) = 1 + \sum_{i=0}^{k-1} a[i]$$

### (b) Rozšířený program a verifikační podmínka

Program s předpokladem o funkci:
```
assume ∀a ∈ A, k ∈ N. k ≥ 1 ⇒ s(a, k) = 1 + Σ_{i=0}^{k-1} a[i]
assume ∀i. p[i] ≥ 4
r₁ ← s(p, 3)
@ r₁ ≥ 9
```

**Verifikační podmínka:**

$$[\forall a \in \mathcal{A}, k \in \mathcal{N}. k \geq 1 \Rightarrow s(a, k) = 1 + \sum_{i=0}^{k-1} a[i] \wedge \forall i. p[i] \geq 4 \wedge r_1 = s(p, 3)] \Rightarrow r_1 \geq 9$$

### (c) Platí VP?

**Ano.** Ze specifikace funkce s $a = p$, $k = 3$ (a $3 \geq 1$): $s(p, 3) = 1 + p[0] + p[1] + p[2]$. Z $\forall i. p[i] \geq 4$: $p[0] \geq 4$, $p[1] \geq 4$, $p[2] \geq 4$. Takže $s(p, 3) \geq 1 + 4 + 4 + 4 = 13$. Protože $r_1 = s(p, 3)$, máme $r_1 \geq 13 \geq 9$. Platí.

### (d) Důkaz

- Ze vzorce pro funkci, dosazením $a \leftarrow p$, $k \leftarrow 3$ a využitím $3 \geq 1$, dostáváme: $s(p, 3) = 1 + \sum_{i=0}^{2} p[i] = 1 + p[0] + p[1] + p[2]$.
- Z $\forall i. p[i] \geq 4$, instanciací $i = 0, 1, 2$: $p[0] \geq 4$, $p[1] \geq 4$, $p[2] \geq 4$.
- Sečtením: $s(p, 3) = 1 + p[0] + p[1] + p[2] \geq 1 + 4 + 4 + 4 = 13$.
- Protože $r_1 = s(p, 3)$ (z přiřazení), máme $r_1 \geq 13$.
- Z $13 \geq 9$ vyplývá $r_1 \geq 9$. ■

---

## Cvičení 4 — Překlad z OO do procedurální notace

Překlad tečkové notace do standardní funkční/predikátové notace:

```
x ← init(0, 1)
y ← x       // y je kopie x
x ← add(x, 7)     // x.add(7) modifikuje pouze x
y ← add(y, 1)     // y.add(1) modifikuje pouze y
assume fst(x) = snd(y)
@ snd(x) < 10
```

**Překladová tabulka:**
| OO notace | Procedurální notace |
|---|---|
| `init(n)` | $\mathrm{init}(n)$ |
| `x.add(v)` | $x \leftarrow \mathrm{add}(x, v)$ |
| `x.fst()` | $\mathrm{fst}(x)$ |
| `x.snd()` | $\mathrm{snd}(x)$ |

Každé volání metody, které modifikuje objekt, vytváří novou hodnotu přiřazenou zpět do proměnné.

---

## Cvičení 5 — Počítání nenulových prvků

### (a) Které verifikační podmínky platí?

Aktuální program má tyto aserce:
- $@ \top$ (invariant cyklu) — triviálně pravdivý, příliš slabý.
- $@ r = |\{k \in \{1, \dots, i\} \mid a[k] \neq 0\}|$ — **nesprávné**. $r$ počítá pouze záporné prvky (když $a[i] < 0$), ne všechny nenulové.
- $@ s = |\{k \in \{1, \dots, i\} \mid a[k] \neq 0\}|$ — **nesprávné**. $s$ počítá pouze kladné prvky (když $a[i] > 0$), ne všechny nenulové.
- $@ r + s = |\{k \in \{1, \dots, n\} \mid a[k] \neq 0\}|$ — bylo by správné, kdyby $r$ počítalo záporné a $s$ kladné, ale mezilehlé aserce jsou špatně.

**VP, které selhávají:** Základní cesty z prvního invariantu přes větev `if a[i] < 0` k druhé aserci selhávají, protože $r$ (které se zvyšuje jen pro záporné) se nerovná počtu všech nenulových. Podobně pro cesty přes `if a[i] > 0` a třetí aserci.

**VP, které platí:** Cesta opětovného vstupu ze třetí aserce přes inkrementaci for-cyklu zpět k $\top$ platí triviálně ($\top$ je vždy pravdivé).

### (b) Opravené aserce

Definujeme:
- $R(i) = |\{k \in \{1, \dots, i\} \mid a[k] < 0\}|$ (počet záporných do $i$)
- $S(i) = |\{k \in \{1, \dots, i\} \mid a[k] > 0\}|$ (počet kladných do $i$)

Platí $R(i) + S(i) = |\{k \in \{1, \dots, i\} \mid a[k] \neq 0\}|$.

Opravený program:

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

**Neformální kontrola všech VP:**

- **Vstup do for-cyklu:** $r=0, s=0, i=1$. $R(0) = 0, S(0) = 0$, takže $r = R(0) \wedge s = S(0)$ platí. ✓
- **První invariant → prostřední** (přes první `if`): pokud $a[i] < 0$, $r' = r+1 = R(i-1)+1 = R(i)$, $s$ beze změny $= S(i-1)$. Pokud $a[i] \geq 0$, $r' = r = R(i-1) = R(i)$ a $s = S(i-1)$. V obou případech $r = R(i) \wedge s = S(i-1)$ platí. ✓
- **Prostřední → spodní** (přes druhý `if`): podobně. Pokud $a[i] > 0$, $s' = s+1 = S(i-1)+1 = S(i)$. Pokud $a[i] \leq 0$, $s' = s = S(i-1) = S(i)$. V obou případech $r = R(i) \wedge s = S(i)$ platí. ✓
- **Opětovný vstup do for** (spodní → první, po $i \leftarrow i+1$, assume $i \leq n$): $i_{nové} = i+1$. $r = R(i) = R((i+1)-1) = R(i_{nové}-1)$. $s = S(i) = S(i_{nové}-1)$. Odpovídá prvnímu invariantu. ✓
- **Opuštění for** (spodní → závěrečná, po $i \leftarrow i+1$, assume $i > n$, tj. $i = n+1$): $r = R(n)$, $s = S(n)$. Pak $r + s = R(n) + S(n) = |\{k \in \{1, \dots, n\} \mid a[k] \neq 0\}|$. ✓
- **Přeskočení for** (počáteční $r = s = 0$, $1 > n$, takže $n = 0$): $r + s = 0 = |\{k \in \{1, \dots, 0\} \mid a[k] \neq 0\}|$. ✓

Všechny verifikační podmínky platí. Program sám nebyl změněn — opraveny byly pouze aserce.
