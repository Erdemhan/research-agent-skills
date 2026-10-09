---
name: "equation-code-correspondence"
description: "Use when a paper states an equation that the code also implements. Checks the LaTeX and the implementing function term by term: every symbol bound, every factor present, operation order preserved, conditions and guards matched. Complements hyperparameter auditing, which compares values rather than formulas."
---

# Equation and Code Correspondence

## Core Rule
> The equation in the paper and the function in the code are two statements of
> one thing. Check them against each other symbol by symbol, not by reading both
> and finding them agreeable.

---

## 1. What this is not

Auditing reported hyperparameters against config files compares **values**. This
compares **formulas**. A study can have every constant correct and still publish
an equation the code does not implement.

The common forms of that divergence:

- A guard present in the code and absent from the equation, such as a clamp on
  distance or a small constant added to a denominator.
- A factor the code applies at the call site rather than inside the function, so
  the function alone does not match the equation and the equation alone does not
  match the function.
- An indicator or condition stated in the equation but implemented as a branch
  that also changes something else.
- Operation order that matters numerically, such as summing in one scale before
  converting to another.

---

## 2. Procedure

**Step 1. Pair them.** For each labelled equation, name the function, file and
line that implements it. An equation with no implementation is either theory or
a finding. A function with no equation is undocumented method.

**Step 2. Build a symbol table for the pair.**

| symbol in the paper | name in the code | where it is bound |
| --- | --- | --- |
| `P_tx` | `p_tx` | argument |
| `beta_0` | `UAVConfig.BETA_0` | config constant |
| `eta` | `eta` | argument, per link class |
| `d` | `distance` | argument |

Every symbol in the equation must appear in this table. An unbound symbol means
the equation says something the code does not read.

**Step 3. Walk the expression.** Compare term by term, in the order the code
evaluates them. Note any term present on one side only.

**Step 4. Check the guards.** Any `max`, `min`, `clip`, epsilon, or lower bound
in the code either appears in the equation or is a documented deviation. In this
project the received-power equation carries `max(d, 1.0)` precisely because the
reference gain is defined at one metre, and the equation shows it.

**Step 5. Check what is applied elsewhere.** Some factors are deliberately kept
out of the deterministic core and applied at the point of use. The shadowing
multiplier here is one: `calculate_received_power` does not include it, and the
environment step does. When that is the design, the paper must say so, and the
pairing must name both sites.

**Step 6. Check the comparison scale.** Where the equation produces a ratio and
the code compares against a threshold, confirm the conversion happens once and
on the correct side.

---

## 3. Conditions and indicators

An indicator function is the most commonly mis-implemented element.

```
I_jam = P_J * beta_0 / max(d,1)^eta * g_J * 1[ch_J == ch_k]
```

Check that:

- the condition in the code is the same condition, not a near relative such as
  "the jammer transmitted" or "the power exceeded the threshold"
- a false condition yields exactly zero, not a small residual
- the condition is evaluated against the same time index the equation uses

A piecewise reward is the same case. Each branch's condition set must match, and
conjunctions must be conjunctions. A reward defined on three conditions joined
by logical AND is a different function from one that drops the third.

---

## 4. Sequencing

Where an equation describes a step within a cycle, the cycle order is part of
the method. Verify the implementation calls things in the stated order.

This project carries a live example in its history: a step that fired before
sensing rather than after was a methodology-breaking defect, not a cosmetic one,
because listen-then-fire is what makes the threat model realizable.

---

## 5. Output

Report as a table, one row per equation:

| equation | implementation | verdict | note |
| --- | --- | --- | --- |
| `eq:rx_power` | `core/physics.py:calculate_received_power` | matches | guard documented |
| `eq:sinr` | `core/physics.py:calculate_sinr` | matches | denominator summed linearly before division |
| `eq:sensing` | `simulation/pettingzoo_env.py:_sense_channel_energy` | — | |

Verdicts: matches, diverges, undocumented deviation, no implementation, no
equation.

A divergence is not automatically a code defect. Decide which side is wrong, and
where a formulation record exists and is authoritative, the code is what gets
corrected.
