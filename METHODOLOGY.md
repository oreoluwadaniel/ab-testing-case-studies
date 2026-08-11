# A/B Testing Methodology & Statistical Foundation

This document explains the statistical choices, test selection, and rigor behind both experiments.

---

## Experimental Design Principles

### Why Randomization Matters

Both experiments used 50/50 random assignment at the user/visitor level. Randomization does one critical thing: it ensures treatment and control groups are exchangeable (statistically identical in expectation before the experiment).

This allows us to attribute differences in outcomes to the treatment, not to pre-existing differences between groups.

**What randomization protects against:**
- Selection bias (users who choose treatment might be different)
- Confounding (unmeasured variables correlated with both treatment and outcome)
- Temporal trends (if we assigned by date rather than randomizing, time-of-day effects would confound results)

**Implementation:**
- Landing Page: Cookie-based session randomization (visitor sees either A or B)
- Smart Onboarding: Device-level assignment at user creation (persistent across sessions)

Both maintained ~50/50 split (validated via χ² test).

---

## Statistical Tests & Why We Chose Them

### Two-Proportion Z-Test (Landing Page & Smart Onboarding Primary Metrics)

**What it answers:** Does the treatment conversion/retention rate differ from control?

**Why we used it:**
- Both experiments have binary outcomes (Converted: Yes/No; Retained: Yes/No)
- Large sample sizes (n > 3,000 per arm) make the normal approximation valid
- It's interpretable: produces a p-value and confidence interval directly

**Formula:**
```
z = (p₁ - p₂) / sqrt(p_pool * (1 - p_pool) * (1/n₁ + 1/n₂))

where:
  p₁ = treatment conversion rate
  p₂ = control conversion rate
  p_pool = pooled proportion across both groups
  n₁, n₂ = sample sizes
```

**Output:**
- z-statistic: How many standard errors the observed difference is from zero
- p-value: Probability of observing this data (or more extreme) if the null hypothesis is true
- 95% CI: Range where we're 95% confident the true effect lives

**Example (Landing Page):**
- p₁ (treatment) = 0.0431
- p₂ (control) = 0.0300
- Difference = 0.0131 (1.31pp)
- z = 3.09
- p-value = 0.0013
- 95% CI = [0.0053, 0.0218]

This means: If there's truly no effect, we'd see a difference this large only 0.13% of the time by random chance.

### Mann-Whitney U Test (Smart Onboarding Guardrail Metrics)

**What it answers:** Do treatment and control distributions differ (without assuming normality)?

**Why we used it for session length and screens viewed:**
- Both metrics are heavily right-skewed (many users with short sessions, few with very long)
- Skewness ≈ 3.3–3.6 (extreme non-normality)
- Session length is hard-capped at 7,200s by the logging system
- Mean and median diverge significantly (mean is inflated by outliers)

**The Alternative (Welch's t-test):**
We also ran Welch's t-test (doesn't assume equal variances) and got p < 0.01 for both metrics. The Mann-Whitney results align, so test choice doesn't matter here, though Mann-Whitney is more robust to distributional violations.

**Interpretation:**
Mann-Whitney p-value: Probability of observing this data if treatment and control come from the same distribution.

---

## Confidence Intervals & Why They Matter More Than P-Values

A p-value tells you: Is the effect likely to be real? (It is, if p < 0.05.)

A confidence interval tells you: What's the plausible range of the true effect?

### Landing Page CI: [+0.53pp, +2.18pp]

**Interpretation:** We're 95% confident the true treatment lift falls between 0.53pp and 2.18pp.

**Business Use:**
- Lower bound (+0.53pp): Worst plausible case still covers development cost
- Upper bound (+2.18pp): Best plausible case drives massive revenue impact
- Decision rule: Accept if lower bound exceeds breakeven (it does: $212K > $50K cost)

### Smart Onboarding CI: [+1.91pp, +6.31pp]

**Interpretation:** We're 95% confident the true retention lift falls between 1.91pp and 6.31pp.

