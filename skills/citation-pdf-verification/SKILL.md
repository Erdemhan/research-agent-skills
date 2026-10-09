---
name: "citation-pdf-verification"
description: "Use when auditing a bibliography against the PDFs the project holds. Matches each bib entry to its file, compares the entry's title against the document's own first pages, and checks that the claim each citation supports appears in that document. Mechanical counterpart to an integrity review, which states the principle but not the procedure."
---

# Citation and PDF Verification

## Core Rule
> A citation is verified when someone has opened that document and found the
> claim in it. A key that resolves, a DOI that exists and a file that is present
> are three different things, and none of them is verification.

---

## 1. The failure mode

The most durable citation error is not a missing reference. It is a reference
that points at a real paper by the right authors which does not contain the
claim.

The case from this project. A 2023 reference was cited for a defence using
reconfigurable intelligent surfaces. The DOI resolved to exactly that paper. The
PDF in the reference folder, filed under the same author and year, was a
different paper by the same author about clustering. The description written
into the related-work section came from the file, not from the cited work, and
described the wrong method.

It survived a full draft and was caught only because someone asked an unrelated
question about that paper's method. An automated title comparison catches it in
seconds.

---

## 2. Stage one: file to entry

Build the mapping from bibliography key to file, then score each pair.

1. Extract the title from each bib entry.
2. Extract the first one or two pages of the matched PDF as text.
3. Reduce both to content words, dropping stop words and short tokens.
4. Score the overlap: the fraction of the title's content words present in the
   document's opening text.
5. Flag anything below a threshold around 0.6, and flag every missing file.

A low score means one of: the wrong file, a scanned PDF with no text layer, a
title changed between preprint and publication, or a file that is a different
work by the same author.

This stage is cheap and runs over the whole bibliography at once. Run it after
any change to the reference folder.

---

## 3. Stage two: claim to document

Scoring titles proves the file is the work. It does not prove the work supports
the claim. For each citation in the text:

1. Read the sentence and state what the citation is being used to support.
2. Find that specific content in the document. A value, a method, a conclusion.
3. Record where: section, table, page.
4. Compare the specifics. Values, units, bands, conditions.

Three real findings from doing this over 41 references:

- An exponent of 6 reported in the manuscript as a shadowing standard deviation
  of 6.0 dB. Two different quantities from the same table.
- A measurement campaign at 470 MHz described in the text as 868 MHz.
- A described method that belonged to a different paper by the same author.

None of these is a missing citation, and none is caught by checking that a key
resolves.

---

## 4. Rules that make the audit possible

- Every cited work must be held as a readable file. A source we cannot open
  cannot be verified, and an unverifiable citation should be replaced with a
  reachable equivalent or the claim dropped.
- One flat folder, with file names that map to bibliography keys by a stated
  convention: author and year is enough when applied consistently. Nested
  folders and ad hoc names are where mismatches hide.
- When a file is replaced, re-run stage one for that key.
- Record the verification: which page carried the claim. A claim verified once
  and not recorded gets re-verified at every revision.

---

## 5. Reporting

Per entry: key, file, title score, verdict, note.

Verdicts: verified, file mismatch, claim not found, claim differs in detail,
file missing, no text layer.

Report the sentences that depend on an unverified citation, not only the
citation. The sentence is what has to change if the verification fails.

---

## 6. What this does not cover

Whether the cited work is the right work to cite, whether a stronger source
exists, and whether the claim needed a citation at all. Those are editorial
judgements. This skill establishes only that the citation says what the
manuscript says it says.
