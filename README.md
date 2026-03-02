# Branching-Trajectory-Inference-for-Droplet-scRNA-seq
Reproducible benchmark for branching trajectories in droplet scRNA-seq, using droplet-realistic NB simulation, glass-box diffusion pseudotime, lineage proxy, and composites.

## Branching Trajectory Inference for Droplet scRNA-seq (Glass-Box Diffusion Pseudotime)

A reproducible, CPU-friendly benchmarking framework for branching trajectory inference in droplet single-cell RNA-seq.  
The repository simulates droplet-realistic UMI counts with known ground truth, runs a transparent diffusion-pseudotime pipeline on a kNN graph, assigns two lineages via terminal-state separation, prioritizes progression and branch markers, and exports publication-style composite figures and standardized tables.

## Why this repo exists
Most trajectory tutorials and pipelines assume heavy single-cell stacks or GPU-friendly environments. This project provides a minimal-dependency baseline that:
- runs on modest hardware (CPU),
- exposes each algorithmic step (graph, diffusion operator, eigendecomposition, pseudotime),
- enables quantitative auditing using simulated ground truth,
- produces ready-to-use composite figures for reporting.

## Key outputs
After running the pipeline, you will get a structured `results_trajectory_sim_no_scanpy/` folder containing:
- **Figures** (PNG and PDF), including:
  - `COMPOSITE_overview.png`, `COMPOSITE_overview.pdf`
  - `COMPOSITE_gene_programs.png`, `COMPOSITE_gene_programs.pdf`
  - QC plots, diffusion diagnostics, gene-trend panels, heatmaps
- **Tables** (CSV), including:
  - `qc_cell_metrics.csv`
  - `gene_mean_variance.csv`
  - `pseudotime_gene_correlations.csv`
  - `late_branch_mean_differences.csv`
  - `FINAL_summary.csv`
- **Data** objects (NPZ, NPY, CSV) for reproducibility:
  - `counts_raw.npz`
  - `pca_Z.npy`, `diffusion_Y.npy`
  - `obs_with_pseudotime.csv`

## Repository structure
Recommended layout:
```
├── src/
│   ├── trajectory_sim_no_scanpy.py
│   └── utils_plotting.py
├── notebooks/
│   └── trajectory_sim_no_scanpy.ipynb
├── results_trajectory_sim_no_scanpy/
│   ├── figures/
│   ├── tables/
│   └── data/
├── README.md
├── modelcard.md
├── datasheet.md
└── LICENSE
```
## Methods overview 
	1.	Simulate droplet-like counts with a negative binomial (Gamma–Poisson) model, including library-size heterogeneity and branch-specific gene programs.
	2.	Normalize + log1p, then select highly variable genes (HVGs).
	3.	Run PCA, build a kNN graph, compute a diffusion operator.
	4.	Infer diffusion pseudotime as diffusion-space distance from an early root cell.
	5.	Infer two lineages via terminal-state separation in diffusion space.
	6.	Compute gene priorities:
	•	Progression genes via gene–pseudotime correlation.
	•	Branch markers via late-stage lineage mean differences.
	7.	Export tables and publication-style composite figures.

## Scientific grounding
	•	Droplet scRNA-seq zero structure often does not require explicit zero inflation: Svensson (2020).
	•	Negative binomial regression normalization and variance stabilization: Hafemeister & Satija (2019).
	•	Diffusion pseudotime for branching trajectories: Haghverdi et al. (2016).
	•	Lineage and pseudotime inference: Street et al. (2018).

## Reproducibility
	•	A fixed random seed is recorded.
	•	Simulation metadata is saved in results_trajectory_sim_no_scanpy/data/simulation_metadata.json.
	•	All intermediate arrays and final outputs are written to disk.

## Limitations
	•	The lineage step is a pragmatic two-terminal proxy rather than a full lineage curve model.
	•	PCA is used as a lightweight 2D visualization embedding (not UMAP).
	•	Simulation is designed to be droplet-like, but it does not include batch effects, ambient RNA, or doublets.

## How to extend
	•	Replace log-normalization with negative binomial regression residuals.
	•	Add lineage-aware curve fitting (e.g., Slingshot-style).
	•	Add parameter sweeps for kNN and kernel bandwidth to quantify robustness.
	•	Add batch effects and confounders to simulation for stress-testing.

## License
MIT 

## Citation
**Petalcorin, M.I.R.** (2026). A Reproducible, Benchmarking Framework for Branching Trajectory Inference in Droplet Single-Cell RNA-seq. GitHub. https://github.com/mpetalcorin/Branching-Trajectory-Inference-for-Droplet-scRNA-seq
