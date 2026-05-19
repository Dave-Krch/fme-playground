# Zadání 5 — Formální metody a specifikace

---

## Cvičení 1 — Specifikace procedury

### (a) Formule predikátové logiky pro specifikaci

Procedura `p(a, r, x)` modifikuje první argument `a`. Výstupní proměnnou pro `a` značíme $a^*$:

$$\forall a, r, x, a^* \in \mathcal{N}. [r \geq 0 \wedge p(a, a^*, r, x)] \Rightarrow a^* = a + r \cdot x$$

Zde $p(a, a^*, r, x)$ je predikát vyjadřující, že volání procedury s argumenty $(a, r, x)$ dává novou hodnotu $a^*$ pro první parametr.

### (b) Program v SSA

Původní program:
```
assume x ≥ 8 ∧ k ≥ 5
p(x, 2, k)
@ x ≥ 15
```

Volání procedury `p(x, 2, k)` může změnit $x$. V SSA zavedeme novou proměnnou $x_1$ pro novou hodnotu $x$:

```
assume x ≥ 8 ∧ k ≥ 5
assume p(x, x₁, 2, k)
@ x₁ ≥ 15
```

Zde $p(x, x_1, 2, k)$ je predikát — volání procedury se stává příkazem **assume**.

### (c) Verifikační podmínka

$$[x \geq 8 \wedge k \geq 5 \wedge p(x, x_1, 2, k)] \Rightarrow x_1 \geq 15$$

### (d) Platí VP?

**Ano.** Ze specifikace procedury (a), dosazením $a \leftarrow x$, $r \leftarrow 2$, $x \leftarrow k$ a $a^* \leftarrow x_1$, s $r = 2 \geq 0$:

$$p(x, x_1, 2, k) \Rightarrow x_1 = x + 2 \cdot k$$

Z předpokladů $x \geq 8$ a $k \geq 5$:
$$x_1 = x + 2k \geq 8 + 2 \cdot 5 = 8 + 10 = 18 \geq 15$$

VP tedy platí.

### (e) Důkaz

1. Z formule pro proceduru (a), instanciací $a \leftarrow x$, $r \leftarrow 2$, $x \leftarrow k$, $a^* \leftarrow x_1$. Vstupní podmínka $r \geq 0$ je $2 \geq 0$, což platí. Z $p(x, x_1, 2, k)$ vyvodíme $x_1 = x + 2 \cdot k$.

2. Z předpokladu $x \geq 8$ máme $x \geq 8$.

3. Z předpokladu $k \geq 5$ máme $k \geq 5$, tedy $2k \geq 10$.

4. Sečtením: $x_1 = x + 2k \geq 8 + 10 = 18$.

5. Protože $18 \geq 15$, máme $x_1 \geq 15$. ■

---

## Cvičení 2 — Ukončení cyklu

### Tabulka průchodu

Vstup: $i = 8$, $k = 0$

$\begin{array}{c|c|c|c}
i & k & k \bmod 10 & \text{Provedená akce} \\ \hline
8 & 0 & 0 & \text{A: } i \leftarrow i-1,\; k \leftarrow 6 \\
7 & 6 & 6 & \text{B: } k \leftarrow k+1 \\
7 & 7 & 7 & \text{B: } k \leftarrow k+1 \\
7 & 8 & 8 & \text{B: } k \leftarrow k+1 \\
7 & 9 & 9 & \text{B: } k \leftarrow k+1 \\
7 & 10 & 0 & \text{A: } i \leftarrow i-1,\; k \leftarrow 6 \\
6 & 6 & 6 & \text{B: } k \leftarrow k+1 \\
6 & 7 & 7 & \text{B: } k \leftarrow k+1 \\
6 & 8 & 8 & \text{B: } k \leftarrow k+1 \\
6 & 9 & 9 & \text{B: } k \leftarrow k+1 \\
6 & 10 & 0 & \text{A: } i \leftarrow i-1,\; k \leftarrow 6 \\
\vdots & \vdots & \vdots & \vdots \\
1 & 10 & 0 & \text{A: } i \leftarrow 0,\; k \leftarrow 6 \\
0 & 6 & — & \text{cyklus končí } (i = 0)
\end{array}$

