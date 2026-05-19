# AGENTS.md — Notation & Formatting Conventions

This project contains markdown files with LaTeX math rendered via a VS Code extension (KaTeX or MathJax-based). Follow these rules so all mathematical notation renders correctly.

---

## Math Delimiters

| Type | Delimiter | Example |
|------|-----------|---------|
| Inline math | `$...$` | `$x \geq 0$` |
| Display/block math | `$$...$$` | `$$\forall x . P(x)$$` |

---

## Pseudocode Containing Math

**Do NOT use markdown code blocks** (````` ``` `````) for pseudocode that contains math symbols, sums, subscripts, etc. Code blocks render literally — `$...$` inside them stays as raw text.

**Use `$$ \begin{aligned} ... \end{aligned} $$` instead:**

```latex
$$
\begin{aligned}
& x_0 \leftarrow x \\
& i \leftarrow 0 \\
& \text{while } i < n \text{ do} \\
& \quad @\ x = x_0 + \sum_{k=0}^{i-1} a[k] \\
& \quad x \leftarrow x + a[i] \\
& \quad i \leftarrow i + 1 \\
& @\ x = x_0 + \sum_{k=0}^{n-1} a[k]
\end{aligned}
$$
```

Key points:
- **`&`** — alignment anchor (at start of each line)
- **`\\`** — line break
- **`\quad`** — indentation
- **`\text{...}`** — plain text inside math mode (for keywords like `while`, `if`, `else`, `assume`, `for`, `to`, `do`, `then`, `return`)
- **NEVER use `\texttt{...}`** — it fails in KaTeX-based renderers

Plain code blocks (````` ``` `````) are fine only when content contains no math symbols.

---

## Spacing in Math Mode

- **NEVER use `\ `** (backslash followed by literal space) — it is a thin-space command that KaTeX often rejects. Use a plain space character instead.
- **Do not use `\:` or `\;`** for spacing — just use regular spaces between math symbols. The renderer handles spacing.
- **`\\`** (double backslash) is a table line-break — distinct from the forbidden standalone `\ `. Do not remove `\\` in table contexts.

---

## Function Names in Math

**Always wrap function/operator names in `\mathrm{...}`.** Bare names like `len(s)` render as italic product `l · e · n(s)`.

| Wrong | Right |
|-------|-------|
| `$len(s)$` | `$\mathrm{len}(s)$` |
| `$substr(s, p, s')$` | `$\mathrm{substr}(s, p, s')$` |
| `$perm(a, b, n)$` | `$\mathrm{perm}(a, b, n)$` |
| `$prime(p)$` | `$\mathrm{prime}(p)$` |
| `$sgn(n)$` | `$\mathrm{sgn}(n)$` |
| `$product(a, n)$` | `$\mathrm{product}(a, n)$` |

**Compound identifiers with underscore:** Wrap entirely in `\mathrm{...}` so the `\_` is in text mode:

| Wrong | Right |
|-------|-------|
| `$substr\_len(s)$` | `$\mathrm{substr\_len}(s)$` |

**Use `\mathrm`, not `\text`, for function names** — `\text` inherits surrounding font style; `\mathrm` always uses upright roman. Reserve `\text` for natural language words (e.g., `\text{ is bijective }`).

---

## Common LaTeX Commands

| Command | Renders as | Usage |
|---------|------------|-------|
| `\forall` | ∀ | universal quantifier |
| `\exists` | ∃ | existential quantifier |
| `\neg` | ¬ | negation |
| `\wedge` | ∧ | conjunction |
| `\vee` | ∨ | disjunction |
| `\Rightarrow` | ⇒ | implication |
| `\Leftrightarrow` | ⇔ | equivalence |
| `\sum` | ∑ | summation |
| `\mathbb{N}` | ℕ | natural numbers |
| `\mathbb{Z}` | ℤ | integers |
| `\mathcal{A}` | 𝒜 | array type |
| `\mathcal{S}` | 𝒮 | string type |
| `\mathcal{L}` | ℒ | list type |
| `\top` | ⊤ | tautology / true |
| `\perp` | ⊥ | contradiction / false |
| `\models` | ⊨ | logical consequence |
| `\text{...}` | text | plain text in math |
| `\mathrm{...}` | text | roman font (e.g., `\mathrm{sgn}`) |

### Summation Notation

**Keep `\sum` subscripts simple.** Do not nest `\in`, `\{`, or other complex constructs inside `\sum` limits — KaTeX may reject them.

| Wrong | Right |
|-------|-------|
| `\sum_{i \in \{0, \dots, n-1\}} a[i]` | `\sum_{i=0}^{n-1} a[i]` |
| `\sum_{k \in S} f(k)` | `\sum_{k=0}^{|S|-1} f(k)` or describe the set separately |

---

## Avoid These

| Don't use | Reason | Use instead |
|-----------|--------|-------------|
| `\texttt{...}` in math | KaTeX rejects it | `\text{...}` |
| `\textcircled{...}` | Not supported in most renderers | `\text{...}` or `\boxed{...}` |
| `` ``` `` for math pseudocode | Math not rendered inside code blocks | `$$ \begin{aligned} $$` |
| Unicode subscripts in code blocks | e.g., `x₀` — inconsistent | `x_0` inside `$$` |
| `\ ` (backslash-space) | KaTeX rejects thin-space command | Plain space |
| `\:` or `\;` for spacing | Unnecessary, may fail | Plain spaces |
| Bare function names in math | Renders as italic product | `\mathrm{len}(s)` |
| `\_` outside `\mathrm{...}` in math | Causes subscript/parse error | `\mathrm{substr\_len}` |

---

## File Structure

Lecture sources live in `mds/lectures/` (read-only, original PDF-to-MD conversion). Assignment solutions live in `assignments_solved/` and must follow the conventions above.
