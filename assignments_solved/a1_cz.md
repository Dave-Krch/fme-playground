# Zadání 1 — Formální metody a specifikace

---

## Cvičení 1 — Příběh o nepřesné specifikaci

Před několika lety kolega pracoval na interním nástroji pro logistickou firmu. Zadání znělo:

> *"Když je stav zásilky 'na cestě', pošli zákazníkovi oznámení každou hodinu."*

Implementoval to doslova: cron úloha, která běžela každých 60 minut a odesílala push notifikaci. Systém byl nasazen.

Ve 3 hodiny ráno obdržel provozní tým rozzlobené hovory. Zákazníky budila každinoní oznámení o zásilkách, které měly dorazit až za několik dní. A co bylo horší — zásilky, které byly "na cestě" déle než dva týdny (zaseknuté na celnici), měly u každého zákazníka přes 300 nepřečtených oznámení.

Specifikace neříkala nic o:
- časovém omezení (noc vs. den),
- maximálním počtu zpráv,
- ani o tom, co se má stát, když stav "na cestě" trvá několik týdnů.

Oprava vyžadovala okamžité nasazení, ale škoda již byla napáchána: tisíce uživatelů aplikaci odinstalovalo. Všechno kvůli **jedné chybějící větě** ve specifikaci.

---

## Cvičení 2 — Vstupně/výstupní specifikace

### Pomocné definice

Používáme následující typy:
- $\mathbb{N}$ — přirozená čísla (včetně nuly)
- $\mathbb{Z}$ — celá čísla
- $\mathbb{N}^+$ — kladná přirozená čísla $\{1, 2, 3, \dots\}$
- $\mathcal{A}$ — pole s celočíselnými prvky
- $\mathcal{S}$ — řetězce znaků

Zapisujeme $a[i]$ pro čtení $i$-tého prvku pole $a$ (indexováno od 0) a $\mathrm{len}(a)$ pro délku pole nebo řetězce $a$.

---

**Definice prvočísla:**

$$\forall p \in \mathbb{N}. \mathrm{prime}(p) :\Leftrightarrow p > 1 \wedge \forall d \in \mathbb{N}. [\exists k \in \mathbb{N}. d \cdot k = p] \Rightarrow [d = 1 \vee d = p]$$

**Definice součinu prvků pole:**

Pro pole $a$ délky $n$ definujeme součin induktivně:

- $\forall a \in \mathcal{A}. \mathrm{product}(a, 0) := 1$
- $\forall a \in \mathcal{A}, n \in \mathbb{N}^+. \mathrm{product}(a, n) := \mathrm{product}(a, n-1) \cdot a[n-1]$

(Budeme také psát $\mathrm{product}(a)$ jako zkratku pro $\mathrm{product}(a, \mathrm{len}(a))$.)

**Definice permutace pole:**

$$\forall a, b \in \mathcal{A}, n \in \mathbb{N}. \mathrm{perm}(a, b, n) :\Leftrightarrow$$

$$\exists p: \{0, \dots, n-1\} \to \{0, \dots, n-1\}. p \text{ je bijekce } \wedge \forall i \in \{0, \dots, n-1\}. b[i] = a[p(i)]$$

**Definice podřetězce:**

$$\forall s, s' \in \mathcal{S}, p \in \mathbb{N}. \mathrm{substr}(s, p, s') :\Leftrightarrow$$

$$0 \leq p \wedge p + \mathrm{len}(s') \leq \mathrm{len}(s) \wedge \forall i \in \{0, \dots, \mathrm{len}(s')-1\}. s'[i] = s[p+i]$$

**Definice palindromu:**

$$\forall s \in \mathcal{S}. \mathrm{palindrome}(s) :\Leftrightarrow \forall i \in \{0, \dots, \mathrm{len}(s)-1\}. s[i] = s[\mathrm{len}(s)-1-i]$$

---

### (a) Prvočíselný rozklad celých čísel

**Vstup:** $x \in \mathbb{N}, x \geq 2$

**Výstup:** pole $f$ délky $m \in \mathbb{N}^+$ takové, že

$$\mathrm{product}(f, m) = x \wedge \forall i \in \{0, \dots, m-1\}. \mathrm{prime}(f[i])$$

---

### (b) Nalezení největšího čísla v poli celých čísel

**Vstup:** pole $a$ délky $n \in \mathbb{N}^+$

**Výstup:** $m \in \mathbb{Z}$ takové, že

$$[\exists i \in \{0, \dots, n-1\}. a[i] = m] \wedge [\forall i \in \{0, \dots, n-1\}. a[i] \leq m]$$

---

### (c) Přeuspořádání pole tak, aby prvočísla předcházela neprvočíslům

**Vstup:** pole $a$ délky $n \in \mathbb{N}$

**Výstup:** pole $b$ délky $n$ takové, že

