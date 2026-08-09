# Smart Onboarding Assistant: Mobile App A/B Test

**Test Period:** July 2026 | **Duration:** 21 days | **Status:** Complete & Shipped

---

## Quick Summary

A guided onboarding flow (Smart Onboarding Assistant) increased 7-day retention from 36.54% to 40.65%, a statistically significant lift of +4.11 percentage points (p = 0.00026). This translates to approximately $1.64M/year in retention value. The feature was deployed to 100% of new users with a 5% holdback for 30/90-day confirmation.

**Recommendation:** Ship to 100% with holdback and discoverability follow-up test.

---

## The Business Context

D7 retention had plateaued at ~36% for three quarters, representing a ceiling on new user value. Product analytics suggested the root cause: new users landed on an empty home screen, never set up their first project, and churned before reaching their first useful moment.

The product team built the Smart Onboarding Assistant—a guided flow that walks new users through creating their first project in their first session, positioning them to engage with core features.

Question: Does guided onboarding improve retention? If so, by how much?

---

## Experiment Setup

### Design

| Element | Details |
|---|---|
| **Control (A)** | Standard empty-state onboarding (screen shows empty home) |
| **Treatment (B)** | Smart Onboarding Assistant (guided project creation flow) |
| **Randomization** | 50/50 traffic split at user creation, device-level assignment |
| **Primary Metric** | 7-day retention (user active on day 7 post-install) |
| **Guardrail Metrics** | Session length, screens viewed, retention by engagement level |
| **Sample Size** | 7,500 new users (3,714 control, 3,786 treatment) |
| **Power Analysis** | 80% power target against 3pp MDE; actual power ~76% |
| **Significance Level** | α = 0.05 (two-sided) |
| **Statistical Test** | Two-proportion z-test (intent-to-treat) |
| **Duration** | 21 days (users tracked through day 7) |

### Hypothesis

**H₀ (Null):** The Smart Onboarding Assistant has no effect on D7 retention.  
**H₁ (Alternative):** The Assistant changes D7 retention (two-sided test).

We're testing whether onboarding quality matters for early retention, not assuming it will help.

---

## Results

### Primary Metric: 7-Day Retention

| Group | Users | Retained (D7) | Retention Rate | 95% CI |
|---|---|---|---|---|
| **Control** | 3,714 | 1,357 | 36.54% | [35.02%, 38.09%] |
| **Treatment** | 3,786 | 1,539 | 40.65% | [39.07%, 42.26%] |
| **Difference** | — | +182 | +4.11pp | [+1.91pp, +6.31pp] |

### Statistical Evidence

**Two-Proportion Z-Test (Intent-to-Treat)**
- z-statistic: 3.66
- p-value: **0.00026** (extremely significant)
- Interpretation: The probability of observing this data if there's no true difference is 0.026%. Overwhelming evidence the Assistant improves retention.

**Confidence Interval Interpretation**
We're 95% confident the true lift falls between +1.91pp and +6.31pp. Even the conservative lower bound represents a meaningful and achievable business impact.

**Why Intent-to-Treat?**
We assigned users to treatment but didn't force them to use the Assistant. ITT analysis counts all users in their assigned arm, regardless of whether they engaged. This is the only comparison that answers "what happens if we ship to everyone?"—the actual business question.

### Guardrail Metrics: Ensuring No Negative Trade-Offs

| Metric | Control | Treatment | Test | p-value | Interpretation |
|---|---|---|---|---|---|
| Median Session Length | 420s | 445s | Mann-Whitney U | 0.0050 | Engagement up ✓ |
| Median Screens Viewed | 12 | 14 | Mann-Whitney U | <0.0001 | Exploration up ✓ |
| Mean Bounce Day 1 | 12.1% | 11.3% | χ² | 0.24 | Balanced |

**Verdict:** Both guardrails moved positive. Session length and screens viewed increased together with retention—this is the hallmark of a real effect, not a measurement artifact. Users in treatment weren't just retaining; they were exploring more.

---

## The Adoption Finding: Feature Quality vs. Discoverability

This insight is critical for what comes next:

**Overall ITT Effect:** +4.11pp retention (Treatment assignment → improved retention).

**Adopter Analysis (Correlational):**
- 37.8% of treatment users engaged with the Assistant
- Among adopters: 50.5% retained on D7
- Among non-adopters: 34.7% retained on D7 (statistically the control baseline)

**Implication:** The entire +4.11pp lift came from the ~38% of users who discovered and used the feature. The other 62% saw no onboarding difference (they ignored it).

**Caveat:** This is correlational. Users who explore new features tend to retain anyway. The causal lift (ITT +4.11pp) is the honest number. But the pattern tells us something crucial: the feature works brilliantly for those who find it, but discoverability is the ceiling. If we could move adoption from 38% to 60%, we'd potentially lift retention further.

