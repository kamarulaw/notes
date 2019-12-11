## Applying the Temporal Abstraction Technique to the Prediction of Chronic Kidney Disease Progression

##### **L.C. Cheng, Y.H. Hu, S.H. Chiou (2017)**

### Definitions  
- **Diabetes**: disease in which the body’s ability to produce or respond to the hormone insulin is impaired, resulting in abnormal metabolism of carbohydrates and elevated levels of glucose in the blood, and urine
- **Glomerulonephritis**: acute inflammation of the kidney, typically caused by an immune response
- **Hypertension**: Occurs when systolic or diastolic blood pressure is too high
- **End Stage Renal Disease (ESRD)**: according to the National Kidney Foundation, ESRD is defined as the GFR decreasing to less than 15 ml/min/1.73m^2
- **Stationary Process**: a stochastic process whose unconditional joint probability distribution does not change when shifted in time.  Consequently, parameters such as mean and variance also do not change over time
- **Augmented Dickey-Fuller test**: tests the null hypothesis that a unit root is present in a time series sample.  The alternative hypothesis is different depending on which version of the test is used, but is usually stationarity or trend-stationarity

### Abstract
- In clinical practice, the physical conditions of CKD patients are regularly recorded.  The data of CKD patients are recorded as a high-dimensional time-series.  Therefore, how to analyze these time-series data for identifying the factors affecting CKD deterioration becomes an interesting topic
- This study aims at developing prediction models for stage 4 CKD patients to determine whether their eGFR level decreased to less than 15ml/min/1.73m^2 (end-stage renal disease, ESRD) 6 months after collecting their final laboratory test information by evaluating time-related features
- A total of 463 CKD patients collected from Jan. 2004 to Dec. 2013 at one of the biggest dialysis centers in southern Taiwan were included in the experimental evaluation
- The authors integrated temporal abstraction techniques along with data mining methods to develop CKD progression prediction models
- The AdaBoost + CART model exhibited the most accurate prediction among the constructed models (Accuracy: .662, Sensitivity: .620, Specificity : .704, AUC: .715)
- A number of TA-related features were found to be associated with the deterioration of renal function
- TA-related features extracted by long-term tracking of changes in lab test values can enable early diagnosis of ESRD
- The models that make use of these features can facilitate medical personnel in making clinical decisions to provide appropriate diagnoses and improved care quality to patients with CKD

### Introduction
- The early symptoms of CKD are typically non-specific; they are similar to those of many other diseases.  CKD is often not diagnosed until more serious symptoms such edema and hamaturia occur.  When the kidney function drops to 5% below normal, the body cannot maintain normal metabolism and the patient enters end-stage renal disease (ESRD) 
- In clinical practice, the physical conditions of patients with CKD are regularly recorded and managed.  At each regular examination, medical personnel record detailed information relevant to the physical status of a patient with CKD such as biochemical blood test records, lifestyle and drug use habits
- Most studies have applied only cross-sectional data and statistical methods to construct prediction models of CKD progression.  Although cross-sectional data are accurate predictors in prediction models of CKD progression, information regarding the variation over time is ignored
- For example blood pressure can be measured several times during a specific period.  One of the easiest methods of presenting BP variations is through mean and stdev, but doing this results in the loss of a lot of critical information
- In the current study, effective CKD progression prediction models were developed by evaluating time-related features.  Specifically, two main tasks has to be accomplished
  1. The first task entailed extracting time-related features accurately reflecting the variation of variables for the TS data of patients with CKD
  2. The second task involved developing models for predicting CKD progression based on multiple machine learning techniques
- In this study, complete records of patients with CKD were collected from the EMR system in dialysis centers in southern Taiwan from Jan. 2004 to Dec. 2013.  The data included the patient’s basic information, medical history, medical history records, lab test records, and care and nutrition tracking records
- The variable selection and extraction were performed by consulting the relevant literature and domain experts 
- Multiple supervised learning techniques were used to develop the prediction models including C4.5, classification, regression tree (CART), and support vector machine (SVM).  Adaptive boosting (AdaBoost) was also used to improve the predictive performance of the models
- The results show that the inclusion of TA-related vairblaes enhances the performance of the prediction models 

### Background overview and literature review

#### Definition of CKD and its stages
- Early symptoms of CKD are generally difficult to detect; these symptoms do not appear until kidney function drops 30% below normal
- Experts worldwide have developed various equations for determining GFR, tailored to various countries and races.  Most experts use the Serum Creatinine measure along with other clinical options such as age, gender, and race.  This paper makes use of the eGFR given by the CKD-EPI
- Although medical staff can evaluate the decline in kidney function in patients by evaluating the eGFR, they cannot further understand related information regarding the progression of CKD if only the eGFR is clinically evaluated.  Therefore, how to construct a reliable prediction model and identify CKD patients who are at potential risk of deterioration has come an essential question in medical research

#### Factors and research affecting the progression of CKD
- Survival prediction models have been frequently used for analysis when predicting the risk of CKD progression, with two of the most common being the Kaplan-Meier model and the Cox model.  However , these two prediction models are based on incomplete information, implying that one can obtain only limited information from the analysis results
- In this study

#### Data mining in kidney research
- In recent years, data mining (DM) has been used for analyzing CKD, ESRD, and hemodialysis (HD) information as well as other related laboratory tests and clinical data
- Authors give examples of data mining techniques that have been attempted over the last decade

