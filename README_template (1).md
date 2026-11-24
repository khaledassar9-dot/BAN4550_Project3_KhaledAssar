# [Your title: a question a client would ask]

> Example (different topic): *Do coffee shops near universities sell more on exam weeks?*

**Author:** [Your name] · BAN 4550, Fall 2026 · [Industry: 2-digit SIC and name] · Comparison industry: [2-digit SIC and name]

## The question
One or two sentences in plain words: who the client is and what they want to know.

> Example: *A coffee chain wants to know whether stores near campuses should add staff during exam weeks.*

## Main result
Two or three sentences with your key number, then Figure 1.

![Figure 1](figures/figure1.png)

> Example: *Stores near campuses sold 18% more during exam weeks; other stores showed no change.*

## Is the difference real?
The t-test (difference, t-statistic, p-value) and the regression (b, p-value, R²) for each industry, in plain words, then Figure 2. If you ran more than one model, say in one sentence whether b changed.

![Figure 2](figures/figure2.png)

## Why the industries differ
The reason you proposed, and what Figure 3 showed when you checked it.

![Figure 3](figures/figure3.png)

## Advice for the client
- **Business implication:** what the client should do or learn.
- **Limitation:** one reason to be careful (data, sample size or method).
- **Next questions:** one predictive and one prescriptive question.

## Data
- Sources: Compustat (`comp.fundq`), CRSP (`crsp.dsf`, `crsp.dsi`), accessed through WRDS. Add or remove tables to match your work.
- **The data are not included in this repository** because of the WRDS license. Someone with WRDS access can rebuild the results by running the notebook with the link file in the same folder.
- Sample period and rules: [period, filters].

## How the tables fit together
Your merge map (image) and two or three sentences on the keys and the date rule.

![Merge map](figures/merge_map.png)

## How I checked the results
Your most useful check in two or three sentences: what you expected, what you found, what you did.

## Changes since Project 2
| What was wrong | How I fixed it | How the results changed |
| --- | --- | --- |
| | | |

Write "None" if there was nothing to fix.

## Where the code came from
The class notebook, an AI tool, or your own changes. List every AI conversation you used, one line per conversation:

| AI tool | Link (or PDF in docs/) | What I used it for |
| --- | --- | --- |
| | | |

Write "None" if you used no AI.

## Files
```
notebooks/   Last_First_Project3.ipynb (runs top to bottom on WRDS JupyterHub)
figures/     figure1.png, figure2.png, figure3.png, merge_map.png
docs/        Last_First_Project3.pdf (slides)
README.md
```
