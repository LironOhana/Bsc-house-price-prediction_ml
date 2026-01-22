# House Price Prediction (ML + Flask)

End-to-end machine learning project for predicting property prices and serving the trained model via a simple **Flask web application**.
The repository includes exploratory analysis, a preprocessing/training pipeline, exported model artifacts, and an inference UI.

## What this project demonstrates
- **Data understanding & EDA** (feature distributions, missing values, relationships)
- **Reusable preprocessing** (scaling / transformations aligned between training and inference)
- **Model training & evaluation** (baseline → selected model)
- **Model packaging** (exporting artifacts as `.pkl`)
- **Deployment-style inference** (Flask app + HTML form)

## Repository structure
Recommended structure (clean, portfolio-friendly):

```
.
├── README.md
├── requirements.txt
├── notebooks/
│   └── house_price_eda.ipynb
├── src/
│   ├── madlan_data_prep.py
│   └── model_training.py
├── app/
│   ├── app.py
│   └── templates/
│       └── property_form.html
└── models/
    ├── trained_model.pkl
    ├── scaler.pkl
    └── scaler_y.pkl
```

> If your repo currently has files in the root (e.g., `api.py`, `data_analysis.ipynb`, `templates/`, `*.pkl`), you can move them into the folders above without changing the logic—only update file paths accordingly.

## How it works

### 1) Data preparation
`src/madlan_data_prep.py` contains functions/scripts for cleaning and preparing the dataset:
- handling missing values
- basic feature engineering
- ensuring the same feature order used later by the model

### 2) Model training
`src/model_training.py` trains a regression model and exports artifacts used for inference:
- `models/trained_model.pkl` — trained model
- `models/scaler.pkl` — feature scaler
- `models/scaler_y.pkl` — target scaler (used to inverse-transform predictions)

### 3) Inference web app
`app/app.py` loads the saved artifacts and exposes:
- `GET /` — a simple HTML form
- `POST /predict` — returns a predicted price rendered back on the form

## Run the web app locally

### Option A — run the Flask app directly
From the repository root:

```bash
pip install -r requirements.txt
python app/app.py
```

Then open:
- `http://127.0.0.1:5000/`

### Option B — set Flask environment variables (optional)
```bash
export FLASK_APP=app/app.py
export FLASK_ENV=development
flask run
```

## Re-train the model (optional)
If you want to re-train and re-generate the artifacts:

```bash
python src/model_training.py
```

After training completes, ensure the exported files exist under `models/`:
- `trained_model.pkl`
- `scaler.pkl`
- `scaler_y.pkl`

## Notes for portfolio reviewers
- The repo includes exported artifacts so the app can run **without** re-training.
- The notebook documents exploratory analysis and the rationale behind preprocessing/model choices.
- The inference code applies the **same preprocessing** used during training to avoid train/inference mismatch.

## Potential next improvements
- Add input validation and user-friendly error messages in the web form
- Add a `Dockerfile` for reproducible deployment
- Add model monitoring / logging of predictions (without collecting sensitive info)
- Add unit tests for preprocessing and inference