$$\mathrm{perm}(a, b, n) \wedge \exists k \in \{0, \dots, n\}. [\forall i \in \{0, \dots, k-1\}. \mathrm{prime}(b[i])] \wedge [\forall i \in \{k, \dots, n-1\}. \neg \mathrm{prime}(b[i])]$$

(Hodnota $k = 0$ znamená, že v poli nejsou žádná prvočísla; $k = n$ znamená, že všechny prvky jsou prvočísla. Obě možnosti jsou platným výstupem.)

---

### (d) Kontrola, zda řetězec obsahuje palindromický podřetězec délky 7

**Vstup:** řetězec $s \in \mathcal{S}$

**Výstup:** $r \in \{\top, \perp\}$ takové, že

$$r = \top \Leftrightarrow \exists i \in \{0, \dots, \mathrm{len}(s)-7\}. \mathrm{palindrome}(\mathrm{substr}(s, i, \langle s[i], s[i+1], \dots, s[i+6] \rangle))$$

(Zde $\langle s[i], \dots, s[i+6] \rangle$ označuje podřetězec řetězce $s$ délky 7 začínající na pozici $i$. Alternativně lze definovat pomocnou funkci $\mathrm{substr\_len}(s, i, l)$ a použít ji přímo.)

---

## Cvičení 3 — Důkazy

Všechny důkazy používají metodu z 2. přednášky: udržujeme seznam známých faktů (předpokladů) a formuli, kterou dokazujeme, a aplikujeme důkazová pravidla pouze na **nejkrajnější symbol**, dokud neuspějeme nebo nenajdeme spor.

---

### (a) $[\neg[[r \vee s] \Rightarrow q] \wedge [[r \vee s] \Rightarrow q]] \Rightarrow [[p \Rightarrow q] \wedge \neg[p \Rightarrow q]]$

**Důkaz.** Předpokládáme $\neg[[r \vee s] \Rightarrow q] \wedge [[r \vee s] \Rightarrow q]$ a dokazujeme $[p \Rightarrow q] \wedge \neg[p \Rightarrow q]$.

Z předpokladu, jehož nejkrajnější symbol je $\wedge$, vyvodíme jak $\neg[[r \vee s] \Rightarrow q]$, tak $[[r \vee s] \Rightarrow q]$.

To je spor (známe jak formuli, tak její negaci), čímž je důkaz hotov. ■

---

### (b) $[\neg p \Rightarrow p] \Rightarrow p$

**Důkaz.** Předpokládáme $\neg p \Rightarrow p$ a dokazujeme $p$.

Nahradíme $p$ ekvivalentní formulí $\neg\neg p$. Dokazujeme tedy $\neg\neg p$.

Pro důkaz negace předpokládáme $\neg p$ a hledáme spor.

Z předpokladu $\neg p$ a známého faktu $\neg p \Rightarrow p$ vyvodíme $p$.

Nyní známe jak $\neg p$, tak $p$ — spor. Tím je důkaz hotov. ■

---

### (c) $[p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]] \Rightarrow \neg p$

**Důkaz.** Předpokládáme $p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]$ a dokazujeme $\neg p$.

Pro důkaz negace předpokládáme $p$ a hledáme spor.

Z $p$ a známého faktu $p \Rightarrow [[q \vee r] \wedge \neg q \wedge \neg r]$ vyvodíme $[q \vee r] \wedge \neg q \wedge \neg r$.

Z této konjunkce vyvodíme tři fakty: $q \vee r$, $\neg q$ a $\neg r$.

Známý fakt $q \vee r$ je disjunkce, proto provedeme rozbor případů:

- **Případ $q$:** známe jak $q$, tak $\neg q$ — spor.
- **Případ $r$:** známe jak $r$, tak $\neg r$ — spor.

Oba případy vedou ke sporu, čímž je důkaz hotov. ■

---

### (d) $q \Rightarrow [[p \wedge q] \vee [\neg p \wedge q]]$

**Důkaz.** Předpokládáme $q$ a dokazujeme $[p \wedge q] \vee [\neg p \wedge q]$.

Pro důkaz disjunkce předpokládáme $\neg[p \wedge q]$ a dokazujeme $\neg p \wedge q$.

Pro důkaz konjunkce $\neg p \wedge q$ dokazujeme $\neg p$ a $q$ zvlášť.
$q$ je již známý fakt.

Dokazujeme $\neg p$: předpokládáme $p$ a hledáme spor.
Z $p$ a $q$ vyvodíme $p \wedge q$. To je ve sporu s naším předpokladem $\neg[p \wedge q]$. Tím je $\neg p$ dokázáno.

Nyní z $\neg p$ a $q$ vyvodíme $\neg p \wedge q$, což je to, co jsme potřebovali dokázat. ■

---

### (e) $\neg[p \wedge q] \Rightarrow [\neg p \vee \neg q]$

