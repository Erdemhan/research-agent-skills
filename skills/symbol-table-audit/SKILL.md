---
name: "symbol-table-audit"
description: "Use when a manuscript, thesis or formulation record carries mathematical notation. Finds one symbol carrying two meanings, one quantity written two ways, symbols used before definition, and subscripts whose letters collide with another quantity. Run before submission and after any notation change."
---

# Symbol Table Audit

## Core Rule
> One symbol, one meaning, across the whole document. One quantity, one symbol,
> across the whole document. Everything else is a reader trap.

---

## 1. Build the table first

Extract every symbol from the equations and the prose, with where it is first
defined and what it denotes.

| symbol | meaning | unit | first defined | occurrences |
| --- | --- | --- | --- | --- |
| `eta` | path loss exponent, air | — | §3.2 | 14 |
| `eta_s` | path loss exponent, ground | — | §3.2.2 | 9 |
| `L_sys` | mean excess loss over free space | linear | §3.2 | 6 |
| `sigma_air` | shadowing std. dev., air | dB | §3.2.1 | 5 |
| `sigma_SF` | shadowing std. dev., ground | dB | §3.2.2 | 7 |
| `SF` | LoRa spreading factor | — | Table 2 | 4 |

The table itself surfaces most problems. Two rows that mean the same thing, or
one row whose meaning changes between occurrences, are visible once the symbols
are beside each other.

---

## 2. The four failures

**Collision: one symbol, two meanings.**
`sigma_SF` uses `SF` for *shadow fading*. The same two letters denote the *LoRa
spreading factor* in the same table, with the value 7. Two unrelated quantities,
one abbreviation. A reader meeting `sigma_SF` after reading `SF = 7` has to be
told they are unrelated, and the text usually does not tell them.

**Drift: one quantity, two symbols.**
A coefficient written as `eta` in an early draft and `L_sys` later. Worse when
both symbols remain live elsewhere: here `eta` is the path loss exponent, and
both quantities happen to equal 2.0 numerically, so a swap produces no visible
absurdity. The code guards against this by keeping `L_SYS` and `ETA` as separate
constants, and the formulation record carries an explicit warning. The document
needs the same discipline.

**Use before definition.**
A symbol appearing in §3.1 and defined in §3.3. Common when sections are
reordered during revision.

**Silent redefinition.**
A symbol defined once, then reused for something else in a later section,
usually an index or a counter.

---

## 3. Subscript discipline

Subscripts carry meaning and collide more easily than the base symbols.

- Is the subscript an abbreviation of a word (`sys`, `air`, `SF` for shadow
  fading) or an index (`i`, `j`, `c`, `k`)? Do not mix the two conventions on
  one base symbol.
- Where an abbreviation subscript collides with a standalone symbol in the same
  document, rename one. Prefer renaming the subscript, because the standalone
  symbol is usually the field's standard.
- Keep parallel quantities parallel. If one class's spread is `sigma_air`, the
  other's should be `sigma_ground`, not `sigma_SF`. Asymmetric naming of
  symmetric quantities is itself a finding.

---

## 4. Procedure

1. Extract every symbol from the math environments and from inline math in the
   prose.
2. Build the table. Record meaning, unit, first definition, occurrence count.
3. Sort by symbol and look for one symbol with two meanings.
4. Sort by meaning and look for one meaning with two symbols.
5. Compare each first-definition position against the first occurrence position.
6. List every subscript and check it against the standalone symbols.
7. Check that parallel quantities are named in parallel.
8. Cross-check the table against the code's constant names where the code is
   the implementation of this notation.

---

## 5. Reporting

For each finding, give the two sites and a proposed resolution. Where the
resolution is a rename, say which occurrences change and confirm the new symbol
is free across the whole document, including figures, tables and the code.

A notation change is a document-wide edit. Do it in one pass or not at all: a
half-applied rename is worse than the collision it was meant to fix.
