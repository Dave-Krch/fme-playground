# Zadání 2 — Formální metody a specifikace

---

## Cvičení 1 — Důkazy v predikátové logice

### (a) $[\forall x . P(x)] \Rightarrow [\forall x . P(f(x))]$ (ukázkový)

**Důkaz.** Předpokládáme $\forall x . P(x)$ a dokazujeme $\forall x . P(f(x))$.

Pro důkaz univerzálního kvantifikátoru zavedeme novou konstantu $a$ ("nechť $a$ je libovolné, ale pevné") a dokazujeme $P(f(a))$.

Z předpokladu $\forall x . P(x)$ vybereme term $t = f(a)$ a vyvodíme $P(f(a))$, což je přesně to, co jsme potřebovali dokázat. ■

---

### (b) $[\forall x . P(x) \wedge Q(x)] \Rightarrow [[\forall x . P(x)] \wedge [\forall x . Q(x)]]$

**Důkaz.** Předpokládáme $\forall x . P(x) \wedge Q(x)$ a dokazujeme $[\forall x . P(x)] \wedge [\forall x . Q(x)]$.

Pro důkaz konjunkce dokazujeme obě části zvlášť.

**První část** — dokázat $\forall x . P(x)$: zavedeme novou konstantu $a$ ("nechť $a$ je libovolné, ale pevné") a dokazujeme $P(a)$.

Z předpokladu $\forall x . P(x) \wedge Q(x)$ vybereme $t = a$ a vyvodíme $P(a) \wedge Q(a)$. Z této konjunkce vyvodíme $P(a)$, čímž je tato část hotova.

**Druhá část** — dokázat $\forall x . Q(x)$: opět s novou konstantou $a$, dokazujeme $Q(a)$.

Z $\forall x . P(x) \wedge Q(x)$ vybereme $t = a$, vyvodíme $P(a) \wedge Q(a)$ a z něj $Q(a)$. Tím je druhá část i celý důkaz hotov. ■

---

### (c) $[\exists x . P(f(x)) \vee Q(g(x))] \Rightarrow [[\exists x . P(x)] \vee [\exists x . Q(x)]]$

**Důkaz.** Předpokládáme $\exists x . P(f(x)) \vee Q(g(x))$ a dokazujeme $[\exists x . P(x)] \vee [\exists x . Q(x)]$.

Z předpokladu, jehož nejkrajnější symbol je $\exists$, zavedeme novou konstantu $a$ a přidáme $P(f(a)) \vee Q(g(a))$ jako známý fakt.

Nyní provedeme rozbor případů na disjunkci $P(f(a)) \vee Q(g(a))$:

**Případ $P(f(a))$:** Dokazujeme cíl $[\exists x . P(x)] \vee [\exists x . Q(x)]$ pomocí pravidla pro disjunkci: předpokládáme $\neg[\exists x . P(x)]$ a dokazujeme $\exists x . Q(x)$.

Dokážeme lemma: $\exists x . P(x)$ — výběrem termu $t = f(a)$ a důkazem $P(f(a))$, které je již známo.

Nyní $\exists x . P(x)$ je ve sporu s $\neg[\exists x . P(x)]$, což je spor. Spor dokazuje $\exists x . Q(x)$ (náš aktuální cíl). Tím je tento případ hotov.

**Případ $Q(g(a))$:** Symetricky dokazujeme disjunkci předpokladem $\neg[\exists x . Q(x)]$ a důkazem $\exists x . P(x)$. Lemma $\exists x . Q(x)$ (term $t = g(a)$, známé $Q(g(a))$) je ve sporu s $\neg[\exists x . Q(x)]$, což dokazuje $\exists x . P(x)$. Hotovo.

Oba případy uspěly, důkaz je hotov. ■

---

### (d) $[\exists x . S \Rightarrow Q(x)] \Rightarrow [S \Rightarrow \exists x . Q(x)]$

**Důkaz.** Předpokládáme $\exists x . S \Rightarrow Q(x)$ a dokazujeme $S \Rightarrow \exists x . Q(x)$.

Pro důkaz implikace předpokládáme $S$ a dokazujeme $\exists x . Q(x)$.

Z předpokladu $\exists x . S \Rightarrow Q(x)$ zavedeme novou konstantu $a$ tak, že známe $S \Rightarrow Q(a)$.

Ze známého faktu $S$ a $S \Rightarrow Q(a)$ vyvodíme $Q(a)$.

Pro důkaz $\exists x . Q(x)$ vybereme term $t = a$ a potřebujeme dokázat $Q(a)$, což již známe. Tím je důkaz hotov. ■