**Důkaz.** Předpokládáme $\neg[p \wedge q]$ a dokazujeme $\neg p \vee \neg q$.

Pro důkaz disjunkce předpokládáme $\neg\neg p$ a dokazujeme $\neg q$.

Pomocí ekvivalence $\neg\neg p$ a $p$ nyní předpokládáme $p$ a dokazujeme $\neg q$.

Pro důkaz $\neg q$ předpokládáme $q$ a hledáme spor.

Z $p$ a $q$ vyvodíme $p \wedge q$. Ale my již známe $\neg[p \wedge q]$.

To je spor, čímž je důkaz hotov. ■

---

### (f) $[[p \wedge q] \Rightarrow r] \Rightarrow [[p \Rightarrow r] \vee [q \Rightarrow r]]$

**Důkaz.** Předpokládáme $[p \wedge q] \Rightarrow r$ a dokazujeme $[p \Rightarrow r] \vee [q \Rightarrow r]$.

Pro důkaz disjunkce předpokládáme $\neg[p \Rightarrow r]$ a dokazujeme $q \Rightarrow r$.

Pro důkaz $q \Rightarrow r$ předpokládáme $q$ a dokazujeme $r$.

Nyní známe: $[p \wedge q] \Rightarrow r$, $\neg[p \Rightarrow r]$, $q$. Potřebujeme dokázat $r$.

Dokážeme lemma: $p$.

*Důkaz lemmatu $p$:* Nahradíme $p$ formulí $\neg\neg p$. Předpokládáme $\neg p$ a hledáme spor. Dokážeme $p \Rightarrow r$ v pod-lemmatu.

- *Důkaz pod-lemmatu $p \Rightarrow r$:* Předpokládáme $p$ a dokazujeme $r$. Z $\neg p$ (předpoklad lemmatu) a $p$ máme spor, čímž je pod-důkaz hotov.

Takže $p \Rightarrow r$ platí. Ale my již známe $\neg[p \Rightarrow r]$.
To je spor, lemma $p$ je tedy dokázáno. □

Nyní z lemmatu známe $p$. Spolu s $q$ vyvodíme $p \wedge q$.
Z $p \wedge q$ a známého faktu $[p \wedge q] \Rightarrow r$ vyvodíme $r$.
Tím je důkaz hotov. ■

---

### (g) $[p \wedge q] \Rightarrow \neg[\neg p \vee \neg q]$

**Důkaz.** Předpokládáme $p \wedge q$ a dokazujeme $\neg[\neg p \vee \neg q]$.

Z $p \wedge q$ vyvodíme jak $p$, tak $q$.

Pro důkaz negace předpokládáme $\neg p \vee \neg q$ a hledáme spor.

Známý fakt $\neg p \vee \neg q$ je disjunkce, proto provedeme rozbor případů:

- **Případ $\neg p$:** známe jak $p$, tak $\neg p$ — spor.
- **Případ $\neg q$:** známe jak $q$, tak $\neg q$ — spor.

Oba případy vedou ke sporu, čímž je důkaz hotov. ■

---

### (h) $\neg[p \Rightarrow q] \Rightarrow [q \Rightarrow p]$

**Důkaz.** Předpokládáme $\neg[p \Rightarrow q]$ a dokazujeme $q \Rightarrow p$.

Pro důkaz implikace předpokládáme $q$ a dokazujeme $p$.

Nyní známe $\neg[p \Rightarrow q]$ a $q$. Potřebujeme dokázat $p$.

Dokážeme lemma: $p \Rightarrow q$.

*Důkaz lemmatu $p \Rightarrow q$:* Předpokládáme $p$ a dokazujeme $q$. Protože $q$ je již známý fakt, důkaz lemmatu je hotov. □

Nyní známe $p \Rightarrow q$. Ale zároveň známe $\neg[p \Rightarrow q]$.

To je spor. Spor ukončuje aktuální důkaz, takže $p$ je dokázáno. ■

---

### (i) $[p \Rightarrow q] \vee [q \Rightarrow r]$

**Důkaz.** Dokazujeme disjunkci.

Pro důkaz disjunkce předpokládáme $\neg[p \Rightarrow q]$ a dokazujeme $q \Rightarrow r$.

Pro důkaz $q \Rightarrow r$ předpokládáme $q$ a dokazujeme $r$.

Nyní známe $\neg[p \Rightarrow q]$ a $q$. Potřebujeme dokázat $r$.

Dokážeme lemma: $p \Rightarrow q$.

*Důkaz lemmatu $p \Rightarrow q$:* Předpokládáme $p$ a dokazujeme $q$. Protože $q$ je již známý fakt, důkaz lemmatu je hotov. □

Nyní známe $p \Rightarrow q$. Ale zároveň známe $\neg[p \Rightarrow q]$.

To je spor, který ukončuje aktuální důkaz. Tím je $r$ dokázáno. ■

---
