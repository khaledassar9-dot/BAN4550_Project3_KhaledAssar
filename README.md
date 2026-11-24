# BAN4550_Project3_KhaledAssar

Do Quarterly Earnings Surprises Move Stock Prices?
Author: Khaled Assar · BAN 4550, Fall 2026 · Industrial Machinery & Computers (SIC 35) · Comparison industry: Miscellaneous Retail (SIC 59)
The question
Apple Inc. (AAPL) wants to know whether beating or missing quarterly earnings expectations actually moves stock prices around announcement day, and how its industry (SIC 35) compares to Miscellaneous Retail (SIC 59).
Main result
High earnings surprises clearly beat low earnings surprises in both industries. In Apple's sector (SIC 35, $n = 1,729$), High-SUE announcements average a $+2.16%$ 3-day abnormal return compared to $-1.74%$ for Low-SUE announcements—a gap of $+3.90$ percentage points. In Miscellaneous Retail (SIC 59, $n = 491$), the High–Low gap is even larger at $+5.41$ percentage points ($+2.62%$ vs. $-2.79%$).
![Figure 1](figures/figure1_car_by_sue_group.png)
Is the difference real?
Yes—the gap between high and low earnings surprises is statistically significant in both industries. In a Welch t-test comparing High-SUE to Low-SUE quarters, the difference is $+3.90$ pp in SIC 35 ($t = 6.9177$, $p < 0.0001$) and $+5.41$ pp in SIC 59 ($t = 3.8360$, $p = 0.0002$).
In simple OLS regressions, every $+1$ pp increase in earnings surprise (SUE) adds $+0.22$ pp to the 3-day CAR in SIC 35 ($b = 0.2164$, $p < 0.01$, $R^2 = 0.0159$) and $+0.19$ pp in SIC 59 ($b = 0.1868$, $p < 0.01$, $R^2 = 0.0211$). After running robustness checks—winsorizing extreme returns at the 1st/99th percentiles ($b = 0.1893$ in SIC 35; $b = 0.1576$ in SIC 59) and adding year fixed effects ($b = 0.2190$ in SIC 35; $b = 0.1986$ in SIC 59)—the slope $b$ barely changed and remained positive and highly significant ($p < 0.01$) in both industries.
![Figure 2](figures/figure2_sue_vs_car.png)
Why the industries differ
We proposed that loss-making quarters (where quarterly EPS is below 0) weaken the link between earnings surprises and stock returns, because investors react less strongly to earnings surprises when a firm is losing money. Figure 3 confirms this: Retail (SIC 59) reports losses more often than Apple's sector ($27.3%$ vs. $19.7%$ of quarters), and when we remove loss-making quarters to look only at profitable ones, the SUE slope jumps in both industries—more than doubling in SIC 35 from $0.22$ to $0.52$ ($p < 0.01$) and rising in SIC 59 from $0.19$ to $0.34$ ($p < 0.01$).
![Figure 3](figures/figure3_loss_share_and_slope.png)
Advice for the client
Business implication: Realistic earnings expectations are critical for a profitable company like Apple. In profitable SIC 35 quarters, each $+1$ pp earnings surprise adds $+0.52$ pp to the 3-day return, while falling into the Low-SUE group leads to an average $-1.74%$ 3-day drop ($3.90$ pp below High-SUE quarters).
Limitation: Other market news still dominates short-term price moves ($R^2$ is only $1.6%$ in SIC 35 and $2.1%$ in SIC 59), the 3-day window misses earlier expectations priced in beforehand and slower post-earnings drift, and 2-digit SIC 35 mixes tech firms like Apple with heavy machinery manufacturers.
Next questions:
Predictive: Do earnings surprises still predict stock returns over a wider 31-day window ($\pm 15$ days around each announcement)?
Prescriptive: Should Apple benchmark itself only against direct tech competitors using narrower 3-digit SIC codes rather than all of SIC 35?
Data
Sources: Compustat (`comp.fundq`), CRSP (`crsp.dsf`, `crsp.dsi`), and the CRSP/Compustat Merged link table (`ccm_link.csv`), accessed through WRDS.
The data are not included in this repository because of the WRDS license. Someone with WRDS access can rebuild the results by running the notebook with the link file in the same folder.
Sample period and rules: Quarterly earnings announcements (`rdq`) from 2021-01-01 to 2024-12-31 for SIC 35 and SIC 59; matched within valid CCM link dates (`linkdt <= datadate <= linkenddt`); requires a known announcement date, non-missing SUE (`prc_lag1` and `eps_lag4`), prior-quarter closing price $\ge $5$, $|SUE| \le 1$, and all 3 trading days (`days_found == 3`) around Day 0.
How the tables fit together
Five merges connect Compustat financials to CRSP daily stock returns. First, `comp.fundq` is inner-joined to `ccm_link.csv` on `gvkey` within valid link dates, then left-joined to shifted copies of itself on `gvkey` and `qtr_num` ($+1$ for prior-quarter price, $+4$ for year-ago EPS) to avoid weekend/holiday date mismatches. Next, `crsp.dsf` is left-joined to `crsp.dsi` on `date` to compute daily abnormal returns, inner-joined to filtered announcements on `permno` to find the first trading day on or after the announcement (`date >= rdq`, Day 0), and finally left-joined back onto `events` on `event_id` to attach the 3-day CAR.
![Merge map](figures/merge_map.png)
How I checked the results
I verified the final `events` table for zero duplicate `gvkey` + `datadate` rows, zero announcements occurring on or before quarter-end (`rdq <= datadate`), and confirmed every Day 0 date exists in the CRSP market calendar (`crsp.dsi`). I also checked `days_found == 3` after the left join to confirm complete 3-day windows (keeping 1,729 of 1,730 events in SIC 35), manually recalculated SUE and CAR for a sample event, and tested removing the top company and top 5 extreme CARs to verify the group averages did not change materially.
Changes since Project 2
What was wrong	How I fixed it	How the results changed
Merge 1 had no explanation of why an inner join was used or what goes wrong with other joins.	Added the full explanation of why `inner` is required, what fails with `left`/`right` joins, and how one-to-many becomes one-to-one after link-date filtering.	No change to numerical results; merge documentation now matches the code.
Merge 2 described merging with `crsp.dsf` on quarter-end dates instead of self-merging `panel` on shifted quarters.	Updated Merge 2 to match the notebook code (`last_prc` shifted by $+1$ and `last_eps` shifted by $+4$ on `gvkey` and `qtr_num`).	No change to numerical results; documentation now accurately reflects the weekend/holiday-safe lag merge.
Merges 3 and 4 were missing alternative-join failures and one-to-one vs. one-to-many cardinality.	Added explanations of what goes wrong with other joins and stated match cardinality for both merges, then reran the notebook top to bottom.	Confirmed all 1,729 SIC 35 and 491 SIC 59 events, t-tests, regressions, and figures reproduce cleanly.
Where the code came from
Started from the class starter notebook and adapted it for SIC 35 (Apple) and SIC 59 (Retail), with AI assistance for checking merges, reviewing slides, and formatting the README.
AI tool	Link (or PDF in docs/)	What I used it for
Gemini	[Insert share link or PDF path in docs/]	Checked merge plan against notebook code, reviewed presentation slides, drafted presentation script, and formatted README.
Files

GEMINI FILE
https://gemini.google.com/share/d/1jlqh86IiJZUVZTe-wbwnJK0R83t22YF9?usp=sharing 
```
notebooks/   Assar_Khaled_Project3.ipynb (runs top to bottom on WRDS JupyterHub)
figures/     figure1_car_by_sue_group.png, figure2_sue_vs_car.png, figure3_loss_share_and_slope.png, merge_map.png
docs/        Assar_Khaled_Project3.pdf (slides)
README.md
```