---

### (e) $[\neg \exists x \exists y . T(x, y)] \Rightarrow [\forall x . \neg T(f(g(x)), f(x))]$

**Důkaz.** Předpokládáme $\neg \exists x \exists y . T(x, y)$ a dokazujeme $\forall x . \neg T(f(g(x)), f(x))$.

Pro důkaz univerzálního kvantifikátoru zavedeme novou konstantu $a$ ("nechť $a$ je libovolné, ale pevné") a dokazujeme $\neg T(f(g(a)), f(a))$.

Pro důkaz negace předpokládáme $T(f(g(a)), f(a))$ a hledáme spor.

Z $T(f(g(a)), f(a))$ můžeme dokázat $\exists y . T(f(g(a)), y)$ výběrem $t = f(a)$ (protože $T(f(g(a)), f(a))$ platí). Poté dokážeme $\exists x \exists y . T(x, y)$ výběrem $t = f(g(a))$ pro $x$ a opětovným použitím $y = f(a)$.

Ale my již známe $\neg \exists x \exists y . T(x, y)$ — to je spor. Tím je důkaz hotov. ■

---

## Cvičení 2 — Certifikát pro palindrom (délky 7)

### (a) Neformální popis (přirozený jazyk)

Když řetězec **neobsahuje** žádný palindromický podřetězec délky 7, certifikátem je funkce (nebo pole), která pro každou možnou počáteční pozici $i$ podřetězce délky 7 udává pozici $j$ uvnitř tohoto podřetězce (kde $0 \le j \le 2$), na které se znaky na opačných, symetrických koncích liší. Konkrétně: certifikát poskytuje pro každé $i$ svědecký index $j$ takový, že $s[i+j] \neq s[i+6-j]$.

Pro ověření certifikátu stačí jeden průchod přes všechny počáteční pozice $i$ od $0$ do $\mathrm{len}(s)-7$ a ověření, že na svědecké pozici se dva symetrické znaky skutečně liší. Není potřeba žádný vnořený cyklus.

### (b) Formální popis (predikátová logika)

Rozšiřujeme výstupní specifikaci z 1. zadání, cvičení 2d. Nyní je výstupem dvojice $(r, c)$, kde $r \in \{0, 1\}$ a $c$ je certifikát.

Nechť $\mathcal{C}$ je typ pole (indexovaný přirozenými čísly, s přirozenými čísly jako prvky).

V případě $r = 0$ (žádný palindrom délky 7) musí certifikát $c$ splňovat:

$$\forall i \in \{0, \dots, \mathrm{len}(s)-7\} . c[i] \in \{0, 1, 2\} \wedge s[i + c[i]] \neq s[i + 6 - c[i]]$$

V případě $r = 1$ (palindrom existuje) je certifikátem prostě počáteční pozice:

$$c \in \{0, \dots, \mathrm{len}(s)-7\} \wedge \mathrm{palindrome}(\mathrm{substr}(s, c, \langle s[c], \dots, s[c+6] \rangle))$$

---

## Cvičení 3 — Důkazy v logických teoriích

### (a) V teorii seznamů: $\forall l, x, y . l = \text{cons}(x, \text{cons}(y, \text{empty}())) \Rightarrow \text{first}(\text{rest}(l)) = y$

**Důkaz.** Nechť $l, x, y$ jsou libovolná, ale pevná. Předpokládáme $l = \text{cons}(x, \text{cons}(y, \text{empty}()))$ a dokazujeme $\text{first}(\text{rest}(l)) = y$.

Pomocí předpokladu nahradíme $l$: potřebujeme dokázat $\text{first}(\text{rest}(\text{cons}(x, \text{cons}(y, \text{empty}())))) = y$.

Z axiomu seznamů $\forall l, x . \text{rest}(\text{cons}(x, l)) = l$, volbou $l \leftarrow \text{cons}(y, \text{empty}()), x \leftarrow x$ víme:

$$\text{rest}(\text{cons}(x, \text{cons}(y, \text{empty}()))) = \text{cons}(y, \text{empty}())$$

Stačí tedy dokázat $\text{first}(\text{cons}(y, \text{empty}())) = y$.

Z axiomu seznamů $\forall l, x . \text{first}(\text{cons}(x, l)) = x$, volbou $l \leftarrow \text{empty}(), x \leftarrow y$ víme $\text{first}(\text{cons}(y, \text{empty}())) = y$, což je to, co jsme chtěli dokázat. ■

---

### (b) V teorii seznamů: $\exists l . \text{rest}(l) = \text{empty}()$