### Variant cyklu

**Variant:** $v(i, k) = 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10)$

**Uspořádání s klesajícími prvky:** $<$ na $\mathbb{N}$ (standardní uspořádání na nezáporných celých číslech).

**Vysvětlení:** Variant klesá v každé iteraci:

- **Větev A** ($k \bmod 10 = 0$): $i$ klesne o $1$, $k$ se změní na $6$.
 $v_{původní} = 10i + 0 = 10i$.
 $v_{nové} = 10(i-1) + (10 - 6) = 10i - 10 + 4 = 10i - 6$. Pokles o $6$. ✓

- **Větev B** ($k \bmod 10 \in \{1,\dots,8\}$): $i$ beze změny, $k$ vzroste o $1$.
 $(k+1) \bmod 10 = k \bmod 10 + 1$.
 $v_{původní} = 10i + (10 - n)$, kde $n = k \bmod 10$.
 $v_{nové} = 10i + (10 - (n+1)) = 10i + 9 - n$.
 $v_{nové} = v_{původní} - 1$. Pokles o $1$. ✓

- **Větev B** ($k \bmod 10 = 9$): $i$ beze změny, $k$ vzroste o $1$, přetočí se na $0$.
 $(k+1) \bmod 10 = 0$.
 $v_{původní} = 10i + (10 - 9) = 10i + 1$.
 $v_{nové} = 10i + ((10 - 0) \bmod 10) = 10i + 0 = 10i$.
 $v_{nové} = v_{původní} - 1$. Pokles o $1$. ✓

Variant je vždy nezáporné celé číslo (protože $i \geq 0$, $k \geq 0$) a klesá v každé iteraci. Protože $<$ na $\mathbb{N}$ je relace s klesajícími prvky, cyklus musí skončit.

### Aserce kontrolující variant

Přidáme pomocnou proměnnou $v$ a aserce do programu:

$$
\begin{aligned}
& \text{assume } i \geq 7 \\
& \text{assume } k \geq 0 \\
& v \leftarrow \infty \\
& \text{while } i \neq 0 \text{ do} \\
& \quad @ v > 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10) \wedge 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10) \geq 0 \\
& \quad v \leftarrow 10 \cdot i + ((10 - (k \bmod 10)) \bmod 10) \\
& \quad \text{if } k \bmod 10 = 0 \text{ then} \\
& \quad \quad i \leftarrow i - 1;\; k \leftarrow 6 \\
& \quad \text{else} \\
& \quad \quad k \leftarrow k + 1
\end{aligned}
$$

Zde $\infty$ je konstanta větší než jakákoli možná hodnota variantu (např. libovolné číslo větší než $10 \cdot 7 + 10 = 80$).

---

## Cvičení 3 — Bounded Model Checking

### (a) Rozvinutí while cyklu dvakrát

Původní program:
```
i ← k
while i ≤ n do
 i ← 2i
@ i ≥ k
```

Po dvojím rozvinutí (příkaz `assume` omezuje provádění na nejvýše 2 iterace):

```
i ← k
if i ≤ n then
 i ← 2i
 if i ≤ n then
 i ← 2i
 assume ¬(i ≤ n)
@ i ≥ k
```

Toto pokrývá $0$, $1$ nebo $2$ iterace. Pro více než $2$ iterace příkaz assume selže (kontrola je pro hlubší provádění neprůkazná).

### (b) SSA s if-then-else (bez rozkladu na základní cesty)

```
i₁ ← k
γ₁ ← [i₁ ≤ n]
i₂ ← 2·i₁
i₃ ← γ₁ ? i₂ : i₁
γ₂ ← [i₃ ≤ n]
i₄ ← 2·i₃
i₅ ← γ₂ ? i₄ : i₃
assume ¬(i₅ ≤ n)
@ i₅ ≥ k
```

