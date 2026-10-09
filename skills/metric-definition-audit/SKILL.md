---
name: "metric-definition-audit"
description: "Use when defining, reporting or interpreting an evaluation metric. Checks what the denominator holds, whether the metric is conditional on something that differs between the methods compared, whether its parts partition the whole, and whether a cross-method comparison of it is valid. Run before a results table is built, not after the discussion is written."
---

# Metric Definition Audit

## Core Rule
> Before comparing a metric across methods, establish what its denominator
> contains. A metric conditioned on an event the methods reach at different
> rates is not comparable between them, however the numbers look.

---

## 1. The failure this exists for

A metric can be correctly computed, consistently applied, and still not mean
what a reader takes it to mean.

The case from this project. Three quantities partition every reachable
node-step: success, power loss, channel loss. Power loss is defined as the steps
where the attacker was **on the correct channel** but lacked sufficient power.
It is therefore conditional on channel match.

The consequence: a method that rarely matches the channel accumulates little
power loss, because the condition under which power loss can be counted is
rarely met. In the results, the tabular baseline showed a power loss of 7.23
against the oracle's 31.52, and read as the better power decision. It was the
opposite: the baseline was on the wrong channel in about 87% of steps.

Nothing was miscomputed. The metric's denominator simply differed between arms,
and no amount of staring at the table reveals that.

---

## 2. Questions to answer per metric

**What is the denominator?** Write it out. "Percentage of what, exactly."

**Is it conditional?** Does the metric only accumulate when some event occurs? If
so, name the event.

**Does the condition differ across the compared methods?** If it does, the metric
is not directly comparable and the text must say so at the point of
interpretation, not in a footnote.

**Do the parts partition the whole?** If several metrics are claimed to sum to
100, verify the sum on real data for every arm, including the references.

**Is the reference's value a decision or a limit?** An upper-bound solver's
residual loss may be physical rather than a mistake. Say which. Here the
oracle's 31.52% is the share of reachable steps that cannot be reached at full
power, set by the jamming radius, not a bad power decision.

**Is lower better?** For a conditional metric the answer is often "not
necessarily", and that must be stated.

**What is the unit and what is the base?** Percentage points against percent,
share of total against share of a subset, per-step against per-episode.

---

## 3. Procedure

1. For each metric, write the explicit numerator and denominator in words.
2. Compute it on one arm by hand from raw per-step records and confirm the
   pipeline agrees.
3. For a set claimed to partition, sum them per arm and check against the whole.
4. Identify every conditional metric and record the condition.
5. For each conditional metric, report the condition's rate per arm beside it.
   Here that is the channel-tracking rate beside the power loss.
6. Decide, before writing the discussion, whether a cross-arm comparison of each
   metric is valid. Record the decision.
7. Where a comparison is not valid, either report the conditional rate alongside
   or drop the comparison. Do not repair it in prose afterwards.

---

## 4. Reporting conditional metrics

Two things make a conditional metric safe to publish.

**State the condition where the metric is interpreted**, in the paragraph that
reads the table, not only where the metric is defined. A reader arriving at the
results will not have the definition in mind.

**Report the conditioning rate beside it.** Power loss beside channel tracking
makes the selection effect visible without any further explanation.

Resist the instinct to add a "corrected" or "normalized" column. A metric
conditioned differently per arm cannot be normalized into comparability by
rescaling, and the attempt produces a column whose own meaning then needs
explaining.

---

## 5. Sanity checks on a finished results table

- Every row of a partitioned set sums to the whole, for every arm.
- The reference floor is at or below every learned arm on the primary metric.
- The reference ceiling is at or above every learned arm on the primary metric.
  An arm above the ceiling means the ceiling is not a ceiling, or the arms were
  evaluated under different conditions.
- A metric that moves in the unexpected direction for one arm has an explanation
  written down before the draft goes further.
- Any arm performing below the uninformed floor is itself a finding worth
  stating, not a number to pass over.