**Business Use:**
- Lower bound (+1.91pp): $764K revenue impact, exceeds $400K breakeven by 91%
- Upper bound (+6.31pp): $2.52M revenue impact
- Decision rule: Accept because even worst case clears breakeven

**Why CIs > p-values for decisions:**
- p-values answer: Is it real? (yes/no, binary)
- CIs answer: How big is it? (range, actionable)
- Over-reliance on p-values causes winner's curse (inflated effect estimates in underpowered studies)

---

## Multiple Comparisons & Bonferroni Correction

When we test multiple hypotheses (e.g., seven app version segments), the probability of a false positive increases.

**Example:**
- Single test at α = 0.05: 5% chance of false positive
- Seven independent tests at α = 0.05: ~30% chance of at least one false positive

**Solution: Bonferroni Correction**

Divide α by the number of tests:
```
α_corrected = 0.05 / 7 = 0.0071
```

We report p-values for all segments, then flag only those surviving the corrected threshold (p < 0.0071).

**In Smart Onboarding Segment Analysis:**
- iOS (+3.9pp): p = 0.0043 ✓ (survives correction)
- Android (+4.4pp): p = 0.0031 ✓ (survives correction)
- All versions positive direction (v3.3.0 outlier noted but not actioned)

This prevents claiming a segment effect that's just random noise.

---

## Intent-to-Treat vs. Per-Protocol Analysis

### Smart Onboarding Context

We randomized users to receive (or not receive) the onboarding assistant, but didn't force engagement. This creates a choice point.

### Intent-to-Treat (ITT)

**Definition:** Analyze all users in their assigned arm, regardless of whether they used the feature.

**Our Primary Result:** +4.11pp retention

**Pros:**
- Answers the practical question: "If we ship to 100%, what happens?"
- Avoids selection bias (users who explore features tend to retain anyway)
- Respects randomization (preserves causal comparison)

**Cons:**
- If adoption is low, the treatment effect is diluted
- Doesn't directly tell us "how good is the feature for users who actually use it?"

### Per-Protocol (Adopter Analysis)

**Definition:** Compare only users who used the feature to all controls.

**Our Adopter Result:** 50.5% (adopters) vs 34.7% (non-adopters) = +15.8pp

**Pros:**
- Shows feature quality: "Among users who engage, how well does it work?"

**Cons:**
- Selection bias: Users who explore features are different (more engaged, less likely to churn anyway)
- Not causal: We can't claim the feature caused all +15.8pp
- Misleading for ship decisions: If only 38% adopt, the business impact is +4.11pp, not +15.8pp

### Why ITT is the Right Answer

