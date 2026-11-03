# Smart-Mobility Assessment of Urban Districts (Shannon Entropy + BWM + CODAS)

A multi-criteria decision analysis (MCDA) model that ranks five districts of a large metropolitan city
(named A–E) by their level of smart / intelligent transport provision,
using **13 criteria**, **two weighting methods** and the **CODAS** ranking method.

This repository is a Python reproduction of the analysis in my MSc thesis in Urban Planning
(2024), which was later developed into a research paper.

![Ranking comparison](ranking_comparison.png)

## Research question
Which districts are better equipped with smart-mobility infrastructure, and how robust is
this ranking to the way the criteria are weighted?

## Data
- **Alternatives:** five urban districts (A, B, C, D, E)
- **Criteria (13):** intelligent traffic control, smart traffic lights, CCTV, highway speed control,
  VMS, bus-lane cameras, smart BRT and metro stations, access to smart bus stations,
  mechanised pedestrian bridges, air-quality monitoring, and accessibility of BRT/metro stations
  for people with disabilities. Full list in [`criteria.csv`](criteria.csv).
- One criterion (*access to smart bus stations*) is a **cost** criterion (lower is better);
  all others are **benefit** criteria.
- District values: [`decision_matrix.csv`](decision_matrix.csv)
- **Sources:** combined data from the city municipality, transport contractors and the author's own field surveys (collected for the MSc thesis).

## Method
1. **Shannon entropy** (objective weights): criteria with more variation between districts get higher weight.

   p<sub>ij</sub> = x<sub>ij</sub> / Σ<sub>i</sub> x<sub>ij</sub>,  E<sub>j</sub> = −(1/ln n) Σ<sub>i</sub> p<sub>ij</sub> ln p<sub>ij</sub>,  w<sub>j</sub> = (1 − E<sub>j</sub>) / Σ<sub>j</sub>(1 − E<sub>j</sub>)

2. **Best-Worst Method, BWM** (subjective weights): pairwise comparisons by urban-planning and
   transport experts, collected through a questionnaire and aggregated
   ([`weights_bwm.csv`](weights_bwm.csv)). Individual responses are not published.
3. **Combined weights:** the average of Shannon and BWM weights.
4. **CODAS** (Keshavarz Ghorabaee et al., 2016) with each weight set:
   - linear normalisation (x / max for benefit, min / x for cost criteria)
   - weighted matrix and negative-ideal solution
   - Euclidean (E) and Taxicab (T) distances from the negative-ideal solution
   - relative assessment matrix, h<sub>ik</sub> = (E<sub>i</sub> − E<sub>k</sub>) + ψ(E<sub>i</sub> − E<sub>k</sub>)·(T<sub>i</sub> − T<sub>k</sub>), with threshold τ = 0.02
   - score H<sub>i</sub> = Σ<sub>k</sub> h<sub>ik</sub> (higher = better)

## Results

| District | Shannon | BWM | Average |
|---|:-:|:-:|:-:|
| District B  | 1 | 1 | 1 |
| District D | 2 | 3 | 2 |
| District A  | 3 | 2 | 3 |
| District C | 4 | 4 | 4 |
| District E | 5 | 5 | 5 |

- **District B** ranks first and **District E** last under all three weight sets.
- Only Districts D and A swap places, depending on whether expert (BWM) or data-driven
  (Shannon) weights are used, so the ranking is largely robust to the weighting method.

Detailed scores: [`codas_scores.csv`](codas_scores.csv) ·
weights: [`weights.csv`](weights.csv)

## Reusing the model
The code is not specific to this case study. To rank other alternatives (cities, districts, projects),
replace `decision_matrix.csv`, `criteria.csv` and `weights_bwm.csv` with your own
values and run the script again.

## How to run
```bash
pip install -r requirements.txt
python codas.py
```

## Repository files
```
decision_matrix.csv, criteria.csv, weights_bwm.csv   input data
codas.py                                            Shannon entropy, CODAS and chart
weights.csv, codas_scores.csv, ranking_comparison.png  results
```

## Author
**Yasamin Dorji**, MSc student, EIT Urban Mobility Master School (IST Lisbon / UPC Barcelona)
[LinkedIn](https://www.linkedin.com/in/yasamin-dorji) ·
[Google Scholar](https://scholar.google.com/citations?user=Bj5jfzYAAAAJ) ·
[ORCID](https://orcid.org/0009-0003-0881-3073)

## Reference
Keshavarz Ghorabaee, M., Zavadskas, E. K., Turskis, Z., & Antucheviciene, J. (2016).
A new combinative distance-based assessment (CODAS) method for multi-criteria decision-making.
*Economic Computation and Economic Cybernetics Studies and Research*, 50(3), 25–44.
