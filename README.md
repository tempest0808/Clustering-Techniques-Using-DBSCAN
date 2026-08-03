# MSCS_634_Lab_5 — Clustering Techniques: Hierarchical and DBSCAN

**Name:** Richin Swaroop Dasari
**Course:** 2026 Summer - Advanced Big Data and Data Mining (MSCS-634-B01) - Second Bi-term
**Assignment:** Lab 5: Clustering Techniques - Hierarchical and DBSCAN Clustering

## Purpose

For this lab I worked with the **Wine dataset** from `sklearn.datasets` — 178 wines with 13 chemical measurements each, coming from 3 different cultivars grown in the same region of Italy. The goal was to:

- Standardize the features and explore the dataset's structure.
- Apply Agglomerative Hierarchical Clustering with different numbers of clusters, and interpret a dendrogram.
- Apply DBSCAN with different `eps`/`min_samples` combinations and see how they affect cluster formation and noise.
- Evaluate clustering quality with silhouette, homogeneity, and completeness scores.
- Compare the two algorithms and reflect on their strengths and weaknesses.

## Key Insights

- **Hierarchical clustering at n_clusters=3 lined up well with the true cultivar structure.** It got the highest silhouette score among the values I tried (2, 3, 4, 5), and the dendrogram's top-level branch structure backed that up visually before I even looked at the true labels.
- **Hierarchical clustering clearly outperformed DBSCAN on this dataset** — homogeneity and completeness scores of about 0.79 and 0.78 for hierarchical, versus about 0.49 and 0.51 for DBSCAN. The wine cultivars form roughly equal-density, evenly-shaped groups in the standardized feature space, which is exactly the kind of structure Ward-linkage hierarchical clustering is good at finding.
- **DBSCAN was much more sensitive to its parameters than I expected.** Small changes in `eps` swung the result from "almost everything is noise" (very small eps) to "everything collapses into one giant cluster" (eps too large), with only a fairly narrow band in between where it found a sensible number of clusters at all.
- **There was a real tradeoff between cluster count and noise for DBSCAN.** Getting close to 3 clusters (eps=2.3, min_samples=3) still left about 18% of the data labeled as noise — points on the boundary between cultivars that didn't have enough dense neighbors to join a cluster, not necessarily true outliers.
- **DBSCAN's core assumption (dense regions separated by sparse ones) didn't fit this dataset as well as I initially assumed.** The wine cultivars sit fairly close together with only a gradual density drop-off between them, rather than being separated by clearly empty space, which is the scenario DBSCAN is really built for.

## Challenges and Decisions

- **Choosing the DBSCAN parameters to feature.** My first pass at eps=2.5 collapsed everything into a single cluster once noise points were excluded, which broke the silhouette score calculation (it needs at least 2 groups). I did a finer sweep around the eps=2.0-2.5 range and landed on eps=2.3, min_samples=3, which was the best combination I found for getting close to 3 clusters without excessive noise.
- **Visualizing 13-dimensional data.** Since the Wine dataset has 13 features, I used PCA to project down to 2 components purely for plotting — the actual clustering was always done on the full standardized 13-feature space, not on the PCA output, so the visualizations are a simplification for interpretation rather than what the algorithms actually see.
- **Keeping the evaluation honestly unsupervised.** I only used the true cultivar labels for the homogeneity and completeness metrics at the very end, never to influence how the clustering itself was done, so the comparison reflects what each algorithm can actually discover on its own.

## Contents

- `MSCS_634_Lab_5.ipynb` — the notebook with all the data exploration, clustering, evaluation, and visualizations.
- `README.md` — this file.

## How to Run

1. Open `MSCS_634_Lab_5.ipynb` in Jupyter Notebook or JupyterLab.
2. Run all cells top to bottom (`Kernel > Restart & Run All`).
3. Needs: `numpy`, `pandas`, `matplotlib`, `seaborn`, `scipy`, `scikit-learn`.
