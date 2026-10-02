# Dementia-predictions
Predicting dementia from clinical and MRI data.


A machine learning project exploring which clinical and brain imaging measures are linked to dementia, and how well they can predict it.

## Question
Can we predict whether an older adult has (or will develop) dementia using their age, education, memory test score and brain measurements?

## Data
The [OASIS longitudinal dataset](https://www.kaggle.com/datasets/jboysen/mri-and-alzheimers) from Kaggle: MRI and clinical data from 150 adults aged 60 to 96, scanned on two or more visits.

## Approach
- **One visit per person:** only each participant's first visit was used, so the same person couldn't appear in both training and test data.
- **Label:** participants who were demented, or who converted to dementia during the study, were labelled as 1.
- **Missing values:** a small number of missing SES and MMSE values were filled with the median.
- **Avoiding data leakage:** CDR (Clinical Dementia Rating) was excluded from the model because it is essentially a clinical rating of dementia itself. ASF was excluded because it is almost perfectly correlated with eTIV (r = -0.99).
- **Models:** logistic regression (baseline) and random forest, evaluated with 5-fold stratified cross-validation.

## Key findings from exploring the data
- **MMSE** (a memory and thinking test) showed the clearest difference: the dementia group scored lower (median ≈ 27 vs 29 to 30), though the groups overlapped.
- **Brain volume** was lower in the dementia group (≈ 0.73 vs ≈ 0.75), consistent with atrophy caused by neuron and synapse loss.
- **Age** was almost identical between groups (median ≈ 75), so differences in brain volume and MMSE are not simply explained by age.
- **Education** was lower in the dementia group (≈ 13.5 vs ≈ 16 years), consistent with the cognitive reserve hypothesis. Notably, socioeconomic status was not linked to dementia, despite being strongly linked to education.

## Results (5-fold cross-validation)

| Model | Accuracy | Recall (dementia) | ROC AUC |
|---|---|---|---|
| Logistic regression | 0.75 | 0.65 | 0.84 |
| Random forest | 0.79 | 0.72 | 0.82 |

The random forest caught more dementia cases, which matters most in a medical setting, while both models ranked risk similarly well. MMSE was by far the most important feature, followed by brain volume and skull size.

## Limitations
- Small dataset (150 people), so results vary depending on how the data is split.
- "Converted" participants were healthy at their first visit, which makes them hard to distinguish and lowers recall.
- Median imputation was done before splitting the data, which may cause slight leakage.
- Random forest feature importance can favour features with many unique values, so rankings should be interpreted with caution.
- This is an exploratory project, not a diagnostic tool.

## Next steps
- Use permutation importance to check feature rankings.
- Use region-specific measures such as hippocampal volume, which is affected early in Alzheimer's disease.
- Use participants' changes across visits to predict who will convert to dementia.
- Tune model settings and test on a larger dataset.

## Tools
Python, pandas, NumPy, matplotlib, seaborn, scikit-learn, Google Colab
