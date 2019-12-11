## Multi-Trajectory Models of Chronic Kidney Disease Progression

##### **P. Burckhardt, D. S. Nagin, R. Padman**

### Definitions
- **End-Stage Renal Disease (ESRD)**: a medical condition in which a person’s kidneys cease functioning on a permanent basis leading to the need for a regular course of long-term dialysis or a kidney transplant to maintain life
 
### Abstract 
- A better understanding of the progression of CKD and its complications is needed to address what is becoming a major burden for health-care systems worldwide.  
- Utilizing a rich data set consisting of the EHRs of more than 33,000 patients from a leading community nephrology practice in Western PA, the authors applied group-based trajectory modeling (GBTM) in order to detect patient risk groups and uncover typical progressions of CKD and related comorbidities and complications

### 1 - Introduction
- By modeling biomarkers not only for CKD, but also complications typically linked to it, the authors aim to obtain a fuller, multi-dimensional picture of its progression.  Specifically the authors used eGFR along with other appropriate lab-based biomarkers for its complications
- Historically, CKD was assessed via patient’s serum creatinine levels.  However, serum creatinine is not a good measure of kidney function, so nowadays labs report eGFR which can be computed using characteristics such as age, gender, and race
- With that said, eGFR by itself is not sufficient for guiding decision-making on the care of kidney patients.  A multi-dimensional approach is required since CKD patients tend to have numerous comorbidities.  In fact, a large fraction of patients suffering from CKD do not progress to the later stages of the disease but die prematurely due to these comorbidities and complications
- Thus, any treatment should take into account the contemporaneous progression of CKD and its varying complications at differing levels of severity that patients experience
- This poses a major challenge since care coordination amongst various medical professionals is required.  Discerning disease patterns via interpretable and accessible statistical models has the potential to alleviate the communication challenges between these stakeholders
- At their core, GBTMs are an example of *finite mixture models*.  More specifically they are mixtures of regression models applied to longitudinal data, where the likelihood of a time series for an individual is assumed to be a mixture of linear models that include both an explicit time variables as well as other covariates
- The data set spans four years of patient data and includes diagnoses, lab results, and patient characteristics
  - After restricting the data to all patients who were diagnosed with CKD stage III during the covered time span, all transplant patients were removed as their trajectories differ significantly from the rest of the problem.  The final cohort consisted of 1,944 individuals 
- Authors fit a single, joint multi-group trajectory-model for five biomarker time series, and uses an appropriate model selection procedure to identify a parsimonious and interpretable model.  Covariates such as patient characteristics (age, weight, gender, race, etc.) and binary indicators on whether the patient suffers from the comorbidities of diabetes and hypertension are included in addition to the biomarker time series
- The constructed model is an extreme simplification, but could prove to be useful for better screening of patient populations containing high-risk individuals

### 2 - Data
- Total number of unique patients is 33,832.  The data set contains information about patient characteristics (age, gender, weight, and height) as well as all of their lab measurements with associated dates
- The patient population is split almost evenly among female and male patients, and the median age of the cohort is about 70 years
- About half the patients have a diagnosis of CKD Stage III while the remaining are in more advanced stages of CKD
- Historically, serum creatinine levels have been used as a marker for CKD.  As kidney function deteriorates, blood levels of creatinine typically rise.  Tracking eGFR has replaced the creating test as a more reliable means to detect early kidney damage.  
- For this study eGFR was calculated from serum creatinine levels using the CKD-EPI equation
- Creatinine values larger than 10 mg/ml were removed because these values were likely entered incorrectly
- Typical complications of CKD include Anemia, Secondary Hyperparathyroidism, Hyperphosphatemia, and Metabolic Acidosis.  The respective lab measurements of Hemoglobin (HGB), parathyroid hormone (PTH), phosphate (PO4), and carbon dioxide (CO2) can be used as markers for the considered complications 
- After data cleaning and processing, 1944 of the patients diagnosed with CKD stage III were used for model fitting 

### 3 - Methods

#### Single Trajectory Model
- Delves into theory that I can come back to later (done)

#### Multi-Trajectory Model
- Delves into more theory that I can come back to if time permits

#### Model Predictions
- Delves into more theory that I can come back to if time permits
- The aforementioned procedure yields a multi-response model, which fits trajectories for all five biomarkers and incorporates time-stable covariates, namely demographic variables and indicators for the existence of diabetes and hypertension
- New patients can be classified using the posterior probabilities of group membership, which can be computed using Bayes’ rule  

### 4 - Results
- The number of groups was determined using BIC as the model selection criterion, as was the order of the polynomial terms and the inclusion of time-stable covariates
  - Using BIC in the model search is supposed to help build a sparse but at the same time sufficiently large model to accurately reflect the data at hand
- This paper makes use of more traditional methods in trying to deal with the problem at hand
- It is known that CKD prevalence increases drastically with age and it has been speculated that elderly people might be more susceptible to CKD
