# MSCS 634 – Lab 5: Hierarchical and DBSCAN Clustering

**Course:** MSCS 634 – Advanced Data Mining  
**Assignment:** Lab 5 – Clustering Techniques on the Wine Dataset  
**Author:** Monalisa Patil
---

## Purpose

This lab explores two unsupervised clustering algorithms — **Agglomerative Hierarchical Clustering** and **DBSCAN** — applied to the [Wine dataset](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_wine.html) from `sklearn`. The dataset contains 178 samples with 13 chemical features measured from three cultivars of wine grown in Italy.

The goals of this lab are to:
- Understand how to prepare and standardize data for clustering
- Apply and tune Hierarchical and DBSCAN clustering algorithms
- Evaluate clustering quality using Silhouette, Homogeneity, and Completeness scores
- Compare the two algorithms and reflect on their respective strengths and weaknesses

---

## Repository Contents

| File | Description |
|------|-------------|
| `MSCS_634_Lab_5_Wine_Clustering.ipynb` | Jupyter Notebook with all code, visualizations, and analysis |
| `README.md` | This file |

---

## Key Insights

### Hierarchical Clustering
- **Dendrogram analysis** confirmed that **3 clusters** is the natural partitioning, matching the true number of wine cultivars.
- Using **Ward linkage** with `n_clusters=3` produced the best metrics:
  - Silhouette Score: ~0.28
  - Homogeneity: ~0.87
  - Completeness: ~0.87
- Testing `n_clusters=2` and `n_clusters=4` showed degraded metrics, confirming that 3 is optimal.

### DBSCAN Clustering
- DBSCAN was highly sensitive to `eps` and `min_samples`:
  - Small `eps` (1.5) → many noise points, fragmented clusters
  - Large `eps` (3.0) → most points merged into one cluster
  - Best config: **eps=2.0, min_samples=5** (2–3 clusters, moderate noise)
- DBSCAN correctly identified outlier samples as noise (labeled -1), which Hierarchical Clustering cannot do.
- However, DBSCAN's overall homogeneity and completeness scores were lower than Hierarchical on this dataset.

### Algorithm Comparison

| Metric | Hierarchical (Ward, n=3) | DBSCAN (eps=2.0, min=5) |
|--------|--------------------------|--------------------------|
| Silhouette Score | Higher | Lower |
| Homogeneity | ~0.87 | ~0.55–0.65 |
| Completeness | ~0.87 | ~0.55–0.65 |
| Noise handling | ❌ | ✅ |
| Needs k upfront | ✅ | ❌ |

**Conclusion:** Hierarchical Clustering with Ward linkage is better suited for the Wine dataset due to its compact, well-separated, similarly-dense clusters. DBSCAN's advantage is noise detection and flexibility with arbitrary cluster shapes, making it more powerful in messy real-world datasets.

---

## Challenges and Decisions

- **Visualization:** With 13 features, direct scatter plots are not possible. PCA was used to project to 2D for visualization (explaining ~55% of variance), which may slightly distort the perceived cluster separation.
- **DBSCAN tuning:** Selecting good `eps` values requires either domain knowledge or a k-distance elbow plot. Multiple configurations were tested to demonstrate parameter sensitivity.
- **Noise in DBSCAN metrics:** Silhouette and other metrics were computed only on non-noise points to avoid misleading results from unassigned samples.
- **Linkage method:** Ward linkage was selected for Hierarchical Clustering because it minimizes within-cluster variance and generally performs well on feature-rich, standardized datasets.

---

## How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/YourUsername/MSCS_634_Lab_5.git
   cd MSCS_634_Lab_5
   ```

2. Install dependencies:
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn scipy
   ```

3. Launch Jupyter:
   ```bash
   jupyter notebook MSCS_634_Lab_5_Wine_Clustering.ipynb
   ```

4. Run all cells in order (Cell → Run All).