Vysvětlení: $\gamma_1, \gamma_2 \in \{\top, \perp\}$ jsou booleovské proměnné zachycující podmínku cyklu. Ternární operátor `? :` vybírá mezi aktualizovanou a nezměněnou hodnotou. Po posledním `if` příkaz `assume` vynucuje, že žádná další iterace již není možná.

### (c) Logická formule

Formule kóduje omezení SSA a závěrečnou aserci:

$$[i_1 = k \wedge [\gamma_1 \Leftrightarrow (i_1 \leq n)] \wedge i_2 = 2i_1 \wedge i_3 = (\gamma_1 ? i_2 : i_1)$$

$$\wedge [\gamma_2 \Leftrightarrow (i_3 \leq n)] \wedge i_4 = 2i_3 \wedge i_5 = (\gamma_2 ? i_4 : i_3)$$

$$\wedge \neg(i_5 \leq n)] \Rightarrow i_5 \geq k$$

Kde $s = \phi ? t_1 : t_2$ je zkratka za $[\phi \Rightarrow s = t_1] \wedge [\neg\phi \Rightarrow s = t_2]$.

---

## Cvičení 4 — Hledání podřetězce

### (a) Invarianty cyklu

Program hledá podřetězec $s$ v řetězci $a$. Definujme pomocné predikáty:

- $R(i) \equiv \exists r \in \{0, \dots, i-1\}. \mathrm{substr}(a, r, s)$ (podřetězec nalezen před pozicí $i$)

**Invariant vnějšího cyklu** (na pozici `@` po `for i ← …`):

$$found \Leftrightarrow R(i)$$

To říká: na začátku vnější iterace $i$ platí $found$ právě tehdy, když byl nalezen výskyt na nějaké pozici $< i$.

**Invariant vnitřního cyklu** (na prvním `@` uvnitř vnitřního `for`):

$$p \Leftrightarrow (\forall m \in \{0, \dots, j-1\}. a[i+m] = s[m])$$

To říká: na pozici $i$ se znaky $0$ až $j-1$ dosud shodují. $p$ je pravdivé právě tehdy, když prefix $s$ délky $j$ odpovídá.

**Po vnitřním `if`** (pokud $a[i+j] \neq s[j]$, pak $p \leftarrow \perp$):

$$p \Leftrightarrow (\forall m \in \{0, \dots, j\}. a[i+m] = s[m])$$

Po zpracování pozice $j$ odráží $p$ to, zda všechny kontrolované pozice odpovídají.

**Po vnitřním cyklu** (po `if p then found ← ⊤`):

$$found \Leftrightarrow R(i+1)$$

Protože $p$ na konci vnitřního cyklu je pravdivé právě tehdy, když všechny pozice $0, \dots, \mathrm{len}(s)-1$ odpovídají (což znamená $\mathrm{substr}(a, i, s)$), aktualizujeme $found$ odpovídajícím způsobem.

### (b) Počet verifikačních podmínek

Program má:
- 1 vnější `for` cyklus s 1 invariantem
- 1 vnitřní `for` cyklus s 1 invariantem
- 1 aserci po vnitřním `if`
- 1 aserci po těle vnitřního cyklu
- 1 závěrečnou aserci

Počet základních cest:
- Vstupní cesta k vnějšímu invariantu: 1
- Přeskočení vnějšího cyklu: 1
- Vnější invariant → vnitřní invariant (včetně vstupu do vnitřního cyklu): 1
- Vnitřní invariant → po vnitřním `if` (2 větve): 2
- Po vnitřním `if` → zpět k vnitřnímu invariantu (opětovný vstup): 1
- Po vnitřním `if` → opuštění vnitřního cyklu: 1
- Po vnitřním cyklu → vnější invariant (opětovný vstup): 1
- Opuštění vnějšího for → závěrečná: 1