**Důkaz.** Pro důkaz existenčního kvantifikátoru musíme najít term $t$ a dokázat $\text{rest}(t) = \text{empty}()$.

Zvolíme $t = \text{cons}(0, \text{empty}())$ (nebo libovolný prvek místo $0$).

Z axiomu seznamů $\forall l, x . \text{rest}(\text{cons}(x, l)) = l$, volbou $l \leftarrow \text{empty}(), x \leftarrow 0$ víme $\text{rest}(\text{cons}(0, \text{empty}())) = \text{empty}()$. Hotovo. ■

---

### (c) V teorii polí: $\forall a, i, j, x . \text{write}(a, i, x)[j] = x \Rightarrow [i = j \vee a[j] = x]$

**Důkaz.** Nechť $a, i, j, x$ jsou libovolná, ale pevná. Předpokládáme $\text{write}(a, i, x)[j] = x$ a dokazujeme $i = j \vee a[j] = x$.

Pro důkaz disjunkce předpokládáme $\neg[i = j]$ (tj. $i \neq j$) a dokazujeme $a[j] = x$.

Z axiomu polí $\forall a, v, i, j . \neg[i = j] \Rightarrow \text{write}(a, i, v)[j] = a[j]$ zvolíme $a \leftarrow a$, $v \leftarrow x$, $i \leftarrow i$, $j \leftarrow j$ a dostaneme:

$$i \neq j  \Rightarrow \text{write}(a, i, x)[j] = a[j]$$

Protože předpokládáme $i \neq j$, vyvodíme $\text{write}(a, i, x)[j] = a[j]$.

Nyní známe jak $\text{write}(a, i, x)[j] = x$ (náš původní předpoklad), tak $\text{write}(a, i, x)[j] = a[j]$. Z vlastností rovnosti (symetrie a tranzitivita) vyvodíme $a[j] = x$, což je to, co jsme chtěli dokázat. ■

---

### (d) Dokažte $\forall k . 0 + k = k$ z Peanových axiomů **bez** indukčního axiomu, s použitím indukce tak, jak jste se ji učili ve škole

**Důkaz.** Dokazujeme tvrzení matematickou indukcí podle $k$.

**Bázový případ $k = 0$:** Potřebujeme dokázat $0 + 0 = 0$. Z Peanova axiomu $\forall x . x + 0 = x$, volbou $x = 0$, dostáváme $0 + 0 = 0$. ✓

**Indukční krok:** Předpokládáme indukční hypotézu $0 + k = k$ (pro libovolné $k$). Dokazujeme $0 + (k+1) = k + 1$.

Z Peanova axiomu $\forall x, y . x + (y+1) = (x+y) + 1$, volbou $x = 0$, $y = k$:
$$0 + (k+1) = (0 + k) + 1$$

Podle indukční hypotézy $0 + k = k$ dosadíme a dostaneme:
$$(0 + k) + 1 = k + 1$$

Tedy $0 + (k+1) = k + 1$. ✓

Podle principu matematické indukce platí $\forall k . 0 + k = k$. ■

---

### (e) Dokažte $\forall k . 0 + k = k$ z Peanových axiomů (včetně indukčního axiomu)

**Důkaz.** Použijeme Peanův indukční axiom. Pro formuli $F(k)$ s jedinou volnou proměnnou $k$ axiom říká:

$$[F(0) \wedge [\forall k . F(k) \Rightarrow F(k+1)]] \Rightarrow \forall k . F(k)$$

Zvolíme $F(k)$ jako $0 + k = k$. Dokážeme konjunkci na levé straně.

**První konjunkt — $F(0)$:** Dokázat $0 + 0 = 0$. Z Peanova axiomu $\forall x . x + 0 = x$, volbou $x = 0$, dostáváme $0 + 0 = 0$. ✓

**Druhý konjunkt — $\forall k . F(k) \Rightarrow F(k+1)$:** Nechť $k$ je libovolné. Předpokládáme $F(k)$, tj. $0 + k = k$, a dokazujeme $F(k+1)$, tj. $0 + (k+1) = k + 1$.

Z Peanova axiomu $\forall x, y . x + (y+1) = (x+y) + 1$, volbou $x = 0$, $y = k$:
$$0 + (k+1) = (0 + k) + 1$$

Z indukční hypotézy $0 + k = k$ dosadíme:
$$(0 + k) + 1 = k + 1$$

Tedy $0 + (k+1) = k + 1$. ✓

Oba konjunkty jsou dokázány. Podle indukčního axiomu vyvozujeme $\forall k . 0 + k = k$. ■
