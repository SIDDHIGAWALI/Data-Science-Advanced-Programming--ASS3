# Employee Attrition Prediction Models

Classification (Logistic Regression, Decision Tree, Random Forest, XGBoost) to predict `Attrition`, plus an optional regression task (`JobSatisfaction`).
Evaluation: Accuracy, Precision, Recall, F1, ROC-AUC. Improvement: hyperparameter tuning, 5-fold CV, feature importance, SHAP.

## Structure
```
data/WA_Fn-UseC_-HR-Employee-Attrition.csv   IBM HR Analytics dataset (1470 rows)
notebooks/employee_attrition_models.ipynb     Model notebook (deliverable 1)
reports/evaluation_report.md                  Evaluation report (deliverable 2, auto-generated)
reports/business_interpretation_report.md     Business interpretation report (deliverable 3)
outputs/                                      Charts and result CSVs
```

## How to run
```bash
python -m venv venv
source venv/bin/activate            # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Option A: open and run interactively
jupyter notebook notebooks/employee_attrition_models.ipynb     # Run > Run All Cells

# Option B: run everything from the command line (~1-2 min)
cd notebooks
jupyter nbconvert --to notebook --execute --inplace employee_attrition_models.ipynb
```
Running the notebook regenerates `outputs/` and both reports in `reports/`.

## Results (test set, tuned models)
See `outputs/classification_results.csv`. Best model by ROC-AUC is Logistic Regression (about 0.81), with the tree ensembles close behind. Top drivers include OverTime, YearsSinceLastPromotion, NumCompaniesWorked and MaritalStatus.
Regression on JobSatisfaction gives R² of about 0, which is an honest negative result for this dataset.

## Submit to GitHub
```bash
git init
git add .
git commit -m "Employee attrition prediction project"
git branch -M main
git remote add origin https://github.com/<your-username>/employee-attrition-prediction.git
git push -u origin main
```
Create the empty repo on github.com first, then submit its link.
