# Prediction of Hospital Readmission and Gender-Biased Prediction Errors with Logistic Regression and Random Forest Models

**QUESTION**

To what extent can logistic regression and random forest predict early readmission among hospital encounters involving patients with diabetes discharged home, and is there gender bias in their prediction errors?

**DATA SOURCE**

In order to answer this question, I used the following dataset: Beata Strack, Jonathan P. DeShazo, Chris Gennings, Juan L. Olmo, Sebastian Ventura, Krzysztof J. Cios, and John N. Clore, “Impact of HbA1c Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records,” BioMed Research International, vol. 2014, Article ID 781670, 11 pages, 2014. The dataset contains clinical care at 130 US hospitals, from 1999 to 2008. Information was extracted for encounters that: 
* were an inpatient encounter (were admitted into the hospital)
* were diabetic (any kind of diabetes that was entered into the system as a diagnosis)
* the length of stay was at least 1 day and at most 14 days
* got laboratory tests during it
* got administered medications during it

The data contains 50 attributes: encounter_id, patient_nbr, race, gender, age, weight, admission_type_id, discharge_disposition_id, admission_source_id, time_in_hospital, payer_code, medical_specialty, num_lab_procedures, num_procedures, num_medications, number_outpatient, number_emergency, number_inpatient, diag_1, diag_2, diag_3, number_diagnoses, max_glu_serum, A1Cresult, metformin, repaglinide, nateglinide, chlorpropamide, glimepiride, acetohexamide, glipizide, glyburide, tolbutamide, pioglitazone, rosiglitazone, acarbose, miglitol, troglitazone, tolazamide, examide, citoglipton, insulin, glyburide-metformin, glipizide-metformin, glimepiride-pioglitazone, metformin-rosiglitazone, metformin-pioglitazone, change, diabetesMed, **readmitted**.

**METHOD**

I use the **readmitted** variable in the dataset, which tell us if patients were readmitted within the 30 days or not. What I try to make the model predict is therefore whether the patient will be readmitted within 30 days or not, by flagging the top 10% that are at risk of being readmitted. The predictors I kept to help train the model are those that didn't have an excessive amount of missing values so that the model would have enough data to train on. 

Since I have 2 models I want to compare, I decided to split the data as 60% for training, 20% for validation and 20% for testing. I pre-processed the data using the training set, and then used the validation data to compare both models and select the optimal parameters for each one. Finally, the test set was used to check the robustness of my findings.

What I make the models do is assign a predicted probability of readmission to each encounter. The highest-risk 10% overall are the 10% that have the highest predicted probability. To check prediction errors across models, I compute both the false-negative and false-positive rates, which essentially compares the flagged encounters with encounters that were actually readmitted. To answer the gender-bias question, I compare the earlier rates across men and women (where gender is listed), where my confidence intervals resample patients, while keeping the fitted predictions and flags fixed. 

**RESULTS**

I find that the models have mild predictive ability: they capture approximately only 24% of early readmissions within 10% of encounters, missing most readmissions (they aren't flagged by the model). Although the random forest is slightly more precise, this is a very small and almost negligible improvement relative to logistic regression. Gender audits for both models found a negative false-negative gap and a positive false-positive gap. This means that among encounters that actually resulted in early readmission, male encounters were more likely to go unflagged, and that among those that did not result in readmission, female encounters were more likely to be incorrectly flagged. However, after computing confidence intervals, I find that the only significant result is the false-negative rate gap computed for the random forest model. Therefore, I cannot establish that the model is unfair. 

**LIMITATIONS**

Firstly, this is an old dataset from almost two decades ago, and from US hospitals, so the results I found may not be the same results as results I might find using a more recent dataset in another country. Therefore, these results should not be generalised to other settings.

Secondly, I may not have used enough predictors, or predictors that would have actually been more useful when training the model. Due to some variables having too many missing values, using them might have led to an under-trained model. However, I could have tested the model using additional variables such as the diagnoses (1, 2, 3). 

Thirdly, the confidence intervals that I decided to compute only reflect patient sampling uncertainty, not the full uncertainty from training and model selection. 
