# DMBI CA Submissions: VESIT D15A Group 09

Data Mining and Business Intelligence, Semester V, 2026-27.

| Name | Roll No. |
|---|---|
| Jaiprakash Talreja | 54 |
| Adarsh Singh | 52 |
| Vidushi Singh | 53 |
| Piyush Shukla | 49 |

## Contents

| CA | Title | Files in `DMBI CAs 54,52,53,49` |
|---|---|---|
| CA1 Case Study | BlackSpot Miner: A Case Study-Based Data Mining Analysis of Severe Road Collisions | `CA1/` report PDF |
| CA2 Mini Project | BlackSpot Miner: Mining Road Collision Data to Locate Severe-Crash Hotspots and Prioritise Road Safety Action | `CA2/` notebook, report, slides, figures, results, interactive map |
| CA3 Content Beyond Syllabus | High-Utility Itemset Mining | `CA3/` slides, demo notebook, results |

## CA1 and CA2: BlackSpot Miner

A road authority has a limited safety budget. This project uses five years of real police collision records to find where collisions that kill or seriously injure (KSI) concentrate, which conditions raise that risk, and which sites to review first.

- **Data:** 513,801 collisions from the UK Department for Transport STATS19 record, 2021 to 2025.
- **Reference paper:** Kumar, S. and Toshniwal, D. (2015). A data mining framework to analyze road accident data. *Journal of Big Data*, 2, 26. https://link.springer.com/article/10.1186/s40537-015-0035-y
- **SDG goal:** SDG 3 (target 3.6, road deaths and injuries) and SDG 11 (sustainable cities and safe transport).

Results below are tested on 2025, a year not used for mining or training.

| Task | Method | Result |
|---|---|---|
| Where | DBSCAN | 804 street-scale hotspots; 4.79 times the 2025 KSI collisions per km² of random past sites; 93.89% of KSI collisions fall outside any hotspot |
| Why | Apriori | 107 of 109 rules kept their lift on 2025 |
| Segments | K-Modes, k = 6 | 60 rules appear only inside segments; 55 held on 2025 |
| Severity | Random Forest, best of 4 | ROC-AUC 0.679, PR-AUC 0.440 against 0.270 by chance |
| What next | Poisson boosting on 500 m cells | ROC-AUC 0.764 against 0.680 for a past-count list; 2.13 more points of KSI covered at 5,000 sites |

Download `blackspot_map.html` and open it in a browser for the interactive map of hotspots and 2026 priority sites. GitHub shows only its source code. The full ranked list is in `results/priority_sites_2026.csv`.

**Limits.** STATS19 covers police-reported injury collisions only and has no traffic volumes. Rules and models show association, not cause, and no crash reduction is claimed. The numbers describe Great Britain; the method could run on India's eDAR records, but the numbers do not transfer.

## CA3: High-Utility Itemset Mining

Apriori, in Module V, counts how often items appear together. High-utility itemset mining also measures what those items are worth. The demo implements HUI-Miner with the FHM pair check and an FHM+ length limit, checks it against exhaustive search on 300 random databases, and runs it on UCI Online Retail.

- The top 20 itemsets by support and by utility share only 2 itemsets.
- The top 20 by utility hold 1.65 times the revenue of the top 20 by support.
- 77.6% of high-utility itemsets fall below 1% support, so Apriori at that setting misses them.

Scope: top 300 products by revenue (54.7% of the total), itemsets of up to 3 items, minimum utility 0.25% of revenue. Utility here means revenue, because profit is not in the data.

**Main reference:** Gan, W., Lin, J. C.-W., Fournier-Viger, P., Chao, H.-C., Tseng, V. S. and Yu, P. S. (2021). A Survey of Utility-Oriented Pattern Mining. *IEEE Transactions on Knowledge and Data Engineering*, 33(4), 1306-1327. https://doi.org/10.1109/TKDE.2019.2942594

## Running the notebooks

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Run each notebook from its own folder. Each one downloads its data from the official source on first run. The CA2 notebook takes about 20 minutes and the CA3 notebook about 5 minutes. Seeds are fixed, and all reported numbers are written to `results/metrics.json`.

## Data sources

- Department for Transport, [Road Safety Open Data](https://www.gov.uk/government/statistical-data-sets/road-safety-open-data). Contains public sector information licensed under the Open Government Licence v3.0.
- *Online Retail* (2015), UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33
