# rRNA Modification Detection Tool Benchmarking

Repository for reproducing all figures from the paper:  
**Benchmarking Tools for Identification of rRNA Modifications**

## 📥 Inputs

All input files **must** be placed inside the `rnamod-tool-benchmark-code/` directory (this is the exact location expected by every notebook).

### Required CSV Files (Core Inputs)

These four CSV files are **mandatory** for the notebooks to run:

| Filename                        | Used in Notebook                          | Purpose                                                                 | Figures Generated                  |
|---------------------------------|-------------------------------------------|-------------------------------------------------------------------------|------------------------------------|
| `metrics_summary_long.csv`      | `auroc_auprc_f1.ipynb`                    | Aggregated performance metrics (AUROC, AUPRC, precision, recall, F1, etc.) | Fig. 3–9 + Supp. S3–S6, S9        |
| `metrics_long.csv`              | `auroc_auprc_f1.ipynb`                    | Detailed per-replicate long-format metrics (validation & GT recovery)   | All GT recovery tables             |
| `coverage_completeness_long.csv`| `completeness_inflation_plots.ipynb`      | Tool output completeness (%) per coverage level                         | Supp. Fig. S7                      |
| `scope_metric_summary.csv`      | `completeness_inflation_plots.ipynb`      | Scope comparison (Universe vs Reported AUROC/AUPRC)                     | Supp. Fig. S8                      |

> **Optional but recommended**: `lag_metrics_summary_long.csv` (used for window analysis in Supp. Fig. S9).

### Additional Data Directories (for Figures 1, 2, S1, S2)

The notebook `Figs_1-2-S1-S2_notebook.ipynb` requires the following folders with raw sequencing analysis files:

- **`depths/`**  
  Contains 18 samtools depth files (tab-separated, no header):  
  `k12_{native|ivt}_bc{1|2|3}_mapped_{16s|23s}_sorted.txt`

- **`nanoplot_rawdata/`**  
  Contains 6 NanoPlot pickle files:  
  `k12_{native|ivt}_bc{1|2|3}_mapped_{16s|23s}-NanoPlot-data.pickle`

- **`subsampled_coverage_depths/`**  
  Organized sub-folders with subsampled depth files (used for Supp. Fig. S1):  
  `subsampled_coverage_depths/{cov}x_coverage_depth/{kind}/{rna}/`

### Expected Folder Structure

rnamod-tool-benchmark/
├── rnamod-tool-benchmark-code/               ← All notebooks and CSVs go here
│   ├── completeness_inflation_plots.ipynb
│   ├── Figs_1-2-S1-S2_notebook.ipynb
│   ├── auroc_auprc_f1.ipynb
│   ├── metrics_summary_long.csv
│   ├── metrics_long.csv
│   ├── coverage_completeness_long.csv
│   ├── scope_metric_summary.csv
│   ├── depths/
│   ├── nanoplot_rawdata/
│   ├── subsampled_coverage_depths/
│   └── downstream_analysis/                  ← Python module used by auroc notebook
├── plots/                                    ← Auto-created output folder
├── completeness_inflation_figures/           ← Auto-created
└── clean_figures_chapter3_copy/              ← Auto-created

**Tip**: If your data lives elsewhere, update the RUN_DIR, BASE_DIR, or path variables at the top of each notebook.

### Requirements

#### Python Packages

```
pandas
numpy
matplotlib
Pillow
seaborn
```

#### Environment

1. Python 3.10+
2. Jupyter Notebook

### Usage

1. Clone the repo and place all input files as shown above.
2. Install requirements:Bashpip install -r requirements.txt
3. Open the notebooks in Jupyter and run them in this order (recommended):
4. Figs_1-2-S1-S2_notebook.ipynb → generates Fig. 1, Fig. 2, Supp. Fig. S1, S2
5. completeness_inflation_plots.ipynb → generates Supp. Fig. S7, S8
6. auroc_auprc_f1.ipynb → generates Fig. 3–9 + Supp. Fig. S3–S6, S9 + tables


Each notebook automatically creates its own output folder and saves PNG (600 dpi), PDF, and SVG versions of every figure.

### Outputs

All figures are saved in the following auto-created folders:

plots/ → Fig. 1, 2, S1, S2
completeness_inflation_figures/ → Supp. Fig. S7, S8
clean_figures_chapter3_copy/ → Fig. 3–9 + all other supplementary figures + CSV tables

Tables (e.g., GT recovery, operating points) are also exported as CSV files inside the figure output folders.