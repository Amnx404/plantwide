### Analysis 1 – simple dynamic surrogate (tutorial)

This folder contains a first “warm‑up” problem for building surrogate models on synthetic process data.

#### 1. What data do we have?

- The Excel file `simple_dynamic_process.xlsx` has two sheets: **Training Data** and **Testing Data**.
- Each sheet has a time index and four tag columns:
  - `Time` – just a sample counter (we do not use it as a model input).
  - `U1`, `U2` – process inputs.
  - `Y1`, `Y2` – process outputs.
- The data is noise‑free and comes from a simple dynamic test model.

#### 2. What problem are we solving?

We want a surrogate model that can predict the outputs (Y1, Y2) from the inputs (U1, U2), including the dynamic behaviour over time.  
The model should work on the **test** sheet, not just replay the training data.

#### 3. How do we solve it?

The notebook `experiment.ipynb` walks through three steps:

1. **Data loading**  
   - Read the Excel file and split into training and testing sets.  
   - Separate inputs (U1, U2) and outputs (Y1, Y2).

2. **Static models (no history)**  
   - Train **Random Forest** and **XGBoost** using only the current inputs U1(t), U2(t) to predict Y1(t), Y2(t).  
   - Evaluate on both train and test sets using MAE, RMSE, and R².

3. **Dynamic models (with history)**  
   - Build new features that include short histories of the inputs and outputs (for example U1 and U2 at the current and previous step, and Y1 and Y2 from previous steps).  
   - Train Random Forest and XGBoost on these lagged features.  
   - Evaluate again on train and test, and compare to the static models.

#### 4. What is the accuracy?

The notebook prints metric tables (MAE, RMSE, R2) for each model and each output.  
For the **dynamic** models on the **test** data, the numbers look like this:

Dynamic Random Forest (test):

| Output | MAE     | RMSE    | R2      |
|--------|---------|---------|---------|
| Y1     | 0.1371  | 0.3419  | 0.9935  |
| Y2     | 0.2348  | 0.5542  | 0.9985  |

Dynamic XGBoost (test):

| Output | MAE     | RMSE    | R2      |
|--------|---------|---------|---------|
| Y1     | 0.1391  | 0.3924  | 0.9915  |
| Y2     | 0.2278  | 0.5070  | 0.9987  |

So the test R2 values are all around 0.99 or higher, and the errors are small, which means the dynamic surrogates match the test trajectories very well on this warm‑up case.

#### 5. What are the limitations?

- The data is synthetic and noise‑free, with clean inputs and perfect measurements. Real plant data will be noisier and harder to model.
- Only two inputs and two outputs are considered here; real systems may involve many more tags and interactions.
- The lag structure (how many past steps we include) is fixed by hand. A poor choice of lags can hurt performance, and the best choice may vary by process.
- The models are “black boxes”: they approximate the mapping well, but they do not provide physical insight or guarantees outside the range of the training data.

#### 6. Results and plots

The notebook includes tables with the metric values and several plots of actual vs predicted outputs.  
For quick reference, here is the **dynamic XGBoost test table** again:

| Output | MAE     | RMSE    | R2      |
|--------|---------|---------|---------|
| Y1     | 0.1391  | 0.3924  | 0.9915  |
| Y2     | 0.2278  | 0.5070  | 0.9987  |

The two PNGs below capture example results from the same dynamic XGBoost model and are here to help you visualise what is going on.

- The first figure shows **timestep‑wise error behaviour** (how the prediction error changes over time on the test set).
- The second figure shows **Y1 and Y2 trajectories**: the true curves and the model’s predicted curves over time.

![Timestep-wise XGBoost performance](./timestepwise_xgboost.png)

![Dynamic XGBoost trajectories](./dynamic_xgboost.png)

You can re‑run `experiment.ipynb` to regenerate the metrics and plots, or tweak the configuration (lags, model hyperparameters) and see how the results change.