**Next Move:** Run a discoverability experiment testing onboarding trigger placement, timing, and messaging (target: 60% adoption). This is higher-leverage than building a new feature.

---

## Business Impact

### Revenue Calculation

Finance values each point of D7 retention at $400K/year (based on cohort LTV). This reflects downstream spending, engagement, and lifetime value correlation.

**Scenario Analysis:**

| Scenario | Lift | New Users/Year* | Added Retained Users | Annual Revenue Impact |
|---|---|---|---|---|
| **Conservative** | +1.91pp (CI low) | 2.5M | 47,750 | ~$764K |
| **Point Estimate** | +4.11pp | 2.5M | 102,750 | ~$1.64M |
| **Upper Bound** | +6.31pp (CI high) | 2.5M | 157,750 | ~$2.52M |

*Assumes 2.5M new users/year (industry typical for mobile apps in growth phase).

### Breakeven Analysis

- Smart Onboarding development cost: ~2 FTEs × $200K/year = $400K/year maintenance
- Breakeven lift: $400K / $400K per point = **1.0pp**
- Conservative CI lower bound: +1.91pp (clears breakeven by 91%)
- Point estimate: +4.11pp (10x+ breakeven)

**Conclusion:** Even the worst plausible outcome (CI lower bound) comfortably clears breakeven. The business case is robust.

---

## Segment Analysis

### By Device Type

| Device | Control D7 Ret. | Treatment D7 Ret. | Lift | p-value (Bonf. α=0.0071) | N |
|---|---|---|---|---|---|
| iOS | 37.2% | 41.1% | +3.9pp | 0.0043 ✓ | 3,821 |
| Android | 35.6% | 40.0% | +4.4pp | 0.0031 ✓ | 3,679 |

**Insight:** Lift is consistent across platforms. No platform-specific strategy needed.

### By App Version (Exploratory)

| Version | Control D7 Ret. | Treatment D7 Ret. | Lift | N | Note |
|---|---|---|---|---|---|
| v4.0.1 | 36.8% | 40.3% | +3.5pp | 2,104 | Stable |
| v3.3.0 | 35.1% | 46.4% | +11.3pp | 1,247 | Outlier |
| v3.2.1 | 36.9% | 40.9% | +4.0pp | 1,891 | Stable |
| v3.1.0 | 36.2% | 40.8% | +4.6pp | 1,258 | Stable |

**Alert:** v3.3.0 shows +11.3pp, significantly above others. Bonferroni-corrected threshold (α=0.0071) means this survives multiple testing correction. However, no product story explains why v3.3.0 would respond differently. **Recommendation:** Log this finding but don't action it. Flag for targeted replication in future tests.

---

## Data Quality Checks

### Sample Ratio Test
- Control: 3,714 users
- Treatment: 3,786 users
- Ratio: 49.5% vs 50.5% (expected: 50/50)
- χ² p-value: **0.41**
- Verdict: Random assignment succeeded; no bias.

### Balance Check: Device Distribution
- iOS: 51.0% control vs 51.5% treatment (χ² p = 0.76)
- Android: 49.0% control vs 48.5% treatment (χ² p = 0.76)
- Verdict: Balanced.

### Contamination Check
- 38 Control users (1.0%) logged Smart Onboarding feature usage
- Verdict: Negligible; biases our effect estimate conservative (underestimates true lift)

### Tracking Validation
- User IDs: No duplicates
- Retention flags: Binary (Yes/No), no ambiguous values
- Timestamp plausibility: All events within test window
- Verdict: Data quality is high.

---

## Design Considerations & Transparency

### Power Analysis Trade-Off

Sample size was 7,500 (3,750 per arm). For a 3pp MDE at 80% power:
- Required n: ~4,108 per arm (8,216 total)
- Actual n: ~3,750 per arm
- **Actual power: ~76%**

This shortfall is acceptable because:
1. Results are highly significant (p = 0.00026), so underpowering didn't hurt here
2. Lower bounds of CI still exceed breakeven
3. Guardrails confirm the effect is real

However, it highlights an important principle: don't overconfidently generalize from results with 76% power. Use the CI lower bound for decision-making.

### Intent-to-Treat Rationale

We randomized at assignment but didn't force feature use. Analysis counts all users in their original arm:
- This answers the practical question: "If we ship to everyone, what happens?"
- Avoids selection bias that would arise from comparing only adopters to all controls
- Treatment effect includes both feature quality and discoverability headwinds

The adopter analysis is purely descriptive, not causal.

---

## Key Findings

1. **The feature works.** +4.11pp retention lift (p = 0.00026) is extremely strong evidence. Even the conservative CI lower bound (+1.91pp) exceeds breakeven.

2. **Guardrails move together.** Session length and screens viewed both improved, confirming engagement didn't decay. Single-metric wins are suspect; correlated metrics are reassuring.

