---
name: "multilingual-manuscript-sync"
description: "Use when one study is written up in more than one document: a paper and its translation, a paper and a thesis chapter, a manuscript and a report. Carries a correction to every document that needs it, applies each document's own conventions rather than copying text across, and reports what was deliberately left unsynchronised."
---

# Multilingual Manuscript Synchronisation

## Core Rule
> A correction belongs to the finding, not to the document it was found in.
> Decide for every document whether it applies, and record the ones where it
> deliberately does not.

---

## 1. The setup this is for

One study, several documents, different conventions:

| document | language | class | tables | figures |
| --- | --- | --- | --- | --- |
| journal paper | English | two-column template | `tabularx` in `table*` | English labels |
| companion paper | Turkish | same template | same | English labels |
| thesis chapter | Turkish | institutional class | institutional format | Turkish labels |

A number corrected in one is wrong in the other two until carried. A
terminology decision made in one is inconsistent until applied. In this project
a figure-reference change was carried to two of three documents and the third
was found only on a later audit.

---

## 2. Classify every change

Before touching any file, decide which kind of change this is.

**Universal.** A corrected number, a corrected citation, a corrected equation, a
factual claim. Goes to every document, in that document's language.

**Convention-bound.** The same intent, different execution per document. Percent
sign placement, table environment, caption language, citation style, section
numbering.

**Document-specific.** Belongs to one document only. A journal's highlights
block, a thesis's institutional front matter, a length-driven cut made for one
venue. Record the decision so the next audit does not read it as drift.

A change recorded as universal but applied to one document is the failure this
skill exists to prevent.

---

## 3. Convention table

Keep one, per project, and consult it before every port.

| aspect | paper | thesis |
| --- | --- | --- |
| percent sign | after the number in English, before it in Turkish | before the number |
| tables | `tabularx` inside `table*` | institutional format, unchanged |
| figure labels | English | Turkish |
| confidence intervals in result tables | removed | removed |
| emphasis commands | none | none |
| caption length | two or three words | two or three words |

Never copy a sentence from one language's document into the other. Carry the
finding and write the sentence in the target document's language and register.
A translated sentence inherits the source language's conventions, which is how a
Turkish percent convention ends up in an English manuscript.

---

## 4. Procedure

1. State the finding once, in one sentence, independent of any document.
2. Classify it: universal, convention-bound, document-specific.
3. For each document, decide: applies, applies with a different execution, does
   not apply. Record all three outcomes.
4. Apply. For a convention-bound change, consult the table rather than the other
   document's text.
5. Recompile every document that changed.
6. Re-audit cross-references and language-specific suffixes in each, reading the
   resolved numbers from that document's own `.aux`.
7. Report what was applied where, and what was deliberately left alone.

---

## 5. Divergence audit

Independently of any single change, periodically check the documents against
each other:

- Every number that appears in more than one document agrees.
- Every shared table reports the same values.
- The same concept carries the same name within each document, and the two
  names correspond across languages.
- A figure that exists in one and not another is a deliberate decision with a
  record.
- A section present in one and absent in another is deliberate.

Report divergences as a list of pairs, with the value in each document, rather
than as prose.

---

## 6. When a document is frozen

Where one document is submitted or otherwise fixed, it stops receiving
universal changes. Say so explicitly in the record, because the next audit will
otherwise report every subsequent correction as a divergence. A frozen document
needs a dated note of what it was frozen at.
