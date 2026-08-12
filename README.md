# A/B Testing and Product Experimentation

Two product experiments showing how I would use data to decide whether a product change is worth shipping.

Product teams do not need another conversion chart. They need to know whether a change actually improved the customer outcome, how certain the result is, and whether the size of the improvement is worth acting on.

Both datasets are synthetic. The numbers are examples used to show the analysis process, not results from a real company.

| Case study | Question | Result |
|---|---|---|
| [Landing Page Redesign](landing-page/) | Does the new page convert more visitors? | **+1.35pp conversion lift** |
| [Smart Onboarding Assistant](smart-onboarding/) | Does guided onboarding improve retention? | **+4.11pp D7 retention lift** |

## 01. Landing Page Redesign

**Decision:** Should the new landing page replace the old one?

The test covered **8,200 visitors over 42 days**.

Conversion moved from **3.08% to 4.44%**, a **1.35 percentage-point lift** and **43.9% relative improvement**.

The p-value was **0.0013**, with a 95% confidence interval of **+0.53pp to +2.18pp**.

Bounce rate also fell and time on page increased.

**What I would do:** the evidence supports rolling out the redesign for the tested traffic, while continuing to watch the result after release.

[Read the full landing page case study →](landing-page/)

## 02. Smart Onboarding Assistant

**Decision:** Can guided onboarding improve early retention?

The experiment covered **7,500 new mobile users**.

D7 retention moved from **36.54% to 40.65%**, a **4.11 percentage-point lift** and **11.3% relative improvement**.

The p-value was **0.0003**, with a 95% confidence interval of **+1.91pp to +6.31pp**.

There is a catch. Only **37.8% of treatment users actually used the Assistant**. The next test should therefore look at whether better exposure increases adoption and whether the retention effect holds for more users.

[Read the full onboarding case study →](smart-onboarding/)

## How I make the decision

I don't ship a feature just because the number looks good.

For each experiment I check:

- Was the experiment randomized correctly?
- Is the lift statistically reliable?
- How large could the real effect be?
- Does the result hold across important groups?
- Are supporting engagement measures moving in the same direction?
- Is the effect large enough to matter to the business?
- Did the test have enough power?
- Are we analyzing everyone who was assigned to treatment, or only the people who chose to use the feature?

The final question is simple: **what should the product team do next?**

## Main statistical choices

**Conversion and retention:** two-proportion z-tests for the primary binary outcomes.

**Engagement:** Mann-Whitney U tests for skewed session and screen-count data.

**Uncertainty:** confidence intervals are reported alongside p-values.

**Multiple comparisons:** Bonferroni correction was used for segment tests.

**Causal analysis:** intent-to-treat is the main analysis. Adopter results are supporting evidence, not proof that the feature caused the outcome.

**Experiment quality:** power and minimum detectable effect were checked before interpreting the size of the result.

## What this shows

The workflow is:

**Product idea → controlled test → statistical result → business impact → shipping decision**

The aim is not to collect significant p-values. It is to answer practical questions:

- Should we ship it?
- Should we keep testing?
- What should we change next?
- Is the expected upside large enough to justify the work?

## What's included

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

The notebooks contain the analysis from data checks through statistical testing and interpretation.

## Important limitations

- The datasets are synthetic.
- Financial impact estimates are scenarios, not observed company revenue.
- The onboarding test ran below its target statistical power, so the confidence interval matters when judging the size of the opportunity.
- Segment findings are exploratory unless they were independently powered.
- D7 retention and a 42-day conversion test are early indicators. A real rollout should include longer-term monitoring or a holdout group.

The standard I use is simple: test the idea, measure the uncertainty, understand what the result means for the business, and then decide what to do next.
