# Evaluation Report - Employee Attrition Prediction

## 1. Data & method
- IBM HR Analytics dataset, 1470 employees, 30 usable features; attrition rate 16.1% (imbalanced).
- Stratified 80/20 train/test split (random_state=42); preprocessing inside a Pipeline to avoid leakage.
- Imbalance handled with class weights (`balanced`) and XGBoost `scale_pos_weight` = 5.19.
- Tuning: GridSearchCV / RandomizedSearchCV, 5-fold stratified CV, scoring = ROC-AUC.

## 2. Classification results (test set; CV = 5-fold on train)
| Model               | Stage    |   Accuracy |   Precision |   Recall |    F1 |   ROC_AUC |   CV_AUC_mean |   CV_AUC_std |
|:--------------------|:---------|-----------:|------------:|---------:|------:|----------:|--------------:|-------------:|
| Decision Tree       | Baseline |      0.779 |       0.304 |    0.298 | 0.301 |     0.584 |         0.582 |        0.024 |
| Decision Tree       | Tuned    |      0.731 |       0.289 |    0.468 | 0.358 |     0.677 |         0.733 |        0.036 |
| Logistic Regression | Baseline |      0.752 |       0.349 |    0.638 | 0.451 |     0.803 |         0.827 |        0.028 |
| Logistic Regression | Tuned    |      0.772 |       0.381 |    0.681 | 0.489 |     0.812 |         0.831 |        0.029 |
| Random Forest       | Baseline |      0.837 |       0.485 |    0.34  | 0.4   |     0.791 |         0.81  |        0.038 |
| Random Forest       | Tuned    |      0.837 |       0.485 |    0.34  | 0.4   |     0.797 |         0.811 |        0.034 |
| XGBoost             | Baseline |      0.854 |       0.583 |    0.298 | 0.394 |     0.776 |         0.802 |        0.041 |
| XGBoost             | Tuned    |      0.786 |       0.389 |    0.596 | 0.471 |     0.789 |         0.829 |        0.025 |

Best tuned model by ROC-AUC: **Logistic Regression** (AUC 0.812, Recall 0.681, Precision 0.381, F1 0.489).
Best hyperparameters: `{'C': 0.1, 'penalty': 'l2'}`.

Confusion matrix of the best model: TN=195, FP=52, FN=15, TP=32.

## 3. Threshold analysis (Logistic Regression)
|   Threshold |   Precision |   Recall |    F1 |
|------------:|------------:|---------:|------:|
|         0.1 |       0.185 |    0.957 | 0.31  |
|         0.2 |       0.223 |    0.936 | 0.361 |
|         0.3 |       0.274 |    0.851 | 0.415 |
|         0.4 |       0.325 |    0.787 | 0.46  |
|         0.5 |       0.381 |    0.681 | 0.489 |
|         0.6 |       0.466 |    0.574 | 0.514 |
|         0.7 |       0.564 |    0.468 | 0.512 |

## 4. Feature importance
Top drivers (permutation importance): OverTime, YearsSinceLastPromotion, NumCompaniesWorked, MaritalStatus, JobSatisfaction, BusinessTravel.
Figures: `outputs/roc_curves.png`, `confusion_matrices.png`, `feature_importance.png`, `permutation_importance.png`, `shap_summary.png`.

## 5. Regression (optional): JobSatisfaction
| Model             |   MAE |   RMSE |     R2 |   CV_R2 |
|:------------------|------:|-------:|-------:|--------:|
| Linear Regression | 0.975 |  1.155 | -0.054 |  -0.056 |
| Ridge             | 0.973 |  1.151 | -0.049 |  -0.049 |
| Random Forest     | 0.969 |  1.138 | -0.025 |  -0.042 |
| XGBoost           | 0.987 |  1.162 | -0.069 |  -0.105 |

## 6. Limitations
- Only 294 test rows (47 leavers), so metrics vary by split; CV std is reported.
- The dataset is a fictional IBM sample; results may not transfer to a real company.
- JobSatisfaction is ordinal (1-4); regression is a simplification.
