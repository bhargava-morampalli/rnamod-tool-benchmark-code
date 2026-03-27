# rRNA Modification Detection Tool Benchmarking

Repository for reproducing all figures from the paper:
**Benchmarking Tools for Identification of rRNA Modifications**

---

## Overview

Ten tools are evaluated across two rRNA references (16S and 23S), three biological replicates, and 25 coverage levels (5×–1000×). Three Jupyter notebooks generate all manuscript figures.

| Notebook | Figures Generated |
|---|---|
| `Figs_1-2-S1-S2_notebook.ipynb` | Fig. 1, Fig. 2, Supp. Fig. S1, S2 |
| `completeness_inflation_plots.ipynb` | Supp. Fig. S7, S8 |
| `auroc_auprc_f1.ipynb` | Fig. 3–9, Supp. Fig. S3–S6, S9 |

---

## Input Files

All input files **must** be placed inside the `rnamod-tool-benchmark-code/` directory, which is the location expected by every notebook.

### Required CSV Files

All six CSV files below are mandatory — the notebook raises a hard error if any are missing:

| Filename | Used In | Purpose | Figures |
|---|---|---|---|
| `metrics_summary_long.csv` | `auroc_auprc_f1.ipynb` | Aggregated performance metrics (AUROC, AUPRC, precision, recall, F1) | Fig. 3–9, Supp. S3–S6 |
| `metrics_long.csv` | `auroc_auprc_f1.ipynb` | Per-replicate long-format metrics for validation and ground-truth recovery | All GT recovery tables |
| `window_metrics_summary_long.csv` | `auroc_auprc_f1.ipynb` | AUPRC at each window size (±0–10 nt) for window relaxation analysis | Supp. Fig. S9 |
| `lag_metrics_summary_long.csv` | `auroc_auprc_f1.ipynb` | AUPRC at each directional offset (δ −10 to +10 nt) for lag/offset analysis | Fig. 6 |
| `coverage_completeness_long.csv` | `completeness_inflation_plots.ipynb` | Tool output completeness (%) per coverage level | Supp. Fig. S7 |
| `scope_metric_summary.csv` | `completeness_inflation_plots.ipynb` | Scope comparison — Universe vs Reported AUROC/AUPRC | Supp. Fig. S8 |

### Additional Data Directories

The notebook `Figs_1-2-S1-S2_notebook.ipynb` requires the following folders placed inside `rnamod-tool-benchmark-code/`:

**`depths/`** — 12 samtools depth files (tab-separated, no header), one per combination of condition, replicate, and reference:

```
k12_{native|ivt}_bc{1|2|3}_mapped_{16s|23s}_sorted.txt
```

**`nanoplot_rawdata/`** — 12 NanoPlot pickle files, one per combination of condition, replicate, and reference:

```
k12_{native|ivt}_bc{1|2|3}_mapped_{16s|23s}-NanoPlot-data.pickle
```

**`subsampled_coverage_depths/`** — Subsampled depth files organised into sub-folders by coverage level (used for Supp. Fig. S1):

```
subsampled_coverage_depths/{cov}x_coverage_depth/{kind}/{rna}/{kind}_{rna}_rep{rep}.txt
```

**`downstream_analysis/`** — Python module required by `auroc_auprc_f1.ipynb` for ground-truth recovery. Must contain `parse_outputs` and `position_standardization`.

**`tool_individual_outputs/`** — Required by `auroc_auprc_f1.ipynb` for GT site recovery. Must contain:
- `16s_positions.txt` and `23s_positions.txt` — ground-truth modification positions (one padded position per line)
- Raw tool output files for all ten tools, loaded by the `load_all_tool_outputs` function from `downstream_analysis/parse_outputs`

### Expected Directory Structure

```
rnamod-tool-benchmark/
└── rnamod-tool-benchmark-code/
    ├── Figs_1-2-S1-S2_notebook.ipynb
    ├── completeness_inflation_plots.ipynb
    ├── auroc_auprc_f1.ipynb
    ├── metrics_summary_long.csv
    ├── metrics_long.csv
    ├── window_metrics_summary_long.csv
    ├── lag_metrics_summary_long.csv
    ├── coverage_completeness_long.csv
    ├── scope_metric_summary.csv
    ├── depths/
    ├── nanoplot_rawdata/
    ├── subsampled_coverage_depths/
    ├── downstream_analysis/
    ├── tool_individual_outputs/
    ├── plots/                                ← auto-created by Figs_1-2-S1-S2 notebook
    ├── completeness_inflation_figures/       ← auto-created by completeness notebook
    └── clean_figures_chapter3_copy/          ← auto-created by auroc_auprc_f1 notebook
```

> **Tip:** If your data lives elsewhere, update `RUN_DIR`, `BASE_DIR`, or the path variables at the top of each notebook.

---

## Requirements

### Python Packages

```
pandas
numpy
matplotlib
seaborn
Pillow
```

### Environment

- Python 3.10+
- Jupyter Notebook or JupyterLab

### Installation

```bash
pip install -r requirements.txt
```

---

## Usage

Run the notebooks in the following order:

1. **`Figs_1-2-S1-S2_notebook.ipynb`** — generates Fig. 1, Fig. 2, Supp. Fig. S1, S2
2. **`completeness_inflation_plots.ipynb`** — generates Supp. Fig. S7, S8
3. **`auroc_auprc_f1.ipynb`** — generates Fig. 3–9, Supp. Fig. S3–S6, S9, and summary tables

Each notebook automatically creates its output folder and saves PNG (300 dpi), PDF, and SVG versions of every figure.

---

## Outputs

All figures are saved to auto-created folders inside `rnamod-tool-benchmark-code/`:

| Output Folder | Contents |
|---|---|
| `data_quality_figures/` | Fig. 1, 2, Supp. Fig. S1, S2 |
| `completeness_inflation_figures/` | Supp. Fig. S7, S8 |
| `metric_figures/` | Fig. 3–9, Supp. Fig. S3–S6, S9, CSV summary tables |

Tables (e.g., GT recovery, operating points) are exported as CSV files alongside the figures in `clean_figures_chapter3_copy/`.
