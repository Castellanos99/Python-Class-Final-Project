# Python-Class-Final-Project
Do better-than-expected earnings lead to higher stock returns?

**Author:** Sofía Castellanos · BAN 4550, Fall 2026 · Industry: SIC 58 Eating & Drinking Places · Comparison industry: SIC 28 Chemicals/Pharma

## The question
McDonald's wants to know whether a quarter that beats last year's earnings is followed by a higher stock return around the announcement, and whether restaurant stocks react the way pharma stocks do.

## Main result
In both industries, announcements with the best earnings news (top third by SUE) had higher stock returns than those with the worst news (bottom third). The gap in average 3-day CAR was +3.0 percentage points for restaurants (445 announcements, 34 companies) and +2.8 points for pharma (2,574 announcements, 219 companies). The restaurant intervals are about twice as wide, because there are fewer observations.

- SUE = (EPS this quarter - EPS four quarters ago) / stock price one quarter ago.
- CAR = the stock's return minus the market's return, added over the day before, the day of and the day after the announcement. The five largest |CAR| events in each industry were removed, and CAR is winsorized at 1% and 99%.

![Figure 1] 
<img width="1632" height="920" alt="image" src="https://github.com/user-attachments/assets/d32db538-88ea-4092-a2bf-e0e66fded37e" />


## Is the difference real?
**t-test (High SUE group minus Low SUE group):**

| Industry | Difference | t | p-value |
| --- | --- | --- | --- |
| Restaurants (SIC 58) | +3.02 pp | 3.71 | 0.0003 |
| Chemicals/Pharma (SIC 28) | +2.85 pp | 6.47 | < 0.0001 |

For both Industries, firms with higher earnings surprises (High) significantly outperform those with lower surprises (Low). 

**Regression, CAR = a + b x SUE (winsorized CAR):**

| Industry | b | p-value of b | R² |
| --- | --- | --- | --- |
| Restaurants (SIC 58) | 0.077 | 0.20 | 0.004 |
| Chemicals/Pharma (SIC 28) | 0.095 | 0.0001 | 0.006 |

b is how much CAR changes when SUE rises by 1. A rise of 0.01 in SUE goes with about +0.08 points of CAR in restaurants and +0.09 points in pharma. For restaurants, the slope cannot be told apart from zero. In both industries SUE explains less than 1% of the differences in CAR. Adding year fixed effects barely changes b (0.073 for restaurants, 0.093 for pharma).

![Figure 2]
<img width="1620" height="930" alt="image" src="https://github.com/user-attachments/assets/53bdee7a-64aa-4ffd-8d7c-8f1676c37e3b" />


## Why the industries differ
Sample size: restaurants have 445 announcements against 2,574 for pharma, so a few extreme returns can tilt the restaurant line and the confidence intervals are about twice as wide. 


![Figure 3] 
<img width="1724" height="980" alt="image" src="https://github.com/user-attachments/assets/8ac4104c-4a0d-4558-8e25-9077e1438564" />

I also think the industries differ because they face different forces that I could not test with my data: raw-material costs, the pandemic (good for pharma, bad for food chains) and seasonality. We should compare with an Industry more similar to Food. 

## Advice for the client
- **Business implication:** McDonald's should not read pharma results as its own, because the two industries can react differently. It should benchmark itself against food-related industries. Good earnings news is still rewarded in both industries (about +3 points between the best and worst third).
- **Limitation:** the restaurant sample is small (445 announcements, 34 firms) and returns swing widely, so slopes are noisy for Rest. Other variables (raw materials, pandemic, seasonality) are not in my data. 
- **Next questions:**
  - *Predictive:* Can last quarter's SUE predict the next announcement's CAR? 

  - *Prescriptive:* Should McDonald’s announce its earnings before the market opens or after it closes in order to reduce reaction volatility?


## Data
- Sources: Compustat (`comp.fundq`), CRSP (`crsp.dsf`, `crsp.dsi`), accessed through WRDS, plus the CRSP-Compustat link file (`ccm_link.csv`).
- **The data are not included in this repository** because of the WRDS license. Someone with WRDS access can rebuild the results by running the notebook with the link file in the same folder.
- Sample period and rules: announcements between 2021-01-01 and 2024-12-31 (earlier quarters are pulled only to compute lags); the announcement date (`rdq`) is known; SUE can be computed; last quarter's price is at least $5; |SUE| is at most 1; all three days (-1, 0, +1) have a return. Day 0 is the first trading day on or after the announcement date.

## How the tables fit together
![Merge map]

Compustat quarters are matched to CRSP with `gvkey` and the link file, keeping only the link whose dates cover the quarter end (`linkdt <= datadate <= linkenddt`), because one company can have different `permno` values over time. Lagged copies of the quarter table give EPS four quarters ago and the price one quarter ago (key: `gvkey` + quarter number). Daily stock returns are matched to daily market returns on `date`, and the three-day window around each announcement is matched back on `permno` + trading-day number.

## How I checked the results
I rebuilt one announcement for McDonald's, quarter ended 2024-09-30, announced 2024-10-29. Compustat shows EPS of 3.15 against 3.19 a year earlier and a prior-quarter price of 254.84, which gives SUE = -0.000157, the same as the notebook. 

## Changes since Project 2
| What was wrong | How I fixed it | How the results changed |
| --- | --- | --- |
| Return window ended 2024-12-31 and started a year too early | Set the return window to 2020-12-01 through 2025-01-31 so day +1 exists for late announcements | No change in the 450 events or in the group averages |
| Use the wrong table for regression | Change the table used for all the regression and analysis so that we don't count the top 5 CAR events (bias results) | Now events change for SIC 58 = 445 (it used to be 450


## Where the code came from
The structure comes from the class notebook & Gemini AI. I changed the keys, the rules and the checks, and I wrote the explanations myself with help of Gemini ai 

| AI tool | Link (or PDF in docs/) | What I used it for |
| --- | --- | --- |
| Gemini | https://share.gemini.google/iImVgoydfAn6 | Project 2 notebook adjustments and counting companies per SUE group |
| Gemini | https://share.gemini.google/ov0uR35mah2I | Project 2: asking about the McDonald's announcement on a specific date |
| Gemini |  https://share.gemini.google/asb5MXd2E9i6 | Project 3: asking some changes in regression graph and understand class Industry. 

## Files
```
notebooks/   Castellanos_Sofia_Project3..ipynb (runs top to bottom on WRDS JupyterHub)
figures/     figure1.png, figure2.png, figure3.png, merge_table
docs/        Castellanos_Sofia_Project3.pdf (slides)
README.md
```
