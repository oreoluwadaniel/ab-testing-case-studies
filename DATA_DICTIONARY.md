# A/B Testing & Product Experimentation Intelligence Platform
## Data Dictionary

> This dictionary documents the datasets used in the two experimentation case studies. The project uses synthetic data for portfolio demonstration.

---

# 1. Dataset Overview

| Experiment | Dataset | Grain | Purpose |
|---|---|---|---|
| Landing Page Redesign | `landing_page_ab_test_dataset.csv` | One row per visitor | Evaluate conversion impact of a redesigned landing page |
| Landing Page Redesign | `landing_page_visitor_level.csv` | One row per visitor | Visitor-level analytical/derived dataset used for deeper analysis |
| Landing Page Redesign | `landing_page_test_summary.csv` | Aggregated experiment summary | Compact experiment-level results for reporting |
| Smart Onboarding | `mobile_app_ab_test_dataset.csv` | One row per user | Evaluate the effect of a Smart Onboarding Assistant on Day-7 retention |

---

# 2. Landing Page A/B Test

## `landing_page_ab_test_dataset.csv`

### Grain

One record represents one experimental visitor.

### Experiment context

- **Experiment:** Landing Page Redesign
- **Duration:** 42 days
- **Experimental groups:** Control and Treatment
- **Primary outcome:** Conversion
- **Data type:** Synthetic

| Field | Definition | Expected Type | Analytical Role |
|---|---|---|---|
| `visitor_id` | Unique identifier for the experiment visitor | String / ID | Primary identifier |
| `date` | Date on which the visitor participated in the experiment | Date | Time analysis |
| `group` | Randomized experiment assignment | Categorical | Control/Treatment comparison |
| `device` | Device category used by the visitor | Categorical | Subgroup analysis |
| `traffic_source` | Acquisition/source channel associated with the visitor | Categorical | Subgroup analysis |
| `converted` | Whether the visitor completed the target conversion | Binary | Primary outcome |

### Valid values

#### `group`

```text
Control
Treatment
```

#### `device`

The dataset contains:

```text
Desktop
Mobile
Tablet
```

#### `traffic_source`

The dataset contains:

```text
organic_search
paid_search
direct
referral
social_media
```

#### `converted`

```text
0 = Did not convert
1 = Converted
```

---

# 3. Landing Page Derived Dataset

## `landing_page_visitor_level.csv`

### Purpose

This dataset represents the visitor-level analytical layer used for additional analysis and reporting.

It should be treated as a **derived analytical artifact**, not as an independent source of truth.

| Field | Definition | Expected Type | Analytical Role |
|---|---|---|---|
| `visitor_id` | Unique visitor identifier | String / ID | Record linkage |
| `date` | Experiment participation date | Date | Time analysis |
| `group` | Control or Treatment assignment | Categorical | Experiment comparison |
| `device` | Visitor device category | Categorical | Segmentation |
| `traffic_source` | Visitor acquisition source | Categorical | Segmentation |
| `converted` | Conversion outcome | Binary | Primary KPI |

> If the actual file contains additional columns, document each additional field here rather than assuming it belongs to the source dataset.

---

# 4. Landing Page Experiment Summary

## `landing_page_test_summary.csv`

### Purpose

Aggregated experiment-level results used to support reporting and executive interpretation.

Typical analytical fields include:

| Field | Definition |
|---|---|
| `group` | Experiment group |
| `visitors` | Number of visitors in the group |
| `conversions` | Number of conversions |
| `conversion_rate` | Conversions divided by visitors |
| `absolute_lift` | Treatment conversion rate minus control conversion rate |
| `relative_lift` | Absolute lift relative to the control conversion rate |
| `p_value` | Statistical test p-value |
| `confidence_interval` | Estimated uncertainty around the treatment effect |

> Exact column names should match the final CSV. The summary file should remain a derived output rather than a manually maintained source.

---

# 5. Smart Onboarding A/B Test

## `mobile_app_ab_test_dataset.csv`

### Grain

One record represents one user participating in the onboarding experiment.

### Experiment context

- **Experiment:** Smart Onboarding Assistant
- **Primary metric:** Day-7 retention
- **Experimental groups:** Control and Treatment
- **Data type:** Synthetic

