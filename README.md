# Plantwide surrogate modelling playground

This repo is a small sandbox for building and experimenting with surrogate models on process data.

- Environment is managed with **uv** and a local `.venv` (see `pyproject.toml`).
- Core dependencies: pandas, numpy, scikit‑learn, xgboost, matplotlib, Jupyter.
- All data files live under the repo (no external services required).

For now, the main work lives in the **`analysis1/`** directory.

### `analysis1/` – dynamic surrogate analysis

`analysis1` contains a first warm‑up case where we build surrogate models for a simple dynamic process:

- Inputs: U1, U2  
- Outputs: Y1, Y2  

Inside `analysis1` you’ll find:

- A notebook that:
  - loads synthetic training/testing data from Excel,
  - trains **static models** (Random Forest, XGBoost) that use only current inputs,
  - trains **dynamic models** that also use short histories of U1, U2, Y1, Y2 as features,
  - compares train/test metrics and plots actual vs predicted trajectories.
- A short README in `analysis1/` that explains the dynamic setup and how to interpret the plots.

Use this as a template for future cases: drop new data/configs into a sibling `analysisX/` folder and clone the notebook to try different model structures or feature choices.

