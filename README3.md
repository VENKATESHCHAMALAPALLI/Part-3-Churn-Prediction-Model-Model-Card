# Part 3 — Churn Prediction Model

This repository contains the Part 3 deliverables for the churn prediction project.

## Files included
- `churn_model.ipynb` — complete modeling workflow from data loading to final model training, evaluation, and export.
- `model.pkl` — final saved sklearn pipeline for the churn prediction model.
- `metrics.json` — final test metrics and selected threshold.
- `error_analysis.md` — false positive / false negative analysis with customer examples.
- `model_card.md` — structured model card for stakeholders.
- `requirements.txt` — dependencies for reproducibility.

## How to run
1. Create and activate a Python environment.
2. Install dependencies:
```bash
pip install -r requirements.txt
```
3. Open `churn_model.ipynb` in Jupyter or VS Code and run all cells.

## Loading the saved model
The final model artifact is `model.pkl`. Load it in Python from the project root using:
```python
import joblib
model = joblib.load('model.pkl')
```
If your script is not running from the project root, provide the correct path to `model.pkl`.

## Notes
- This notebook uses the provided snapshot data file `rfm_modeling_snapshot.csv`.
- It does not use any post-snapshot data as input features.
- The model is evaluated on the held-out `test` partition from the dataset.
