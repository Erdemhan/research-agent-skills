---
name: "turkish-latex-consistency"
description: "Use when a Turkish LaTeX document cites figure, table, section or equation numbers. Turkish case suffixes attached to a number follow how that number is pronounced, so every renumbering silently breaks them. Reads the real numbers from the .aux and checks each suffix, plus cross-reference drift after floats move."
---

# Turkish LaTeX Consistency

## Core Rule
> A Turkish suffix attached to a cross-reference depends on the number LaTeX
> finally assigns, not on the number that was there when the sentence was
> written. Every float added, removed or reordered invalidates every suffix in
> the document.

---

## 1. Why this breaks silently

`Şekil~\ref{fig:x}'de` renders as `Şekil 16'de`, which is wrong: sixteen is
*on altı*, ending in a back vowel, so it takes `'da`. Nothing in the build
complains. The document compiles, the reference resolves, and the error survives
to the referee.

In one revision of this project four suffixes broke at once simply because two
figures were merged into one.

---

## 2. The rule

The suffix follows the **pronunciation** of the number, not its digits.

Vowel harmony: the last vowel of the spoken number decides `a` or `e`.

| last vowel | suffix vowel |
| --- | --- |
| a, ı, o, u | a |
| e, i, ö, ü | e |

Consonant hardening: a number whose spoken form ends in a voiceless consonant
takes `t` instead of `d`.

Spoken forms and what they imply:

| number | spoken | last vowel | ends voiceless | suffix |
| --- | --- | --- | --- | --- |
| 0 | sıfır | ı | no | 'da |
| 1 | bir | i | no | 'de |
| 2 | iki | i | no | 'de |
| 3 | üç | ü | yes | 'te |
| 4 | dört | ö | yes | 'te |
| 5 | beş | e | yes | 'te |
| 6 | altı | a | no | 'da |
| 7 | yedi | e | no | 'de |
| 8 | sekiz | e | no | 'de |
| 9 | dokuz | u | no | 'da |
| 10 | on | o | no | 'da |
| 20 | yirmi | i | no | 'de |
| 30 | otuz | u | no | 'da |
| 40 | kırk | ı | yes | 'ta |
| 50 | elli | i | no | 'de |

A compound number takes the suffix of its **last spoken component**: 16 is
*on altı* and follows 6, giving `'da`. 22 is *yirmi iki* and follows 2, giving
`'de`. 18 is *on sekiz* and follows 8, giving `'de`.

The same rule governs `'deki`, `'daki`, `'ten`, `'dan`, `'ye`, `'ya` and the
other case endings.

---

## 3. Checking it

Read the resolved numbers from the `.aux`, never from the source order.

```
\newlabel{fig:guc_s1}{{16}{17}}
                        ^^  figure number
```

For each `\ref{...}` immediately followed by an apostrophe and a suffix:

1. Look up the label's number in the `.aux`.
2. Derive the expected suffix from the table above.
3. Compare against what the source carries.
4. Report file, line, label, actual number, written suffix, expected suffix.

Compile first. A stale `.aux`, or one truncated by an interrupted run, gives
wrong numbers. A truncated `.aux` shows as `! File ended while scanning use of
\@newl@bel` on the next compile; delete the auxiliary files and rebuild.

---

## 4. Cross-reference drift

Suffixes are the visible symptom. Check the rest of the same class after any
float change.

- A `\ref` to a label that no longer exists. These surface as `??` in the PDF
  and as an undefined-reference warning, which is easy to miss in a long log.
- Prose that names a number literally rather than referencing it: "the two
  figures above", "as Table 8 shows" written as text.
- A sentence that points at two figures where one now exists, or at one where
  two now exist.
- Section numbers quoted in prose after a section was added.

---

## 5. Related conventions

While auditing a Turkish academic document, also check:

- The percent sign goes **before** the number in Turkish and **after** it in
  English. A translated manuscript usually carries the source language's
  convention throughout.
- The dotted capital `İ` from a Turkish keyboard leaking into English text.
  `İnstead` for `Instead` is the common one and survives spell-check.
- Turkish characters inside LaTeX comments are harmless. Separate comments from
  prose before reporting.

---

## 6. Output

A table of findings, then the corrected text for each. Apply them in one pass:
numbers shift again if floats move between the audit and the fix, so re-read the
`.aux` after any structural change and re-run.
