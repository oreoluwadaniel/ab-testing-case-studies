# Landing Page Redesign: A/B Test

**Test window:** 42 days · **Unit:** visitor · **Status:** Complete

---

## Quick summary

A redesigned landing page lifted conversion from 3.08% to 4.44%, a statistically significant +1.35 percentage-point gain (p = 0.0013). Bounce rate dropped and time on page went up at the same time, so the conversion number isn't standing alone. Engagement moved with it. Point-estimate value is roughly $144K/year on an assumed 71,000 annual visitors; even the conservative end of the confidence interval clears the estimated $50K development cost.

**Recommendation:** Ship the redesign.

---

## The setup

### Design

| Element | Detail |
|---|---|
| Control | Existing landing page |
| Treatment | Redesigned landing page |
| Randomization | Cookie-based, at the session level |
| Primary metric | Conversion (visitor completes the target action) |
| Guardrails | Bounce rate, time on page |
| Sample size | 8,200 unique visitors (4,120 control, 4,080 treatment) after de-duplicating repeat visits |
| Test | Two-proportion z-test |
| Duration | 42 days |

**H₀:** conversion(Treatment) = conversion(Control)
**H₁:** conversion(Treatment) ≠ conversion(Control), α = 0.05, two-sided

### A data quality note before any of the numbers below

The raw export had 8,435 rows but only 8,200 unique visitors — 235 people showed up twice inside the window, mostly the same visitor returning within a session or two. I rolled the dataset up to one row per visitor before running anything, keeping their first-assigned group and summing their engagement. Skipping that step and testing at the row level instead of the visitor level nudges the conversion rates in opposite directions (naive row-level gives 4.31% vs 3.00%, the correct visitor-level gives 4.44% vs 3.08%) and would have overstated the lift. Small detail, but it's the kind of thing that quietly breaks an A/B test if nobody checks for it.

---

## Was the randomization trustworthy?

| Check | Result | Verdict |
|---|---|---|
| Sample ratio (4,120 vs 4,080) | χ² p = 0.66 | Pass, no assignment bug |
| Device balance across arms | χ² p = 0.67 | Pass |
| Traffic source balance across arms | χ² p = 0.50 | Pass |
| Visitors appearing in both arms | 0 | Pass, no cross-contamination |

Nothing here suggested a broken randomizer, so the primary result can be read at face value.

---

## Primary result: conversion

| Group | Visitors | Conversions | Rate | 95% CI |
|---|---|---|---|---|
| Control | 4,120 | 127 | 3.08% | — |
| Treatment | 4,080 | 181 | 4.44% | — |
| **Difference** | — | +54 | **+1.35pp** | **[+0.53pp, +2.18pp]** |

z = 3.22, p = 0.00127. If the redesign genuinely did nothing, a gap this size would show up in roughly 1 out of 800 experiments by chance alone. That's a lot more confidence than the 1-in-20 bar of α = 0.05.

The 95% CI says the true lift is plausibly anywhere from +0.53pp to +2.18pp. The business case in this document leans on the lower bound, not the flashier point estimate, because that's the number I'd still be comfortable defending if the true effect turned out to be on the weak end of plausible.

---

## Guardrails: bounce rate and time on page

| Metric | Control | Treatment | Test | p-value |
|---|---|---|---|---|
| Bounce rate | 64.6% | 57.9% | Two-proportion z | < 0.000001 |
| Time on page (median) | 16.9s | 20.0s | Mann-Whitney U | < 0.000001 |

Time on page is heavily right-skewed (skew ≈ 6.25), which is why it's tested with Mann-Whitney rather than a t-test. A handful of very long sessions would otherwise drag the mean around and make a parametric test unreliable. Both guardrails moved the same direction as conversion. That consistency is what makes we trust the primary result rather than suspect a tracking artifact.

---

## Segment analysis

Bonferroni-corrected α for 8 segment tests: 0.05 / 8 = 0.00625.

| Segment | n | Lift | p-value | Survives correction |
|---|---|---|---|---|
| Desktop | 4,431 | +1.55pp | 0.0126 | No |
| Mobile | 3,212 | +0.77pp | 0.1718 | No |
| Tablet | 557 | +2.92pp | 0.1020 | No |
| Direct | 1,387 | +3.15pp | 0.0094 | No |
| Organic search | 2,863 | +0.34pp | 0.6270 | No |
| Paid search | 2,126 | +2.14pp | 0.0129 | No |
| Referral | 761 | +0.69pp | 0.6367 | No |
| Social media | 1,063 | +0.44pp | 0.4991 | No |

None of the individual segments survive Bonferroni correction on their own, which is expected: each segment carries a fraction of the total sample, so they're individually underpowered by design. What matters is that every segment points in the same direction (positive), with no platform or channel showing a negative or reversed effect. That's the signal to act on here, not any single cell's p-value.

---

## Why peeking is dangerous (and why this matters more than it sounds)

The notebook includes a day-by-day trace of the cumulative p-value across all 42 days. It crosses the 0.05 line several times, in both directions, before landing at its final 0.00127 — meaning a team that stopped the test the moment it first looked "significant" around day 10–15 could easily have called it 8 days early on a fluke, or worse, killed a winning test because it dipped back above 0.05 the following week. This is the practical argument for fixing a test duration in advance and not touching the dashboard's "is it significant yet" button every morning.

---

## Business impact

At an assumed 71,000 annual visitors and $150 value per conversion:

| Scenario | Lift | Extra signups/year | Value/year |
|---|---|---|---|
| Conservative (CI lower bound) | +0.53pp | ~377 | ~$56,500 |
| Point estimate | +1.35pp | ~961 | ~$144,000 |
| Optimistic (CI upper bound) | +2.18pp | ~1,546 | ~$232,000 |

Against an estimated $50K development cost, even the conservative case clears breakeven with room to spare.

---

## Files in this folder

- `analysis.ipynb`: the full pipeline. Visitor-level rollup, randomization checks, primary z-test, guardrail tests, segment analysis, the peeking chart, and the business impact scenarios
- `landing_page_ab_test_dataset.csv`: raw visitor-session export, one row per visit
- `landing_page_visitor_level.csv`: the de-duplicated, visitor-level analytical table the analysis actually runs on
- `landing_page_test_summary.csv`: the final aggregated result, ready to feed a BI tool
- `landing_page_ab_dashboard.pbix`: the Power BI executive dashboard built from the summary output

---

## Limitations

- Synthetic data, built to mirror real experimental structure but not drawn from an actual production system.
- Financial figures are scenario estimates based on stated traffic and value assumptions, not observed revenue.
- Segment findings are directional, not independently proven. None survive multiple-comparison correction on their own.
- 42 days captures one season and one set of external conditions. A real rollout would benefit from a longer observation window before fully retiring the control experience.
