# A/B Testing & Product Experimentation

**Two product experiments showing how data can turn product ideas into better shipping decisions.**

Product teams do not need another dashboard showing conversion rates. They need to know whether a change actually improved the customer outcome, whether the result is reliable, and whether the improvement is large enough to justify rollout.

This portfolio applies that approach to two common product decisions:

| Case Study                                      | Business Question                         | Result                        |
| ----------------------------------------------- | ----------------------------------------- | ----------------------------- |
| [Landing Page Redesign](landing-page/)          | Does the new page convert more visitors?  | **+1.35pp conversion lift**   |
| [Smart Onboarding Assistant](smart-onboarding/) | Does guided onboarding improve retention? | **+4.11pp D7 retention lift** |

Both datasets are synthetic and designed to reflect realistic experimentation conditions. The goal is to demonstrate the decision-making process, not present simulated numbers as real company results.

---

## 01. Landing Page Redesign

**Decision:** Should the new landing page replace the existing experience?

The test covered **8,200 visitors over 42 days**.

The redesigned page increased conversion from **3.08% to 4.44%**, a **1.35 percentage-point lift** and **43.9% relative improvement**.

The result was statistically significant, with a p-value of **0.0013** and a 95% confidence interval of **+0.53pp to +2.18pp**.

Bounce rate also fell and time on page increased, supporting the conversion result with stronger engagement.

**Business takeaway:** The redesign produced a meaningful conversion improvement across the tested traffic. The evidence supports rollout rather than treating the result as a random fluctuation.

[Read the full landing page case study →](landing-page/)

---

## 02. Smart Onboarding Assistant

**Decision:** Can guided onboarding improve early user retention?

The experiment covered **7,500 new mobile users**.

D7 retention increased from **36.54% to 40.65%**, a **4.11 percentage-point lift** and **11.3% relative improvement**.

The result was statistically significant, with a p-value of **0.0003** and a 95% confidence interval of **+1.91pp to +6.31pp**.

Session length and screens viewed also increased, suggesting the retention improvement was accompanied by stronger early engagement.

But there is an important product insight: only **37.8% of treatment users actually used the Assistant**, and the observed lift came from that group.

**Business takeaway:** The feature shows promise, but the next opportunity is discoverability. Before simply scaling the feature, test whether better exposure can increase adoption and extend the retention benefit to more users.

[Read the full onboarding case study →](smart-onboarding/)

---

## How I Make Experiment Decisions

The analysis follows a simple principle:

**Do not ship because the number looks good. Ship when the evidence supports the business decision.**

For each experiment I check:

* Was the experiment properly randomized?
* Is the observed lift statistically reliable?
* How large could the real effect reasonably be?
* Does the result hold across important segments?
* Are engagement metrics moving in the same direction?
* Is the effect large enough to matter commercially?
* Does the test have enough statistical power?
* Are we measuring assigned users or only people who chose to engage?

This keeps the analysis focused on one question:

> **What should the product team do next?**

---

## Key Analytical Decisions

**Conversion and retention:** Two-proportion z-tests were used for the primary binary outcomes.

**Engagement:** Mann-Whitney U tests were used for skewed session and screen-count data.

**Uncertainty:** Every primary result is reported with a confidence interval, not just a p-value.

**Multiple comparisons:** Bonferroni correction was applied to segment testing to reduce false positives.

**Causal analysis:** Intent-to-treat remains the primary analysis. Adopter results are treated as supporting evidence, not proof of causality.

**Experiment quality:** Power and minimum detectable effect were checked before interpreting the business impact.

---

## Business Value

This portfolio demonstrates a practical experimentation workflow:

**Product idea → controlled experiment → statistical evidence → business impact → shipping decision**

The goal is not simply to find statistically significant results.

It is to help a product team answer:

* **Should we ship it?**
* **Should we keep testing?**
* **What should we improve next?**
* **Is the expected business upside large enough to justify the investment?**

---

## What's Included

```text
AB-Testing/
├── README.md
├── METHODOLOGY.md
├── DATA_DICTIONARY.md
│
├── landing-page/
│   ├── README.md
│   ├── analysis.ipynb
│   └── landing_page_ab_dashboard.pbix
│
└── smart-onboarding/
    ├── README.md
    └── analysis.ipynb
```

### Tools

Python, pandas, NumPy, SciPy, statsmodels, matplotlib, Jupyter, Power BI, Git/GitHub.

The notebooks contain the full analysis workflow from data validation through statistical testing and business interpretation.

---

## Important Limitations

* The datasets are synthetic.
* Financial impact estimates are scenario-based, not observed revenue.
* The onboarding experiment ran below its target statistical power, so the confidence interval matters when judging the size of the opportunity.
* Segment findings are treated as exploratory unless independently powered.
* D7 retention and a 42-day conversion test are early indicators. A real rollout should include longer-term monitoring or a holdout group to confirm that the effect persists.

**The standard I am demonstrating here is simple: test the idea, quantify the uncertainty, understand the business impact, then make the decision.**
