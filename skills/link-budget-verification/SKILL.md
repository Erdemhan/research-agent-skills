---
name: "link-budget-verification"
description: "Use when a wireless or RF quantity is reported in a paper, a report or a config: noise floor, time on air, receiver sensitivity, minimum effective power, interference range, SINR margin. Recomputes each one from the declared constants, checks the dB and linear scales are not mixed, and reports every value that does not reproduce."
---

# Link Budget Verification

## Core Rule
> Every derived radio quantity in the text must be reproducible from the
> declared constants alone. A number that cannot be recomputed is a finding,
> however plausible it looks.

---

## 1. What to recompute

Build the chain from the declared constants outward. Each step is checkable.

| quantity | from | typical form |
| --- | --- | --- |
| Noise floor | bandwidth, noise figure | `-174 dBm/Hz + 10*log10(B) + NF` |
| Reference channel gain | carrier frequency, excess loss | `(1/L_sys) * (c / (4*pi*f))^2` |
| Received power | transmit power, distance, exponent | `P_tx * beta_0 / max(d,1)^eta` |
| Time on air | SF, bandwidth, payload, preamble, coding rate | symbol count × symbol duration |
| Duty-cycle silence | time on air, regional duty limit | `ToA * (1/duty - 1)` |
| Minimum effective power | SINR threshold, geometry | invert the SINR equality |
| Interference radius | minimum effective power, altitude | invert, then project to ground |

Worked examples from this project, all of which were recomputed by hand before
being trusted:

```
N_0        = -174 + 10*log10(125e3) + 6            = -117.03 dBm
ToA        = 210.25 symbols * 1.024 ms             ~ 215.3 ms
silence    = 215.3 ms * (1/0.01 - 1)               ~ 21.3 s
P_min      = (H / r_slant)^2 = (100/266.70)^2      = 0.1406 W
r_ground   = sqrt(r_slant^2 - H^2)                 = 247.2 m
```

---

## 2. The dB trap

Most link-budget errors are scale errors, not arithmetic errors.

**Powers add in the linear scale, never in dB.** Noise and two interference
terms must be summed in watts and only then converted. Subtracting three dB
values from a signal's dB value is wrong and the error is silent.

**A ratio in the linear scale is a difference in dB.** SINR written as

```
SINR = P_signal / (N_0 + I_jam + I_co)
```

is a linear ratio. Its dB form is `P_dBm - (N_0 + I_jam + I_co)_dBm`, with the
denominator summed first. Writing the equation as a ratio is what keeps the
summation order visible.

**A threshold stated in dB is not a factor.** A threshold of -7.5 dB is a linear
ratio of `10^(-0.75) = 0.178`, meaning the signal may be about 5.6 times weaker
than the sum of noise and interference and still decode. Anyone reading the
ratio equation and the dB threshold together will mis-set this unless the text
or the code makes the conversion explicit.

**Check the code's comparison scale.** Verify that the threshold is converted
once, on the same side as the quantity it is compared against.

---

## 3. Variance combination

Shadowing terms in dB are Gaussian. Their combinations follow from that, not
from intuition.

- The difference of two independent `N(0, sigma^2)` terms has standard deviation
  `sigma * sqrt(2)`. For `sigma = 2.0 dB` this is `2.83 dB`.
- A zero-mean dB term contributes nothing to the mean path loss. It changes the
  spread only. A mean offset and a spread are two moments of one distribution
  and must not be added as if they were two losses.
- A term drawn once per episode and a term redrawn per measurement behave
  differently under repetition: the second averages out, the first does not.
  Say which is which.

---

## 4. Unit and dimension checks

- `dBm` against `dB` against `dBm/Hz`. A noise spectral density and a noise power
  differ by `10*log10(B)`.
- Linear power in watts against milliwatts, especially where a value is quoted
  both ways (`1.0 W` and `30 dBm`).
- Slant range against ground range. Any radius derived from a receiver at
  altitude is a slant range until `sqrt(r^2 - H^2)` is applied.
- Frequency in Hz against MHz inside a wavelength term.
- Energy against power. A detector statistic integrated over `T` is an energy.

---

## 5. Procedure

1. List every radio quantity stated in the text with a number attached.
2. Separate the declared constants from the derived values.
3. Recompute each derived value from the constants. Show the expression.
4. Compare against the text to the precision the text claims.
5. For each comparison, confirm the scale: linear or dB, and where the
   conversion happens.
6. Check the implementing function does the same operations in the same order,
   particularly the summation before the division.
7. Report every value that does not reproduce, and every value whose stated
   precision exceeds what the inputs support.

---

## 6. Failures this catches

- A derived constant that no longer matches the constants it was derived from,
  after one of those constants changed.
- A threshold compared in the wrong scale.
- Interference terms subtracted in dB instead of summed in watts.
- A slant range reported as a ground range.
- A combined standard deviation computed by adding rather than by adding in
  quadrature.
