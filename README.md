# Patient-Hospitalization-Prediction
hospitalization risk prediction


**Input:** patients.csv - labels file. patients_to_score.csv - holdout test file

**Data and quality.** The labeled file 'patients.csv' has 49,680 rows but 46,000 unique patients, with 3,680 repeat rows and no conflicting outcomes. Hospitalization prevalence is 8.1% on the untouched test set. I standardized smoker/state categories, handled missing numeric values within a fitted preprocessing pipeline, and retained one row per patient because timestamps were unavailable. `discharge_summary_count` was excluded: its unusually strong association with the target and sharp train/score distribution difference suggested potential post-outcome leakage. Patient IDs and the target were never model inputs.

**Model.** I compared default logistic regression and histogram gradient boosting on a separate development split, selecting **Logistic regression** by development ROC-AUC. I fit the selected model on the combined 80% train+development set, then evaluated it once on the held-out 20% of unique patients. I did not tune hyperparameters.

**Held-out results.** ROC-AUC **0.778**, PR-AUC **0.390** (random-ranking baseline approximately 0.081), and Brier score **0.061**. In the highest-risk 10% of patients, observed hospitalization rate was **37.3%**, versus **8.1%** overall (4.6x enrichment). That group contained **46.3%** of all observed admissions.

**For a care team.** This model ranks people by their estimated likelihood of inpatient admission in the following 12 months. If outreach resources are limited, review the top-ranked patients first. A score is a planning aid, not a diagnosis or certainty about any individual. Before production, verify the prediction cutoff, test against future real-world patients, check calibration and group-level reliability, and agree on outreach capacity and success measures.

**Tools.** I used an AI assistant to help organize the notebook, draft code, and review methodological risks. The data diagnostics, model evaluations, and generated risk scores were checked by executing the code. Review and personalize this disclosure before submitting.

**Reproduce.** From the folder containing this notebook and both input CSVs, install `requirements.txt`, then run `jupyter nbconvert --to notebook --execute --inplace metriport_hospitalization_review.ipynb`. It regenerates `risk_scores.csv` and this write-up.
