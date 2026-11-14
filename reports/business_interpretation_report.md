# Business Interpretation Report - Employee Attrition

## Key message
About 16% of employees leave. The final model (Logistic Regression) ranks employees by leaving risk with ROC-AUC 0.81.
At the default threshold it catches 68% of actual leavers (32 of 47 in the test set), but only 38% of employees it flags actually leave
(52 false alarms), so it is best used to **prioritise conversations**, not to make automatic decisions.

## What drives attrition (permutation importance)
Most influential features: OverTime, YearsSinceLastPromotion, NumCompaniesWorked, MaritalStatus, JobSatisfaction, BusinessTravel.
Read these alongside the EDA charts (`outputs/eda_overview.png`) and SHAP plot (`outputs/shap_summary.png`) to see the direction of each effect
(for example, whether overtime or low income push risk up).

## Recommended actions (verify against the SHAP/EDA direction before presenting)
1. Review overtime policy and workload for high-risk groups.
2. Check pay competitiveness and promotion gaps for low-income / long-time-in-role employees.
3. Run stay interviews with the top-risk decile each quarter.
4. Prefer a lower threshold (higher recall) if retention conversations are cheap; raise it if HR capacity is limited (see threshold table).

## Cost trade-off
A missed leaver (false negative) costs recruiting + training + lost productivity; a false alarm costs one manager conversation. Because FN cost >> FP cost, recall is prioritised over precision.

## Ethics & caution
Do not use Gender, Age or MaritalStatus to take adverse action; audit for bias. Use predictions to offer support, not to penalise employees.
