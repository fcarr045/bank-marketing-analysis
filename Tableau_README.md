# Bank Marketing Campaign – Tableau Dashboard
 
**🔗 [View the interactive dashboard on Tableau Public](https://public.tableau.com/app/profile/fayed.carraha/viz/BankMarketingCampaignWhoandWhentoCall/Story1)**
 
This is the final stage of the project. It turns the findings from the exploratory analysis (`eda_bank_marketing.ipynb`) into two dashboards and a click-through Story that answer one question:
 
> **Who should the bank call, and when?**
 
---
 
## Data
 
- **Source file:** `EDA_bank_marketing.csv`, the output of the EDA notebook (41,188 calls)
- **Key field:** `subscribed_flag`, where 1 means the client subscribed and 0 means they didn't. The notebook added this column so Tableau can calculate subscription rates.
---
 
## Dashboard 1: Who should the bank call?
 
Each bar shows a group's **subscription rate**. The gray line marks the **11.3% baseline**, which is the rate across all calls. Blue bars beat the baseline and red bars fall below it. Each label shows the rate and the group size (`n`), so small groups are easy to spot.
 
**What it shows:**
- **Previous outcome is the strongest signal.** Clients who said yes in an earlier campaign subscribed at **65.1%**, nearly 6x the baseline, but they were only 1,373 of 41,188 calls. Even clients who said *no* before (14.2%) beat clients never contacted (8.8%).
- **Age is U-shaped.** Clients 60+ subscribed at **39.6%**, but received under 3% of calls. Clients aged 40–49 were the lowest, at 7.9%.
- **Students lead the jobs** at **31.4%**. Blue-collar workers got the second-most calls (9,254) but converted at only 6.9%.
## Dashboard 2: When should the bank call?
 
The bars show **how many calls** were made each month (left axis). The orange line shows the **subscription rate** for that month (right axis).
 
**What it shows:**
- **The busiest month converted worst.** May had the most calls (13,769) and the lowest rate (6.4%). March, September, October, and December each had few calls but converted above 40%.
- **The filter lets you test it.** Switch *Previous Campaign Outcome* to **never contacted**, and the strong months stay strong even with brand-new leads (March 47.4% vs. May 5.5%). So timing matters on its own, not just who was on the call list.
## The Story
 
The Tableau Story walks through the findings in five steps:
 
1. Past subscribers: 65.1%
2. Age 60+: 39.6%
3. Students: 31.4%
4. Busiest month, worst rate
5. Timing still matters for cold leads (same chart, filtered to never-contacted clients)
---
 
## How It Was Built
 
**Calculated fields**
 
| Field | Formula | Purpose |
|---|---|---|
| Subscription Rate | `AVG([subscribed_flag])` | The average of 1s and 0s is the share of clients who said yes |
| Contacts | `COUNT([subscribed_flag])` | Group size, shown as `n` on each bar |
| Baseline | `{FIXED : AVG([subscribed_flag])}` | The overall 11.3% rate. FIXED keeps it the same on every chart |
| Vs Baseline | `[Subscription Rate] - AVG([Baseline])` | Distance above or below the baseline, used for the bar colors |
| Month Number | `CASE [month] WHEN 'march' THEN 3 ... END` | Sorts months in calendar order instead of alphabetically |
 
**Design choices**
- **Rates, not counts.** Counts reward whoever was called most. For example, admin had the most subscribers only because it got the most calls.
- **The baseline line is a calculated field, not Tableau's built-in "Average" line.** The built-in line would average the bars equally, giving a group of 330 people the same weight as a group of 10,422.
- **Group size on every bar**, so results from small groups aren't over-trusted.
- **One axis scale (0–90%) across all charts**, so bar lengths can be compared from chart to chart.
- **Diverging colors centered on the baseline**, so above and below are visible at a glance.
- **A dual-axis month chart**, so call volume and conversion rate appear together.
---
 
## Recommendations
 
1. **Call previous successes first.** They're the highest-converting group by far.
2. **Call more clients aged 60+ and more students.**
3. **Shift calls away from the 30–49 age range.** These clients received about two-thirds of all calls while converting below the baseline.
4. **Spread calls beyond the peak month.** May's high volume came with the lowest rate.
## Limitations
 
- These are associations, not proof of cause.
- The month effect can't be fully explained here. The economic indicator columns were removed during cleaning, and conditions that change from month to month may be part of the reason.
---
 
**Tools:** Tableau Public · Python (pandas) for data prep
 
**Related files in this repo:**
- `README.md`: data cleaning
- `README_EDA.md`: exploratory analysis
- `eda_bank_marketing.ipynb`: the analysis notebook
