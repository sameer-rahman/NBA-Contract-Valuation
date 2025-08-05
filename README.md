# 🏀 NBA Contract Value Prediction (2024–25)

## TL;DR

This project uses machine learning to predict 2024–25 NBA player salaries based on 2023–24 performance data.  
By comparing predicted vs. actual contracts, we identify overpaid and underpaid players, evaluate team-level salary efficiency, and surface potential market inefficiencies in the league.

- Final model: **XGBoost** with **R² = 0.80** and **RMSE ≈ $5.85M**
- SHAP reveals top features: **eff_pts**, **MP**, **Age_sq**, **PTS**, **DRB**
- Visuals include player-level and team-level salary insights

---

## Overview

- **Goal**: Predict NBA contract values from player performance stats and analyze salary efficiency
- **Approach**: Clean and engineer features from base + advanced stats, train regression models, and interpret predictions
- **Output**: Predicted salary values, overpay gap, and interpretability visualizations

---

## Methods & Tools

- **Data Source**: Basketball Reference (2023–24 stats and 2024–25 contract data)
- **Preprocessing**:
  - Removed duplicates and `'2TM'` aggregate rows
  - Merged base stats, advanced stats, and salary data
  - Log-transformed contract targets
  - One-hot encoded player positions
  - Engineered features like `eff_pts`, `PTS_per_min`, `Starter_flag`, `Age_sq`
- **Models Used**:
  - XGBoost Regressor (**best model**)
  - LightGBM, CatBoost, Random Forest for comparison
- **Hyperparameter Tuning**: `RandomizedSearchCV` with 3-fold cross-validation
- **Evaluation Metrics**: R², RMSE, MAE (on actual dollar scale)

---

## Results

| Model                 | R² Score | RMSE ($)        |
|----------------------|----------|-----------------|
| **XGBoost Regressor** | **0.80** | **$5.85M**       |
| Random Forest         | 0.73     | $6.89M           |
| CatBoost              | 0.73     | $6.84M           |
| LightGBM              | 0.71     | $7.11M           |

- The XGBoost model outperformed others, explaining 80% of salary variance
- RMSE of ~$5.85M suggests predictions are typically within that range of actual contracts

---

## SHAP Analysis: Top Predictive Features

Using SHAP (SHapley Additive Explanations), we interpreted the model’s most impactful features:

- `eff_pts` – A custom stat capturing efficient scoring output (`PTS * eFG%`)
- `MP` – Minutes played, reflecting availability and coach trust
- `Age_sq` – Squared age, capturing nonlinear age effects
- `PTS`, `DRB`, `G`, `2P`, `TS%` – Core volume and efficiency stats
- `Starter_flag` – Binary flag for consistent starters
- `VORP`, `PER`, `AST`, `BPM`, `AST%`, `TOV%` – Advanced value metrics

This confirms that both traditional counting stats and advanced impact metrics are driving salary prediction.

---

## Visualizations

- **SHAP Beeswarm Plot** – Global feature importance and direction
- **Predicted vs. Actual Salary Scatterplot** – Color-coded by overpay/underpay
- **Top 10 Overpaid & Underpaid Players** – Based on model vs. actual contracts
- **Most Overpaid/Underpaid Player Per Team** – Bar chart with player labels
- **Team Salary Efficiency Rankings** – Based on total overpay gap
- **Overpay Gap Distribution** – Shows model residuals and skew

---

## Key Takeaways

- Performance and efficiency metrics explain most variation in contract value
- A few teams consistently overpay or underpay relative to player performance
- Several players stand out as market inefficiencies when comparing value to salary
- SHAP analysis validates feature engineering and model interpretability

---

## Limitations

- Does not include injuries, off-court value, contract structure, or qualitative traits
- Based on a single season — multi-year models could improve robustness

---

## Future Work

- Incorporate multi-year performance and injury history
- Add contextual contract data (incentives, extensions, trade clauses)
- Deploy as an interactive app or salary-value dashboard

---

## Repo Structure

data/
├── NBA 2023–24 Stats Data.csv
├── NBA Advanced Stats 2023–24 Season.csv
├── NBA Contract Data.csv

full_pipeline.ipynb # Complete analysis pipeline
README.md # Project overview
requirements.txt # Python dependencies

yaml
Copy code

> Note: `main.ipynb` is for local experimentation and is excluded via `.gitignore`.

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/your-username/nba-contract-value.git
cd nba-contract-value
2. Set up your environment
Create a virtual environment (optional but recommended) and install dependencies:

bash
Copy code
pip install -r requirements.txt
3. Run the notebook
Open full_pipeline.ipynb in Jupyter or VS Code and run all cells from top to bottom.
Ensure the data/ folder is populated with the three CSV files.