3. **Effect is device-agnostic.** iOS (+3.9pp) and Android (+4.4pp) show consistent lifts. No platform-specific strategy needed.

4. **Discoverability is the ceiling.** 62% of treatment users never engaged with the feature. The feature is high-quality (50.5% retention for adopters), but awareness is the bottleneck. Follow-up test on trigger placement/timing is the highest-leverage next move.

5. **D7 is early; monitor long-term.** D7 retention is a strong proxy for LTV, but novelty effects decay. The 5% holdback for 30/90-day measurement is essential.

---

## Recommendation

### Decision: Deploy to 100% of New Users

**Evidence:**
- Statistically significant at p = 0.00026 (not borderline)
- Practically significant: +4.11pp × 2.5M users/year = $1.64M revenue impact
- Consistent across devices (iOS, Android)
- Guardrails positive (engagement improved)
- Lower bound (+1.91pp) exceeds breakeven by 91%

### Conditions

1. **Maintain a 5% holdback** on the old onboarding (control) for 30/90-day measurement
   - D7 is early; confirm the lift persists through day 30 and beyond
   - Watch for novelty decay (most likely concern)
   - If holdback D7 retention drops to <39%, pause and investigate

2. **Run a follow-up discoverability experiment** (Month 2–3)
   - Test: Trigger placement (in-app notification vs. modal vs. auto-triggered)
   - Goal: Move adoption from 38% to 60%
   - Expected outcome: Incremental lift on top of current +4.11pp
   - This is higher-leverage than building new features

---

## Technical Details

### Files in This Directory

- **analysis.ipynb**: Full Python analysis pipeline
  - Data loading and cleaning
  - Descriptive statistics by arm
  - Primary z-test with confidence interval
  - Guardrail metric analysis (Mann-Whitney for non-normal distributions)
  - Segment breakdowns with Bonferroni correction
  - Adopter analysis (correlational, clearly marked)
  - Power post-hoc analysis

- **mobile_app_ab_test_dataset.csv**: Raw user-level data
  - 7,500 rows (one per new user)
  - Columns: User_ID, Group, Session_Length (seconds), Feature_Used (Yes/No), Screens_Viewed, Retained_7_Days (Yes/No), Device, App_Version
  - No PII; synthetic data

---

## How to Reproduce This Analysis

### Setup
```bash
pip install pandas scipy statsmodels matplotlib numpy
jupyter notebook analysis.ipynb
```

### Running the Notebook
1. Load dataset: `mobile_app_ab_test_dataset.csv`
2. Run all cells to generate:
   - Balance checks and sample ratio test
   - Primary metric z-test with 95% CI
   - Guardrail metric Mann-Whitney U tests
   - Segment breakdowns (Bonferroni-corrected)
   - Adopter correlation analysis
   - Power and effect size calculations

### Expected Output
- p-value: 0.00026
- Lift: +4.11pp
- CI: [+1.91pp, +6.31pp]
- Session length Mann-Whitney p: 0.005
- Screens viewed Mann-Whitney p: <0.0001

---

## Limitations

1. **Power: 76% vs. 80% target.** Results are highly significant, so this didn't hurt here, but don't overextrapolate to smaller effects.

2. **D7 is a proxy.** D7 retention correlates with LTV, but we haven't measured 30/90-day outcomes. The holdback addresses this.

3. **Adopter analysis is correlational.** Users who engage with features tend to retain anyway. We can't claim the Assistant caused all retention in the adopter segment—only that ITT +4.11pp is the causal effect.

4. **Seasonal effects.** Test ran in summer; holiday seasonality not captured. Post-launch monitoring is essential.

5. **One app version outlier (v3.3.0: +11.3pp).** Replicates Bonferroni correction but has no product explanation. Flag for follow-up; don't action alone.

6. **1% control contamination.** Negligible; biases our estimate toward zero (conservative).

---

## Reproducibility & Datasets

**Dataset Status:** Synthetic. This data is realistic and built to match true experimental structure (randomized assignment, proper balance, realistic distributions) but is generated for educational and portfolio purposes.

**Use Cases:** Validating analysis methods, building statistical intuition, demonstrating experimentation competency.

**Not:** Proof of a real production deployment or actual company results.

---

## Post-Launch Monitoring

### Week 1–2
- Daily D7 retention tracking (watch for novelty decay)
- Segment monitoring (device, version)
- 5% holdback performance vs. treatment

### Week 2–4
- Average session length by cohort
- Adopter % (feature engagement rate)
- Early signals on D30 retention (if available)

### Month 2–3
- Design and launch discoverability experiment
- Measure 30-day retention holdback results
- Analyze feature engagement patterns (in-session behavior)

---

## Next Reading

- See main README.md for cross-experiment insights and methodology
- See METHODOLOGY.md for deep statistical foundation
- See DATA_DICTIONARY.md for field-by-field dataset reference