Celkem přibližně **12 verifikačních podmínek**.

(Přesný počet se může lišit podle toho, jak přesně linearizujeme vnořené for-cykly.)

### (c) Verifikační podmínky (neformální kontrola)

S použitím invariantů z (a) VP vycházejí následovně:

1. **Vstup do vnějšího for** ($found = \perp$, $i = 0$): $found \Leftrightarrow R(0)$. $R(0)$ znamená, že podřetězec existuje před pozicí 0 — nemožné. Obě nepravda. ✓

2. **Vnější inv → vstup do vnitřního for** ($i \leq \mathrm{len}(a)-\mathrm{len}(s)$): $p \leftarrow \top$, $j \leftarrow 0$. Vnitřní inv: $p \Leftrightarrow (\forall m \in \{0,\dots,-1\}. \dots)$. Prázdný rozsah, vakuózní pravda. $p = \top$, obě pravda. ✓

3. **Vnitřní inv, $a[i+j] = s[j]$** → po vnitřním `if` ($p$ beze změny $\top$): po aserci $p \Leftrightarrow (\forall m \in \{0,\dots,j\}. a[i+m] = s[m])$. Podle vnitřního inv prvních $j$ sedí, a $a[i+j] = s[j]$, takže všech $j+1$ sedí. $p = \top$, obě pravda. ✓

4. **Vnitřní inv, $a[i+j] \neq s[j]$** → po vnitřním `if` ($p \leftarrow \perp$): po aserci $p \Leftrightarrow (\forall m \in \{0,\dots,j\}. a[i+m] = s[m])$. Protože $a[i+j] \neq s[j]$, pravá strana je nepravda, a $p = \perp$, obě nepravda. ✓

5. **Po vnitřním `if` → opětovný vstup do vnitřního for** ($j \leftarrow j+1$, $j \leq \mathrm{len}(s)-1$): vnitřní inv platí pro nové $j$, protože aserce po vnitřním `if` potvrzuje vlastnost pro $j$ pozic a $j_{nové} = j_{původní}+1$. Invariant požaduje shodu pro $0, \dots, j_{nové}-1 = j_{původní}$, což bylo právě potvrzeno. ✓

6. **Po vnitřním `if` → opuštění vnitřního for** ($j \leftarrow j+1$, $j = \mathrm{len}(s)$): $p$ je nyní pravdivé právě tehdy, když všech $\mathrm{len}(s)$ pozic sedí na offsetu $i$, tj. $\mathrm{substr}(a, i, s)$. ✓

7. **Po vnitřním for, aktualizace $found$** → po aserci. Pokud $p = \top$ (nalezen výskyt na $i$), $found \leftarrow \top$. Pak $found \Leftrightarrow R(i+1)$ (protože výskyt na $i$ je zahrnut). Pokud $p = \perp$, $found$ beze změny a $R(i+1) = R(i)$ (žádný výskyt na $i$). Oba případy zachovávají $found \Leftrightarrow R(i+1)$. ✓

8. **Opětovný vstup do vnějšího for** ($i \leftarrow i+1$, $i \leq \mathrm{len}(a)-\mathrm{len}(s)$): vnější invariant požaduje $found \Leftrightarrow R(i_{nové}) = R(i_{původní}+1)$. Aserce po vnitřním cyklu dává $found \Leftrightarrow R(i_{původní}+1)$. ✓

9. **Opuštění vnějšího for** ($i \leftarrow i+1$, $i > \mathrm{len}(a)-\mathrm{len}(s)$): $i_{nové} = i+1 > \mathrm{len}(a)-\mathrm{len}(s)$, takže poslední kontrolovaná pozice byla $i_{původní} = \mathrm{len}(a)-\mathrm{len}(s)$. $R(\mathrm{len}(a)-\mathrm{len}(s)+1)$ pokrývá všechny možné počáteční pozice, takže závěrečná aserce $found \Leftrightarrow \exists r. \mathrm{substr}(a, r, s)$ platí. ✓
