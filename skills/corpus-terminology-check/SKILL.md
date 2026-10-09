---
name: "corpus-terminology-check"
description: "Use before adopting, keeping or changing a technical term in a manuscript. Counts how the term and its alternatives actually occur across the reference PDFs the project holds, rather than relying on recollection of what a field calls something. Answers 'is this the standard name here?' with evidence."
---

# Corpus Terminology Check

## Core Rule
> Do not assert what a field calls something from memory. Count it in the
> sources the project actually holds, and report the count.

---

## 1. When this runs

- A reviewer or colleague asks whether a term is standard.
- A term is being introduced that may already have a name in the field.
- Two terms are in use for one concept and one has to be chosen.
- A term borrowed from a neighbouring field is about to be used in this one.

---

## 2. Procedure

**Step 1. Name the candidates.** The term in use plus every plausible
alternative. For a full-knowledge benchmark the candidate set is: `oracle`,
`genie-aided`, `clairvoyant`, `omniscient`, `upper bound`, `perfect CSI`,
`exhaustive search`, `benchmark`, `ideal case`.

**Step 2. Count across the corpus.** Extract the text of each reference PDF and
count each candidate, per document. Report documents-containing as well as raw
occurrences: one paper using a word forty times is weaker evidence than ten
papers using it twice.

**Step 3. Read the hits in context.** A term can occur in the right field with
the wrong sense. `upper bound` appears in optimisation papers meaning a relaxed
problem's optimum as well as meaning a benchmark scheme. Separate the senses
before concluding.

**Step 4. Decide and record.** Keep, change, or keep with a bridging gloss on
first use. Write the counts into the decision record so the question is not
reopened from memory later.

---

## 3. Worked example

The question: is `oracle` what this field calls a full-knowledge benchmark?

Counts across the 41 reference PDFs of this project:

| term | documents |
| --- | --- |
| `benchmark` | many, 77 occurrences |
| `upper bound` | 10 |
| `full knowledge` | 3 |
| `perfect CSI` | 3 |
| `exhaustive search` | 3 |
| `oracle` | **0** |
| `genie-aided` | 0 |
| `clairvoyant` | 0 |
| `omniscient` | 0 |

The conclusion is not "the term is wrong". It is "the term does not come from
this corpus". A second check against the methodology literature showed `oracle`
is established there in exactly this sense, with `oracle ceiling` and
`oracle-style quantities ... reported as upper bounds on what a deployable
selector can achieve` both attested.

So the term stays, because the paper defines it on first use and the sense is
established in the literature it comes from. What the count changed was the
confidence: the decision is now recorded with evidence rather than defended from
recollection.

---

## 4. Reading the result

**Zero in the corpus is not a verdict.** It means the term is not native to the
documents cited, which may simply mean none of them is a methodology paper. The
next move is to check the field the term does come from, and to confirm the
manuscript defines it on first use.

**High count in one sense only** means the term is safe but the sense must match.

**Split counts** across two spellings or two forms is a consistency finding, not
a terminology finding: pick one and apply it throughout.

**A term absent everywhere** including the methodology literature is an invented
term. Either use the established name or describe the thing plainly without
naming it. Where a recurring concept genuinely needs a name, propose it
explicitly rather than letting it settle in by repetition.

---

## 5. Keeping the corpus usable

This skill is only as good as the reference folder. Keep every cited PDF in one
place, named so that a bibliography key maps to a file. A corpus with gaps
produces counts that understate, and an understated count reads as evidence of
absence.
