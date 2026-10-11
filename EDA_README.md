# Bank Marketing Campaign – Exploratory Data Analysis

Exploratory analysis of a Portuguese bank's phone marketing campaign (UCI Bank Marketing dataset).

**Goal:** find which client and campaign characteristics are linked to subscribing to a term deposit, and use that to recommend who the bank should call.

This notebook picks up where the cleaning notebook (`bank-marketing-campaigns-data-cleaning.ipynb`) left off and uses its output, `cleaned_bank_marketing.csv`.

---

## Data

- **Input:** `cleaned_bank_marketing.csv` (41,188 clients)
- **Target:** `subscribed` (yes / no)
- **Columns analyzed:** job, marital status, education, age, housing loan, personal loan, month contacted, previous campaign outcome
- The five macroeconomic indicator columns were removed during cleaning and are not part of this analysis.

## Tools

Python, pandas, matplotlib, seaborn, Jupyter Notebook

---

## Approach

**1. Set a baseline.**
Only **11.3%** of all contacted clients subscribed. Every group in the notebook is judged by whether it beats or falls short of this number.

**2. Compare rates, not counts.**
Raw counts are misleading because the bank called some groups far more than others. For example, admin had the most subscribers only because it was called the most. Its rate (13.0%) is close to the baseline. To compare groups fairly, I wrote a helper function, `subscribing_by()`. For any column, it returns each group's total contacts, subscribers, subscription rate, and difference from the baseline.

**3. Check group size.**
Groups with very few people (for example, "illiterate" with 18 clients) are flagged and not used to draw conclusions.

**4. Hold one thing constant.**
When two characteristics overlap (students are young, retirees are old), I looked at one characteristic *inside* a single group of the other. This shows which one is actually driving the result. Examples:
- jobs within the same age group
- months among clients never contacted before
- age among single clients only

---

## Key Findings

| Finding | Rate | vs. 11.3% baseline |
|---|---|---|
| Clients who said yes in a previous campaign | **65.1%** | ~6x |
| Age 60+ | **39.6%** | ~3.5x |
| Students | **31.4%** | ~3x |
| May (busiest month, 13,769 calls) | **6.4%** | below |

**Previous successes are the best leads.**
Clients who subscribed in an earlier campaign converted at 65.1%, but they made up only 1,373 of 41,188 calls.

**Age 60+ converts at 39.6%, and it's age, not retirement.**
Among people 60+, admins (41.5%) subscribed almost as often as retirees (42.5%). This group received only 1,193 calls.

**Subscription by age is U-shaped.**
- 17–29: 16.3%
- 40–49: 7.9% (the lowest)
- 60+: 39.6%

The average ages of subscribers and non-subscribers are nearly identical (41 vs. 40), because the high young and old ends cancel each other out.

**Students convert well on their own merit.**
Among 17–29 year olds, students subscribed at 33.4%, compared with 18.1% for admins the same age.

**The busiest months converted worst.**
May had the most calls (13,769) and the lowest rate (6.4%). March, December, September, and October all converted above 40%.
- Part of the reason is the call list: those months had more previous successes. In September, 26.0% of calls were previous successes, compared with 1.7% in May.
- But the strong months stayed strong even among clients never contacted before (March 47.4% vs. May 5.5%). So the month matters on its own too.

**Weak signals.**
- **Loans:** housing and personal loan status stayed within 0.5 points of the baseline.
- **Marital status:** single clients subscribed only 1–3 points more than others the same age.
- **Education:** it largely overlaps with job. Almost half of university-degree clients are admins.

---

## Recommendations

1. **Call previous successes first.** They are the highest-converting group by far.
2. **Call more clients aged 60+.** They convert at 3.5x the baseline but received under 3% of calls.
3. **Call more students.**
4. **Shift calls away from the 30–49 age range.** It received about two-thirds of all calls while converting at or below the baseline.
5. **Don't use loan status or marital status for targeting.**

## Limitations

- **This analysis shows associations, not causes.**
- **The month effect isn't explained.** The economic indicator columns were removed during cleaning, and conditions that change month to month may be part of the reason some months performed better.
- **Some subgroups are small.** For example, December has 98 cold-lead calls, and housemaids aged 60+ number 67. Treat those results with caution.

---

## Output

The notebook saves `EDA_bank_marketing.csv`, which adds two columns for visualization:
- `age_group`
- `subscribed_flag` (1 = yes, 0 = no)

This file is the input for the Tableau dashboard (in progress).

## How to Run

1. Put `cleaned_bank_marketing.csv` in the same folder as the notebook.
2. Install the dependencies: `pip install pandas matplotlib seaborn`
3. Open `eda_bank_marketing4.ipynb` and run **Kernel → Restart & Run All**.