| Field | Definition | Expected Type | Analytical Role |
|---|---|---|---|
| `user_id` | Unique identifier for the experiment user | String / ID | Primary identifier |
| `group` | Randomized experiment assignment | Categorical | Control/Treatment comparison |
| `retained_d7` | Whether the user returned on Day 7 | Binary | Primary outcome |
| `adopted` | Whether the user adopted/used the onboarding assistant | Binary | Adoption analysis |
| `session_length` | User engagement/session-duration measure | Numeric | Secondary outcome |
| `screens_viewed` | Number of screens viewed | Numeric | Secondary engagement outcome |

> Use the exact field names present in the CSV if they differ from the names above.

---

# 6. Experiment Dimensions

## Experiment Group

The treatment assignment defines the experimental comparison.

```text
Control
    ↓
Existing experience

Treatment
    ↓
New experience
```

The primary causal comparison is:

> Treatment outcome − Control outcome

---

# 7. Primary Metrics

## Landing Page: Conversion Rate

```text
Conversion Rate =
Conversions / Eligible Visitors
```

Used to evaluate whether the redesigned landing page changes the probability of conversion.

---

## Smart Onboarding: Day-7 Retention

```text
D7 Retention =
Users retained on Day 7 / Eligible users
```

Used to evaluate whether assignment to the Smart Onboarding experience affects early retention.

---

# 8. Effect Metrics

## Absolute Lift

For a binary outcome:

```text
Absolute Lift =
Treatment Rate − Control Rate
```

Interpretation:

> Difference in percentage points between treatment and control.

---

## Relative Lift

```text
Relative Lift =
(Treatment Rate − Control Rate) / Control Rate
```

Interpretation:

> Percentage improvement relative to the control group's baseline.

---

# 9. Statistical Fields

Where calculated, the following fields should be interpreted as follows:

| Metric | Meaning |
|---|---|
| `p_value` | Probability of observing an effect at least this extreme under the null hypothesis |
| `confidence_interval` | Range of plausible values for the estimated treatment effect |
| `effect_size` | Magnitude of the observed treatment effect |
| `power` | Probability of detecting an effect of a specified size under the assumed conditions |
| `sample_ratio_mismatch` | Test of whether observed allocation differs materially from the intended allocation |

---

# 10. Data Quality Expectations

Before analysis, the datasets should satisfy:

### Uniqueness

- `visitor_id` should uniquely identify visitors.
- `user_id` should uniquely identify users.

### Completeness

Required experimental fields should not contain unexpected missing values.

### Validity

Categorical fields should contain only documented values.

### Binary consistency

Outcome fields should use a consistent binary representation.

### Temporal validity

Landing-page dates should fall within the documented **42-day experiment window**.

### Assignment integrity

Each participant should have one experiment assignment.

---

# 11. Business Interpretation

The datasets support analysis of:

### Landing Page

- Conversion rate
- Treatment lift
- Device performance
- Traffic-source performance
- Daily experiment trends
- Incremental conversion scenarios

### Smart Onboarding

- Day-7 retention
- Treatment lift
- Engagement behavior
- Adoption patterns
- Statistical significance
- Power and sample-size considerations

---

# 12. Important Analytical Distinction

### Intent-to-Treat

The treatment-vs-control comparison estimates the effect of **being assigned to the treatment** when the experiment is properly randomized.

### Adopter Analysis

Comparing users who adopted the Smart Onboarding Assistant with users who did not adopt it is observational.

Therefore:

> Higher retention among adopters does not, by itself, prove that adoption caused the higher retention.

This distinction is intentionally preserved throughout the project.

---

# 13. Data Status

All datasets in this repository are **synthetic portfolio datasets**.

They are designed to demonstrate:

- Experiment design
- Statistical analysis
- Data validation
- Causal reasoning
- Business impact modeling
- Executive reporting

They should not be interpreted as real production customer data or actual company performance.

---

# 14. Maintenance Rule

When the underlying CSV schema changes, update this dictionary at the same time.

The dictionary should always reflect:

**Actual file → Actual field name → Actual meaning → Actual analytical use**

Do not document fields, categories, row counts, or business assumptions that are not present in the current data.