For deployment decisions, ITT is the correct comparison. It says: "If we ship to everyone, retention improves by 4.11pp." The fact that 62% don't engage is a feature quality problem (they don't see the value) or a discoverability problem (they don't find it).

The adopter analysis reveals the ceiling: the feature works brilliantly for those who find it, so the next test should focus on discoverability, not feature quality.

---

## Statistical Significance vs. Business Significance

### Statistical Significance

**Definition:** The observed difference is unlikely to be due to random chance.

**Test:** p-value < 0.05

**Example (Landing Page):** p = 0.0013  
Interpretation: Only a 0.13% chance of this data if there's truly no effect. Highly significant.

### Business Significance

**Definition:** The difference is large enough to justify action and cost.

**Test:** Does the lower bound of the CI exceed breakeven?

**Example (Landing Page):**
- Development cost: ~$50K
- Revenue at lower bound (+0.53pp): $212K
- ROI: 324% in first year ✓

Both experiments clear both bars:
1. **Statistically significant** (p < 0.05, CIs don't cross zero)
2. **Business significant** (lower bound exceeds breakeven)

When these align, the decision is straightforward. When they diverge (e.g., p = 0.049 but effect size is tiny), focus on business significance.

---

## Power Analysis & Underpowered Studies

### Pre-Test Power Calculation (Smart Onboarding)

We wanted:
- MDE (Minimum Detectable Effect): 3pp lift in D7 retention
- Power: 80% (willing to accept 20% Type II error rate)
- α: 0.05 (Type I error rate)

**Required sample size:**
```
n = 2 * ((z_α/2 + z_β) / effect_size)²
  = 2 * ((1.96 + 0.84) / 0.03)²
  ≈ 4,108 per arm
```

We enrolled 3,750 per arm (92% of target). **Actual power: ~76%.**

### Why Underpowering Matters

Studies with less than 80% power have two risks:
1. **False negatives:** Real effects go undetected (happens in 24% of tests with 76% power)
2. **Winner's curse:** Effects that are detected tend to be larger than the true effect (inflated estimates)

### How We Managed This Risk

1. **Reported the CI lower bound** (+1.91pp, not the point estimate of +4.11pp) for business decisions
2. **Pre-specified the MDE** and communicated the power trade-off
3. **Validated with guardrails** (session length, screens viewed both improved)
4. **Significance survived correction** (p = 0.00026, far below α = 0.05)

The higher the p-value, the more you should worry about winner's curse. With p = 0.00026, the result is robust even accounting for selection effects in underpowered tests.

---

## Effect Size & Practical Significance

### What is Effect Size?

A standardized measure of how large the treatment effect is, independent of sample size.

**For proportions, we use:**
- Absolute difference (pp): Easy to interpret directly
- Relative lift: Percentage change vs. baseline

### Landing Page

- Absolute: +1.31pp (easy to interpret)
- Relative: +1.31 / 3.00 = +43.7% lift
- Effect size (Cohen's h): 0.154 (small by Cohen's standards, but business-meaningful)

### Smart Onboarding

- Absolute: +4.11pp (easy to interpret)
- Relative: +4.11 / 36.54 = +11.3% lift
- Effect size (Cohen's h): 0.167 (small by Cohen's standards, but $1.64M/year in revenue)

**Why small effect sizes matter:**
Cohen's conventions (0.2 = small, 0.5 = medium) are guidelines for psychology. In business, a 1pp lift on a 3% baseline is valuable. Don't rely solely on effect size classifications; tie them to business metrics.

---

## Assumptions & When They Break

### Two-Proportion Z-Test Assumptions

1. **Independent samples:** ✓ (randomization ensures independence)
2. **Random sampling:** ✓ (randomization is random)
3. **Adequate sample size:** ✓ (n > 3,000 per arm, expected frequencies > 5)
4. **Binary outcome:** ✓ (Converted: Yes/No)

### When Assumptions Break

- **Dependent samples:** Users could see both treatment and control (violated if randomization failed). Solution: Check sample ratio test.
- **Inadequate sample:** n < 100 per arm or expected frequencies < 5. Solution: Use Fisher's exact test.
- **Non-binary outcome:** "Partial credit" for engagement. Solution: Use ordinal regression or continuous metrics.

None of these applied to our experiments.

---

## Types of Error & Trade-Offs

### Type I Error (False Positive)

**Definition:** Conclude there's an effect when there isn't one.  
**Probability:** α (typically 0.05)  
**Consequence:** Ship a feature that doesn't actually work.  
**Control:** Require p < 0.05 before shipping.

### Type II Error (False Negative)

**Definition:** Conclude there's no effect when there is one.  
**Probability:** β (typically 0.20 for 80% power)  
**Consequence:** Reject a winning feature.  
**Control:** Invest in adequate sample size (80% power target).

### The Trade-Off

- Smaller α (e.g., 0.01) → harder to ship (fewer false positives) but more false negatives
- Larger α (e.g., 0.10) → easier to ship but more false positives

We used α = 0.05, the industry standard. It balances both errors reasonably.

---

## Why Sample Ratio Matters

The χ² sample ratio test validates that randomization worked.

**Null Hypothesis:** Ratio of treatment to control = 0.5

**Expected:**
- Landing Page: 50.0% treatment / 50.0% control
- Smart Onboarding: 50.0% treatment / 50.0% control

**Observed:**
- Landing Page: 49.8% / 50.2%, χ² p = 0.87 ✓
- Smart Onboarding: 50.5% / 49.5%, χ² p = 0.41 ✓

Both passed. This confirms:
- No algorithmic bias in assignment
- No systematic dropoff in one arm
- No assignment drift over time

If sample ratio test failed (p < 0.05), we'd be suspicious of the entire experiment.

---

## Segmentation & Heterogeneous Treatment Effects

### Why Segment?

To detect whether treatment effects vary across user groups. This informs targeting and follow-up experiments.

### Example (Smart Onboarding)

We tested D7 retention separately for iOS and Android:
- iOS: +3.9pp
- Android: +4.4pp
- Difference: 0.5pp

**Is this difference real or random?**

We use Bonferroni correction: with two segments tested, we require p < 0.05/2 = 0.025 to claim significance.

Both iOS and Android p-values were < 0.0071 (even more stringent), so the effects are real and consistent across devices. No platform-specific strategy needed.

### When NOT to Segment

- Sample size < 1,000 per segment (power collapses)
- You don't have a prior hypothesis (fishing for significance)
- You're testing more than ~5 segments (Bonferroni becomes too strict)

We stayed within these guardrails.

---

## Holdouts & Holdback Testing

### Why Holdbacks Matter

D7 retention is an early proxy for long-term value, but early proxies can decay:
- Novelty effects wear off
- Seasonal patterns change
- New competitors enter the market

A holdback (control arm that persists post-launch) measures true long-term impact.

### Implementation (Smart Onboarding)

Post-launch:
- 95% of new users: Shipped variant (Smart Onboarding)
- 5% of new users: Control (old onboarding)

Monitor D7 retention of the 5% holdback weekly. If it drops below 39% (indicating the +4.11pp lift is decaying), pause the feature and investigate.

### Duration

Minimum 30 days (three cohorts × D7 lookback window). Ideally 90 days to capture seasonal effects.

---

## Practical Recommendations for Future Experiments

### Before Running

1. **Set the MDE and power level** (80% power is standard; document trade-offs if lower)
2. **Calculate sample size** (use an online calculator; link in README)
3. **Pre-specify metrics** (primary, guardrails, segments)
4. **Define success threshold** (e.g., p < 0.05 AND lower bound > breakeven)
5. **Set test duration** (fixed, not peeking at results mid-test)

### During Running

1. **Randomize properly** (assign at user/session level, not by date or user ID patterns)
2. **Monitor sample ratio daily** (early warning of assignment bugs)
3. **Don't peek at results** (increases false positive rate; wait for full test window)
4. **Log everything** (timestamps, tracking IDs, data quality flags)

### After Running

1. **Validate data quality** (balance checks, sample ratio test, tracking audits)
2. **Report p-values AND CIs** (never p-values alone)
3. **Report guardrails** (confirm you didn't break anything)
4. **Segment responsibly** (Bonferroni correction or pre-specified hypotheses only)
5. **Translate to business terms** (revenue impact, not just p-values)
6. **Set holdback duration** (minimum 30 days post-launch)

---

## Further Reading

### Theory
- Kohavi, R., Deng, A., & Frasca, B. (2020). Trustworthy Online Controlled Experiments: A Practical Guide to A/B Testing.
- Wasserman, L. (2004). All of Statistics (Chapter on hypothesis testing).

### Tools & Calculators
- Evan Miller's A/B test calculator: https://www.evanmiller.org/ab-testing/sample-size.html
- Z-test calculator: Verify any statistical claim with an independent tool

### Pitfalls to Avoid
- Simpson's Paradox: Aggregated and segmented results can contradict (always check both)
- Multiple Comparison Problem: Test many hypotheses, expect false positives
- P-hacking: Flexible data analysis inflates false positive rate
- Stopping Rule: Peeking at results and stopping early biases toward false positives
