# Cloud Computing Platforms Co-Use

**Project:** Cloud Computing Platforms Co-Use: A Network Analysis of the Stack Overflow Developer Survey



## Overview

This project examines how respondents to the Stack Overflow Developer Survey report using cloud platforms together. It creates a weighted, undirected co-use network, identifies central platforms and strong pairs, and uses community detection to identify groups of platforms. The current notebook reproduces the **2024** .

## Research questions

- **RQ1:** Which cloud platforms are most central in the co-use network?
- **RQ2:** Which platform pairs are most frequently used together, and which show associations beyond platform popularity?
- **RQ3:** Are there identifiable communities of co-used cloud platforms?
- **RQ4:** How do the network and co-use patterns differ between 2024 and 2025? **Pending analysis.**

The above are practical summaries; replace with the exact final RQ wording from the team paper when finalized.

## Source data

- [Stack Overflow Developer Survey 2024](https://survey.stackoverflow.co/2024/)
- [Stack Overflow Developer Survey 2025](https://survey.stackoverflow.co/2025/) — future comparison
- Field used in 2024: `PlatformHaveWorkedWith`
- See [`data/README.md`](data/README.md) for download and processing details.

The raw 2024 CSV is too large for GitHub's 100 MB individual-file limit and must not be committed.

## Repository structure

```text
cloud-platforms-couse/
├── README.md
├── requirements.txt
├── LICENSE
├── .gitignore
├── notebooks/
│   ├── Cloud_CoUse_Network_2024.ipynb          # configurable, export-ready notebook
│   └── Cloud_Platform_Co_Usage_2024_original.ipynb  # original with saved outputs
├── data/
│   ├── README.md
│   ├── raw/                                   # local CSV only, ignored by git
│   └── processed/                             # generated CSV files
├── results/
│   ├── tables/                                # generated CSV tables
│   └── figures/                               # generated PNG figures
└── docs/
    ├── Results_section_draft.pdf             # 2024 results excerpt (not full paper)
    └── source/main.tex                       # current LaTeX source, may be incomplete
```

## Reproduce the 2024 results

1. Download and extract the official **2024** survey dataset; find `results-2024.csv`.
2. Clone this repository, then put the CSV under `data/raw/results-2024.csv`. Alternatively, upload it to Google Drive as `MyDrive/results-2024.csv`.
3. Install Python 3 and dependencies: `pip install -r requirements.txt`. In Google Colab, open `notebooks/Cloud_CoUse_Network_2024.ipynb` and install any required dependencies.
4. **If using Colab:** clone the repository in a cell: `!git clone https://github.com/USERNAME/cloud-platforms-couse.git`; change working directory to the cloned folder; open the notebook from the repository or set `PROJECT_ROOT` and `DATA_PATH` in its first setup cell to the appropriate paths. A notebook opened directly from GitHub in Colab may have `/content` as its working directory; adjust `PROJECT_ROOT` explicitly in that case.
5. Run all cells top to bottom. Processed CSV files, tables and figures should appear in `data/processed/` and `results/`.
6. Check the expected results below, inspect images/tables, and commit generated files after verification.

### Notebook settings

`THRESHOLD = 50` (minimum co-users for a filtered edge), 100 Louvain runs (seeds 0–99), and a `lift` edge attribute for the RQ3 community analysis. The original notebook is kept separately so the starting analysis is available.

## Expected 2024 results (from existing saved outputs and Results draft; **not independently rerun here**)

| Metric | Expected value |
| --- | ---: |
| Respondents after filtering | 41,293 |
| Included platforms | 22 |
| Full-network links | 231 |
| Filtered links at threshold 50 | 183 |
| Filtered network density | 0.792 |
| Filtered co-use retained | 98.7% |
| Communities using Louvain with lift | 3 |
| Modularity using lift | 0.148 |
| Same Louvain partition across seeds | 100/100 |

**RQ3 communities:** (1) AWS, Microsoft Azure, Google Cloud, Oracle Cloud (OCI), Databricks, IBM Cloud; (2) Firebase, Vercel, Heroku, Netlify, Supabase, Render, Fly.io, PythonAnywhere; (3) Cloudflare, DigitalOcean, Hetzner, Linode (Akamai), OVH, Vultr, Alibaba Cloud, Scaleway. The communities reflect usage patterns, not platform-category labels used as model features.

**Threshold sensitivity:** 30/50/75 co-users produce 3 communities; threshold 100 produces 4 communities with 148 links. Therefore the 148-link/4-community result should not be reported as the base 50-co-user result.

## Analysis and interpretation

- **Centrality:** degree/closeness/betweenness are calculated on the filtered network; strength and eigenvector on the full network.
- **Association:** lift adjusts shared usage for each platform's individual popularity.
- **Validation:** compare popularity with centrality using Spearman correlation; compare 100 Louvain runs, greedy modularity, and four edge-filter thresholds.
- **Limits:** Survey-based co-use is not evidence that specific organizations have technically integrated or jointly deployed the platforms.

## Reports and presentation

- [`docs/Results_section_draft.pdf`](docs/Results_section_draft.pdf) is only the provided 2024 Results section.
- [`docs/source/main.tex`](docs/source/main.tex) is the supplied LaTeX source; confirm that it is the team's final version.
- **TODO:** Add the complete `docs/report.pdf` and `docs/presentation.pdf` before submission.


## Reproducibility status

The three PNG figures in `results/figures/` were extracted from saved output images in the original notebook and are not a new analysis run. The original notebook and its saved outputs were supplied by the team; the adapted notebook has not been executed with the raw source dataset as it is not included in the uploaded files. For reproducibility grading, rerun the adapted notebook with the official survey CSV, verify metrics, and commit the resulting `data/processed/` and `results/` files. This package is a reproducibility scaffold, **not** yet a fully validated reproduction.
