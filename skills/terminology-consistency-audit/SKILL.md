---
name: "terminology-consistency-audit"
description: "Use before submission, after a translation, and after any renaming. Counts every variant of each technical term across a manuscript, finds spelling and hyphenation splits, and flags a heading or label whose wording contradicts the section it names. Mechanical: counts and locates, then proposes one form."
---

# Terminology Consistency Audit

## Core Rule
> One concept, one name, throughout. Count the variants before deciding which
> is right, because the minority form is often the one in the title.

---

## 1. Procedure

1. List the concepts that carry a technical name in this document.
2. For each, list every plausible variant: synonyms, hyphenation, British and
   American spellings, singular and plural, capitalised and not.
3. Count each variant across the whole source, including captions, table
   headers, section titles and the abstract.
4. Report the counts side by side.
5. Locate every occurrence of the minority forms in context.
6. Choose one form. Where the title or an established term in the field uses a
   particular form, that one wins regardless of count.
7. Apply in one pass, then recount to confirm zero remaining.

---

## 2. What the counts look like

Real output from one manuscript:

| concept | variants | counts |
| --- | --- | --- |
| the learning attacker | `smart jammer` / `intelligent jammer` | 4 / 1 |
| the full-knowledge benchmark | `oracle solver` / `oracle decoder` | 10 / 1 |
| the attribute | `state-aware` / `state aware` | 16 / 1 |
| spelling | `behavior` / `behaviour` | 9 / 1 |
| spelling | `optimize` / `optimise` | 9 / 3 |
| spelling | `while` / `whilst` | 22 / 1 |
| compound | `channel hopping` / `channel-hopping` | 3 / 2 |
| compound | `duty cycle` / `duty-cycle` | 1 / 2 |

Two readings worth separating:

**A lopsided split is a slip.** One `oracle decoder` against ten `oracle solver`
is a typo for a different word, and the minority form is simply wrong: a solver
is not a decoder.

**A split where the minority is in the title is a decision, not a slip.** Four
`smart jammer` against one `intelligent jammer` looks settled until the title
reads *Intelligent Jamming*. The majority is what needs changing.

---

## 3. Headings and labels

A heading can contradict the section beneath it, and nothing in a spell check
sees it.

The case from one manuscript: a subsection headed `State-aware Baseline` whose
content described the baseline's *lack* of state awareness, in a paper whose
central claim is that distinction, and whose later section was correctly headed
`Position of the State-Unaware Baseline`. The heading came from a translation
that inverted the original.

Check every heading, caption and table header against what it names:

- Does the heading assert the opposite of the content?
- Do two headings naming the same thing disagree?
- Does a caption name a quantity the figure does not show?
- Does a table header use a different term from the column it describes?
- Has a qualifier been dropped in translation? A dropped `tabular` or `stateless`
  changes what the heading claims.

---

## 4. Translation artefacts

After a translation, also count:

- Characters from the source language's keyboard inside the target text. A
  dotted capital `İ` in `İnstead` survives spell-check and renders wrongly.
- Conventions that belong to the source language: percent sign placement, decimal
  separator, quotation marks, date order.
- Literal renderings that are not idiomatic: a header reading `Behaviour of
  model` where the source said *behaviour model*.
- Missing articles at sentence openings, where the source language has none.
- A parenthetical gloss that made sense in the source and is redundant in the
  target, such as a section titled `Network and Topology Model (Network Model)`.

---

## 5. Reporting

Per finding: concept, variants with counts, the chosen form, and every file and
line to change. Where the choice is not obvious, say which evidence decided it —
the title, the field's established usage, or the majority.

Apply in one pass. A half-applied rename leaves the document in a worse state
than the split it was meant to fix, and the next audit cannot tell which form
was intended.
