 # Bank Marketing Analysis - cleaning

Cleaning 41,188 direct marketing call records from a Portuguese bank, in Python and pandas.
Stage one of the project — exploratory analysis in progress.

**Data:** [UCI Bank Marketing](https://archive.ics.uci.edu/dataset/222/bank+marketing) · 41,188 rows · 20 columns → 16
**Each row:** one phone call to one client. **Target:** did they subscribe to a term deposit.

---

## What the notebook does

- Scopes the data to client- and campaign-level columns
- Renames 10 columns so they read without the data dictionary
- Expands abbreviated category values into full text
- Sets `month` and `day_of_week` as ordered categoricals so they sort chronologically
- Handles a sentinel value in the previous-contact column
- Writes out `cleaned_bank_marketing.csv`

---

## Two decisions worth explaining

**The `999` in `pdays`.** The column records days since a client's last contact in
a previous campaign, and uses `999` to mean *never contacted*. Real values run 0 to
27, so left alone the sentinel would put a fake 960-day average into any analysis.
I replaced it with `NaN` and added a boolean `previously_contacted` column to keep
what it meant. Verified: filtering on that flag returns exactly 39,673 rows, matching
the null count. 1,515 clients — about 4% — had real prior contact.

**Dropping five macroeconomic columns.** They describe the state of the economy when
the call was made, not the client, so everyone contacted in the same period shares
identical values. This project asks about clients. Those columns would matter for a
different question: how campaign performance tracked the 2008–2010 downturn.

---

## Running it

```bash
pip install -r requirements.txt
jupyter notebook bank-marketing-campaigns-data-cleaning.ipynb
```

**Next:** subscription rate by job, education, and month, and whether contact volume
within a campaign relates to success.
