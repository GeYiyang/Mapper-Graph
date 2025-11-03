# Mapper Graph Visualizer (Gudhi + scikit-learn)

This repository provides a minimal, reproducible pipeline to build and visualize a **Mapper graph** from a tabular dataset (CSV).  
It uses **StandardScaler → PCA(1) + kNN-distance** as filters and **adaptive K-Means** within Gudhi’s `MapperComplex`.  
The output is a pruned NetworkX graph plotted with Matplotlib.

## Features
- Reproducible Mapper pipeline in ~150 lines.
- Two filter options:
  - `USE_2D_FILTER=True`: [PCA1, kNN distance]
  - `USE_2D_FILTER=False`: [PCA1 only]
- Adaptive K-Means per cover bin (caps cluster count by data size).
- Pruning utilities: drop isolates / small components.
- Stable layouts: `spring` (default) or `kamada`.

## Quick Start

```bash
python -m venv .venv
# Windows
.\.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt

python mapper_graph2.py
```

Input
Place your CSV in the repo root and set:

DATA_CSV = "complete_case.csv" (or your filename)

The script assumes rows = samples and columns = features (numeric).

Output
A Matplotlib window showing the pruned Mapper graph (nodes colored by selected filter dimension).

No files are written by default. You can save the figure via Matplotlib UI if needed.

Parameters (edit at the top of mapper_graph2.py)
DATA_CSV: input CSV path.

RES (int): cover resolution per filter axis.

GAIN (0,1]: cover overlap (higher → more overlap).

USE_2D_FILTER: True → [PCA1, kNNdist], False → [PCA1].

KNN_K: k for kNN distance as the second filter.

N_CLUSTERS: base K-Means clusters per bin (adapted to sample count).

RANDOM_STATE: reproducibility for PCA/layout/cluster seeding.

Plotting/pruning

DROP_ISOLATES: drop degree-0 nodes.

MIN_COMPONENT_SIZE: drop connected components smaller than this.

COLOR_DIM: 0 (PCA1) or 1 (kNNdist) when using 2D filter.

LAYOUT: "spring" or "kamada".

Notes & Tips
If your dataset is large/high-dimensional, std-scaling + PCA1 is usually robust.

KNN_K affects the second filter’s contrast.

RES×GAIN controls cover granularity/overlap.

If you see too many isolates, reduce pruning or increase overlap.

If you see too few nodes, increase RES or lower N_CLUSTERS.