#### Temporal Abstraction
- TA focuses on the extraction of qualitative aspects of TS based on rules that are defined by clinical experts.  TA can be used to define different change types in TS data, such as trends, statuses , or other complex time-related attributes
- Generally speaking, temporal abstraction can be classified as basic TA and complex TA
  1. Basic TA is often used to detect numerical or symbolic time series, and the episode found is presented through a qualitative approach.  In particular, two types of TA were extracted: trend Was, which capture the increasing, decreasing, or stationary patterns in a numerical time series, and state TAs, which detect qualitative patterns corresponding to low, high, and normal values in a numerical or symbolic time series
  2. Complex TA is used to analyze the temporal relationship between interval series.  The temporal relationships investigated correspond to the 13 temporal operators, which include BEFORE, FINISHES, OVERLAPS, MEETS, STARTS, DURING, their corresponding inverse relations, and EQUALS.  

### Method
- The proposed CKD progression prediction models comprise the following modules: data processing and filtering, TA, and classification modules

#### Data collection and preprocessing
- Data from 2066 patient with CKD were collected from Jan. 2004 to Dec. 2013 at one of the biggest Kidney health centers in southern Taiwan
- In practice, collecting the data from patients with CKD at the early stages is difficult because the symptoms regarding CKD normally do not appear until patients have reached at least stage 3 or 4 CKD.  
- Therefore, most of patients with CKD at the early stages do not have corrine body examinations and to extract the values of Tea-related variables from these patients is infeasible
- Patients with the number of body examinations less than 4 were deleted from the experimental data set.  If the patients have more than 4 body examinations we only consider the first 4 results in generating the values of TA-related variables.  As a result, a total of 463 patients were included in the experimental evaluation.  Among them, 132 cases deteriorated to ESRD 6 months after the last date of their one-year-long lab data and 331 cases remained at stage 4
- The dependent variable represented whether patients with stage 4 CKD exhibited signs of ESRD (i.e stage 5 CKD) 6 months after collecting their final lab test information
- According to the National Kidney Foundation, ESRD is defined as GFR decreasing to less than 15 ml/min/1.73m^2
- In the EMR system, detailed information regarding each case, including demographic data, drug use, history, habits, laboratory tests, body checkup, and health education assessment, was recorded.  Categorical variables included items such as gender, whether the patient smoked or used herbal medicine in medical history, and health education.  These variables were constant over time and could be recorded directly
- Continuous variables were primarily the results, such as body checkup and lab tests; the values of these variables were recorded each quarter year
- In addition, the clinicians determined the standard values of every laboratory test item.  It is because most of the laboratory test values of CKD patients differ from those of a person without CKD.  The identification of abnormal laboratory test values may be the important factors in determining in determining the progression of CKD patients

#### TA module
- TA mechanisms can extract various types of changes, including state and trend, from time-series data
- Basic TA variables can be identified from numerical or symbolic time-series.  In particular, two types of TA variables were extracted: state TA variables, which detect qualitative patterns corresponding to low, high, and normal values in a numerical or symbolic time-series, and trend TA variables, which capture the increasing, decreasing, or stationary patterns in a numerical TS
- Each lab test item generated state TA variables according to the TA rules.  However, the numbers for each type of test item differed 
- To retrieve the value of a state TA variable, the average value of a lab test item at two adjacent time points need to be calculated and then mapped into the TA rules
- Trend TA variables were defined by checking the trend of the values between two adjacent time points.  The types of trend TA were divided into S (steady), I (increasing), and D (decreasing)
- Combining both the state and trend TA finally yields the value of the basic TA variable

#### Classification techniques
- We adopted several common supervised learning techniques, including C4.5, CART, and SVM to construct the prediction models.  In addition, AdaBoost, proposed by Freund and Schapire, was integrated to enhance the prediction performance of the proposed models

### Experimental Evaluation

#### Evaluation design and performance measurement
- The dataset containing the records of patients with CKD was imbalanced.  Specifically, 132 cases deteriorated to ESRD 6 months later and 331 cases remained at stage 4 in the experimental data set
- The author resampled the data set to avoid the class imbalance problem.  A total of 132 cases from the 331 patents with stage 4 CKD were randomly selected and integrated with all 132 ESRD patients into one balanced data set
- In addition, certain useful cases in the patients with stage 4 CKD may not have been selected during the resampling process, resulting in the loss of valuable information for classification.  Therefore, a random sampling technique was applied 30 times to construct 30 data sets 
- The predictive performance was measured by evaluating the accuracy, sensitivity, specificity, and the area under the curve of the receiver operating characteristic curves of each classification

#### Results
- Authors reported the mean and standard deviation of the results of the 30 generated data sets to simplify the explanation 
- The CART model exhibited the highest performance among all single classifiers and the integration of AdaBoost significantly enhanced there performance of the model
- The authors adopted four feature selection modules provided by WEKA to determine the factors influencing the deterioration of CKD.  Specifically, the average of the rank results obtained from Gini, ChiSquared, InfoGain, and GainRatio modules were used to rank the importance of factors.  The top 25 factors found in this study are summarized in Table 6
- According to the assessment results, gender was the most critical factor affecting the deterioration of CKD among the first 25 variables that exerted the greatest impact.  Age was also a major factor at stage 3
- Recording changes in kidney function over a long period can facilitate the process of determining the severity and progression of CKD (eg, Scr_4, BUN_4, Basic_34, Scr_3, BUN_Basic_23, BUN_3, BUN_Complex_1)

#### Conclusion
- In practice, the factors affecting CKD deterioration are extremely complex; therefore, to improve the prediction accuracy, we suggest that future studies could analyze other potential factors
