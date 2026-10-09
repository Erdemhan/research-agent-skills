---
name: "parameter-provenance-trace"
description: "Use when adopting, defending or auditing a physical constant, model coefficient or declared design value. Traces each value to its primary source through intermediate citations, assigns it a provenance status, and blocks unsettled values from reaching a draft. Use before writing any number into a methods section, and when a referee could ask 'why this value?'."
---

# Parameter Provenance Trace

## Core Rule
> A design value may not be filled in from our own judgement alone. Either it is
> derived from something external — a standard, a datasheet, a peer-reviewed
> measurement — or it is declared as a design decision and defended on
> operational grounds. What it may not be is silently chosen.

---

## 1. Why the intermediate link matters

A value quoted from a paper is rarely that paper's own. Follow it until it stops
moving.

A real chain from this project:

```
our L_sys = 2.0 (3 dB)
  → Sun et al. (2021), Table I: eta_1 = 3 dB for LoS
      → attributed to Mozaffari et al.
          → Al-Hourani et al. (2014): excess path loss per propagation group,
            drawn as N(mu, sigma^2(theta))
```

Only at the third hop does the meaning become clear: the 3 dB is the **mean** of
a distribution whose **standard deviation** is the shadowing term applied
separately. Stopping at hop one leaves two quantities looking like two unrelated
losses, which is how they end up described as such in the text.

Record every hop. The intermediate paper is where a value most often gets
restated, rounded, or attached to the wrong symbol.

---

## 2. Status vocabulary

Assign one status per parameter. Nothing below `BACKED`, `MEASURED` or
`DECLARED` may be written into a draft.

| status | meaning | may appear in the paper |
| --- | --- | --- |
| `OPEN` | no source, no measurement | no, in either direction |
| `TO VERIFY` | a source is named but the primary document has not been read | no |
| `BACKED` | verified against the primary source | yes, quotable |
| `MEASURED` | settled by a reproducible measurement in this repository | yes, with the script named |
| `DECLARED` | a design decision, defended operationally | yes, stated positively |
| `CLOSED` | fully settled, kept as the record of how | yes |

`TO VERIFY` is the dangerous one. A named source is not a verified source: it is
the state in which a wrong citation survives longest, because the entry looks
complete.

---

## 3. The entry

One file per parameter, or one row per parameter in a central register.

```markdown
# E03 — air shadowing and link margin

**Status:** BACKED

## Value
sigma_air = 2.0 dB

## Where it comes from
Andreadis et al. (2023), LoRa measurement at 868.1 MHz, SF7, 125 kHz bandwidth,
14 dBm transmit power, 2 dBi antennas. Gateway raised 10 m on a hilltop to
emulate a UAV-borne receiver, nodes at ground level, line of sight, 0.5-3 km.

## Why this source and not another
The radio configuration matches ours exactly. Petajajarvi et al. (2015) report
sigma = 7.8 dB but from a 24 m tower over a wide area, which is a different
geometry.

## What it is a moment of
The standard deviation of the excess loss whose mean is carried by L_sys. The
two are not separate losses.

## Verified
Primary document read on <date>. Table and page noted: <...>.
```

---

## 4. Declared values

When no source fits, declare rather than imply. A declared value carries:

- the range the literature does report, with each study named
- which part of that range was taken and why
- the operational reason, stated positively

Example from this project: the ground-to-ground path loss exponent. Reported
exponents across four near-ground studies cluster in 1.75-3.69, with one outlier
at 6 from a low-height peer-to-peer link. The declared value 3.5 takes the upper
half of the concentrated band. The operational reason is that a steeper exponent
compresses distant nodes toward the noise floor and therefore makes the
attacker's inference harder, so the choice does not flatter the reported threat.

Never defend a declared value by denying an alternative the reader has not been
shown.

---

## 5. Audit procedure

Run this over a methods section before submission.

1. Extract every numeric constant and every model coefficient in the section.
2. Match each to a register entry. A number with no entry is a finding.
3. For each `TO VERIFY`, open the primary document and check the value, the
   symbol it is attached to, the measurement conditions, and the units.
4. Flag any value whose source conditions differ materially from ours —
   frequency band, antenna height, environment class, bandwidth.
5. Flag any value that appears in the text with more precision than its source
   supports.
6. Report the parameters still at `OPEN` or `TO VERIFY` and name the sentences
   that depend on them.

---

## 6. Failures this catches

- A value reported under the wrong symbol: a path loss exponent of 6 written
  into the text as a shadowing standard deviation of 6.0 dB.
- A measurement band silently changed: a 470 MHz study cited as 868 MHz.
- A value whose primary source turns out to describe a different quantity, found
  only after following the citation two hops further than the bibliography goes.
- Two quantities that are the mean and the spread of one distribution, described
  in the text as two independent losses.
