# A/B Testing & Product Experimentation

Two product experiments built around one practical question:

**Did the change improve the customer outcome enough to justify shipping it?**

The datasets are synthetic. The purpose is to show the full decision process from experiment design and validation through statistical testing, uncertainty, and product recommendation.

| Experiment | Business question | Result |
|---|---|---|
| [Landing Page Redesign](landing-page/) | Does the new page convert more visitors? | **+1.35pp conversion lift** |
| [Smart Onboarding Assistant](smart-onboarding/) | Does guided onboarding improve retention? | **+4.11pp D7 retention lift** |

---

## 01. Landing Page Redesign

**Decision:** Should the redesigned landing page replace the current version?

The test covered **8,200 visitors over 42 days**.

| Metric | Control | Treatment |
|---|---:|---:|
| Conversion | 3.08% | **4.44%** |
| Absolute lift | | **+1.35pp** |
| Relative lift | | **+43.9%** |
| p-value | | **0.0013** |
| 95% CI | | **+0.53pp to +2.18pp** |

Bounce rate also fell and time on page increased, giving the conversion result supporting evidence rather than leaving it as a standalone metric.

**Recommendation:** Roll out the redesign for the tested traffic and continue monitoring conversion after release.

[Read the full landing page case study →](landing-page/)

---

## 02. Smart Onboarding Assistant

**Decision:** Does guided onboarding improve early retention?

The experiment covered **7,500 new mobile users**.

| Metric | Control | Treatment |
|---|---:|---:|
| D7 retention | 36.54% | **40.65%** |
| Absolute lift | | **+4.11pp** |
| Relative lift | | **+11.3%** |
| p-value | | **0.0003** |
| 95% CI | | **+1.91pp to +6.31pp** |

There is an important product signal behind the headline result: only **37.8% of treatment users used the Assistant**.

That makes the next question more useful than simply declaring the experiment a win:

**Can better exposure increase Assistant adoption and extend the retention benefit to more users?**

[Read the full onboarding case study →](smart-onboarding/)

---

## How I evaluate an experiment

A higher treatment number is not enough.

Before recommending a product change, I check:

- Was assignment randomized correctly?
- Is the observed difference statistically reliable?
- How large could the real effect reasonably be?
- Does the result hold across important segments?
- Are supporting user behaviours moving in the same direction?
- Is the improvement large enough to matter commercially?
- Did the experiment have enough statistical power?
- Are we measuring everyone assigned to treatment or only people who chose to use the feature?

The final output is not a p-value.

**It is a product decision.**

---

## Statistical approach

**Primary conversion and retention metrics**  
Two-proportion z-tests for binary outcomes.

**Engagement metrics**  
Mann-Whitney U tests for skewed session and screen-count data.

**Uncertainty**  
95% confidence intervals reported alongside p-values.

**Segment testing**  
Bonferroni correction used to control false positives across multiple segment comparisons.

**Causal interpretation**  
Intent-to-treat is the primary analysis. Adopter analysis is treated as supporting evidence, not as proof of causality.

**Experiment design**  
Power and minimum detectable effect are considered before interpreting the size of the result.

---

## What this project demonstrates

```text
Product idea
     ↓
Controlled experiment
     ↓
Data validation
     ↓
Statistical testing
     ↓
Effect size + uncertainty
     ↓
Business impact
     ↓
Product decision
