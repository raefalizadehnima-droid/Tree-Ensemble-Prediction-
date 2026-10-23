[README.md](https://github.com/user-attachments/files/33892953/README.md)
# Pharmaceutical AI reconstruction – reproducibility package

Leakage-controlled benchmark of Random Forest, XGBoost and LightGBM on Tox21 (12 endpoints), ESOL solubility and NCATS kinetic solubility
(PubChem AID 1645848), with random and scaffold splits, baselines, SHAP, calibration, uncertainty and measured CPU cost.
The accompanying manuscript is `manuscript/Reconstructed_Pharmaceutical_AI_Manuscript.docx`.

## Install and run
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
# one command, whole pipeline (training ≈ 1 h on one CPU core; resumable from checkpoints in results/metrics/*_checkpoints)
python src/run_all.py                       # uses data/raw
python src/run_all.py --uploads /path/to/original/files   # re-inventories raw uploads and copies canonical files to data/raw
python tests/test_pipeline.py               # sanity tests
```
Versions used for the reported results are in `results/metrics/environment.json` (Python 3.12.3, scikit-learn 1.8.0, RDKit 2026.09.1, XGBoost 3.4.1, LightGBM 4.7.0). Other versions may give slightly different numbers.
Random seeds, paths, features, split fractions and model grids are in `config/config.yaml`.

## Layout
- `data/raw/` supplied files (one canonical copy of each byte-identical group; duplicates are listed in `results/tables/file_inventory.csv`)
- `data/processed/` cleaned data, features (`features_*.npz`), saved split assignments (`splits_*.csv`)
- `data/external_manifest.csv`, `data/data_dictionary.csv`
- `src/` numbered scripts 00–14 plus `common.py`, `models.py`, `run_all.py`, `run_models.sh`
- `results/metrics|tables|figures|logs`; `reports/` provenance, validation, results summary, claim change log, reference-verification table
- `original/` the original manuscript, unchanged

## Script status (what was actually executed)
Scripts 00–14 were each executed in the development environment and their logs are in `results/logs`. A fresh-environment end-to-end re-run
via `run_all.py` has NOT been performed. `src/10_computational_efficiency.py` timings depend on the machine and will differ elsewhere.

## Known limitations
Partial/unknown provenance of the supplied PubChem tables, no stored dataset versions/access dates, three qHTS tables and three NCATS tables without structures
were not modelled (see `reports/Dataset_Provenance_and_Quality_Report.xlsx`). Original-source licences apply to the redistributed data.
