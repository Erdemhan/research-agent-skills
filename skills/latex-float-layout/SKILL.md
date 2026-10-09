---
name: "latex-float-layout"
description: "Use when figures and tables sit badly in a LaTeX document: float-only pages, half-empty columns, a figure too small to read, a float that overflows its page. Measures page density, computes on-page figure size from aspect ratio, and diagnoses the float-placement commands that cause these. Complements compile-error debugging, which this is not."
---

# LaTeX Float Layout

## Core Rule
> A figure's legibility is set by the ratio between the size it was drawn at and
> the size it occupies on the page. Measure that ratio before changing anything
> else.

---

## 1. Measure before editing

**Page density.** Extract the text of each page and count words. A page under
about 60 words is float-only. A run of them means the floats are queued, not
badly sized.

```
pdftotext -f N -l N doc.pdf - | word count
```

**Figure size on the page.** For a two-column class with a text width of about
17.6 cm, an image at `\linewidth` inside `figure*` occupies the full text width
regardless of its own pixel dimensions. What varies is the shrink factor:

```
shrink = authored width (inches) / displayed width (inches)
```

A figure drawn at 16 inches and shown at 6.93 inches shrinks by 2.31, so a 13 pt
label renders near 5.6 pt. The same figure redrawn at 13 inches shrinks by 1.88
and the label lands near 6.9 pt. Nothing about the page changed.

This is the measurement that matters and it is rarely made. A figure that looks
small is usually drawn too wide, not placed too small.

**Vertical budget.** Inside 128 px margins on a 1080-tall canvas, or inside a
24 cm text height on A4, compute what each float costs:

```
height = width / aspect ratio
```

Sum heading, body, floats and gaps before concluding a slide or page is
over-full.

---

## 2. The placement commands that cause trouble

**`\FloatBarrier` in a two-column class.** It forces every pending float out
before the barrier, which empties the remaining column. In one document removing
fourteen barriers took the page count from 28 to 23 with no other change. Use it
only where a float genuinely must not cross a boundary.

**A full-width strip inside a list.** A `strip`-style environment placed inside
`itemize` duplicates the body text on that page. Keep full-width material at the
top level, and break a wide equation with `split` inside the column instead.

**Global column-spec replacement.** Changing every table's column specification
at once breaks the tables that had a different column count, surfacing as
`Extra alignment tab has been changed to \cr`. Change specs per table, keyed by
label.

**`split` without a line break.** An environment intended to break an equation
across lines renders as one long line when the `\\` is missing, and silently
overflows the column.

**Stale or truncated `.aux`.** An interrupted compile leaves a half-written
auxiliary file and the next run fails with `! File ended while scanning use of
\@newl@bel`. Delete the auxiliary files and rebuild.

---

## 3. Making a figure bigger

Options, in the order worth trying:

1. **Redraw it narrower.** If the figure is generated, reduce the authored width
   while keeping font sizes in points. The content grows relative to the frame
   at no cost in page space. This is almost always the right move and the one
   most often missed.
2. **Merge a pair into one figure.** Two images placed side by side at half width
   each shrink to a third of their drawn size. One figure drawn for the full
   width, with both panels inside it, shares one title and one legend and gains
   real area. In one document this made labels about 1.7 times larger with no
   extra height.
3. **Split a composite.** Only where panels are interpreted individually. A
   six-panel figure split into six column-width figures usually costs more page
   height than it gains in legibility: count the total before committing.
4. **Stack a pair vertically at full width.** Doubles the drawn size and roughly
   quadruples the height. Check the result still fits the text height with its
   caption, or it cannot be placed at all.

---

## 4. Reducing float pages

- Remove barriers that are not load-bearing.
- Check whether a figure is interpreted panel by panel in the text. One that is
  only pointed at is a candidate for the appendix or for removal, and those are
  usually the largest.
- Check for duplication between a figure and a table carrying the same numbers.
- Prefer one wide float to two half-width ones: fewer floats queue more easily.

---

## 5. Verification after any change

1. Recompile twice so references settle.
2. Check for unresolved references and overfull boxes, and read the overfull
   amounts rather than the count. A box a point too wide is noise; one 120 pt
   too wide is visible.
3. Re-measure page density.
4. Re-audit cross-references and any language-specific suffixes attached to
   numbers, because float changes renumber.
5. Render the affected pages and look at them.
