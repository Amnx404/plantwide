### Dynamic surrogate model (plain language)

This project builds surrogate models for a simple dynamic process with:
- Inputs: U1, U2
- Outputs: Y1, Y2

The base analysis notebook and data live under `analysis/`:

- `analysis/experiment.ipynb` – main analysis notebook
- `analysis/simple_dynamic_process.xlsx` – training and testing data
- `analysis/simple_dynamic_process.cfg` – simple tag/config info
- `analysis/simple_dynamic_process.pptx` – original case description slides

The notebook has three parts:

- **1. Data loading**: read the Excel file (`simple_dynamic_process.xlsx`), pick the training and testing sheets, and split inputs/outputs.
- **2. Static models**: train Random Forest and XGBoost models that use only the current inputs (U1, U2 at this time step) to predict the current outputs (Y1, Y2 at this time step).
- **3. Dynamic models**: train versions of the models that also look back in time.

For the **dynamic** part, for each time step we:
- collect the current inputs U1 and U2,
- optionally collect a few previous values of the inputs (U1 and U2 at earlier time steps),
- optionally collect a few previous values of the outputs Y1 and Y2.

All of that recent history is stacked into one feature vector, and the target is still the current outputs Y1 and Y2.  
Random Forest and XGBoost then learn a mapping from “recent history” to “next outputs”.

If this works well, the dynamic test plots in the notebook will show the predicted curves for Y1 and Y2 closely following the true curves, especially when the system has noticeable time‑dependent behaviour (lags, slow responses, etc.).