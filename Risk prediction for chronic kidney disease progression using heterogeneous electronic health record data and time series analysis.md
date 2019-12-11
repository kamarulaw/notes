## Risk prediction for chronic kidney disease progression using heterogeneous electronic health record data and time series analysis

##### **A. Perotte, R. Ranganath, J.S. Hirsh, D. Blei, N. Elhadad**

### Abstract 
- As adoption of electronic health records continues to increase, there is an opportunity to incorporate clinical documentation as well as laboratory values and demographics into risk prediction modeling
- The authors develop a risk prediction model for chronic kidney disease progression from stage III to stage IV that includes longitudinal data and features drawn from clinical documentation
- The study consisted of primary-care clinic patients who had at least three visits prior to January 1, 2013 and developed CKD stage III during their documented history
- Time series analysis (Kalman filter) and survival analysis (cox proportional hazards) were combined to produce a range of risk models.  The models were evaluated using concordance, a discriminatory statistic 
- The paper concludes with an understanding that a risk prediction model that takes longitudinal laboratory test results and clinical documentation into consideration can predict CKD progression from stage III to stage IV more accurately than three models that do not take all of these variables into consideration 
- Keywords: risk prediction, electronic health records, topic modeling, survival analysis

### Definitions
- **Generative model**: a statistical model of the joint probability distribution
- **Discriminative model**: a model of the conditional probability of the target, given an observation x
- **Topic model**: a type of statistical model for discovering the abstract “topics” that occur in a collection of documents
- **Deterministic Algorithm**: algorithm which will always produce the same output given a a particular input
- **Latent Dirichlet Allocation (LDA)**: a generative statistical model that allows sets of observations to be explained by unobserved groups that explain why some parts of the data are similar
- **Concordance Index**: Quantifies the quality of rankings, is the standard performance measure for model assessment in SA.  In contrast, the standard approach to learning the popular proportional hazard model is based on Cox’s partial likelihood
- **Gibbs Sampling**: An algorithm for obtaining a sequence of observations which are approximated from a specified multivariate probability distribution, when direct sampling is difficult.  Commonly used as a means of statistical inference, especially Bayesian inference.  It is a randomized algorithm and is an alternative to deterministic algorithms for statistical inference such as the EM algorithm
- **Kalman Filter**: An algorithm that uses a series of measurements observed over time, containing statistical noise and other inaccuracies, and produces estimates of unknown variables that tend to be more accurate than those based on a single measurement alone, by estimating a joint prob distribution over the variables for each timeframe


 
### Background and Significance
- Current progression risk models for CKD largely rely on commonly obtained laboratory or vital sign data
- It is a very serious condition, but often times people with the disease go unnoticed or end up receiving suboptimal treatment
- Earlier identification and more accurate prognostication of these patients using better risk prediction models may improve outcomes by facilitating timelier initiation of appropriate therapies, monitoring, and specialty referral
- While using readily available data for risk prediction might simplify computation , this might be at the expense of more robust prognostication
- With increasing EHR adoption, clinical documentation is a potentially rich, underutilized source of information that can aid in clinical decision support
- Two aspects of the EHR in particular present an opportunity for automated risk prediction
  1. The presence of longitudinal data
  2. The rich information conveyed in the clinical narratives
- Automated information extraction from narrative using NLP is an active field of research and has shown promising results in estimating diseases risk, increasing appropriate cancer screening, and identifying post-operative complications, influenza, inflammatory bowel disease, pneumonia, and heart failure
- The goal of the present study was to incorporate longitudinal clinical documentation as a novel feature in disease progression risk calculation
- This study aims to investigate the following questions
  1. How can longitudinal data from the patient record be incorporate into risk modeling?
  2. How can EHR data be incorporated into a risk modeling paradigm, focusing on two critical data elements of the EHR, laboratory data and clinical data?
- Family of models investigates to represent patient documentation are unsupervised, and the problem of risk modeling as a survival analysis task; thus demonstrating that these methods are capable of producing two valuable outcomes: 
  1. an interpretable set of variables associated with risk of CKD progression at the population level 
  2. an actionable model to estimate risk of progression for individual patients
- 

### Methods

#### Variables 

##### Independent Variables
- Independent Variables: Age, Gender (M/F), eGFR, Laboratory Test-based factors and biases (24 variables), Text-based factors and biases (60 variables)
  - Demographic variables were age and gender.  Ethnicity and race were omitted because their value in the EHR is left as the default value “Other” for most of the patients
- All other variables, as they are time-dependent, were binned by month and the mean value was used if multiple values were observed within a month
- To handle missing values the most recent value was carried forward for each bin prior to stage III onset, mean values from the development dataset were used instead
- The free text notes were preprocessed using the probabilistic topic modeling technique called latent Dirichlet allocation (LDA).  LDA is a prob mixed membership model often applied to text analysis
- LDA models a corpus as a set of documents, each of which is represented by a probability distribution over a set of K topics.  In this analysis, K = 50.  Each topic, in turn, is represented by a probability distribution over all the terms in the vocabulary
- In the generative model for LDA, each document is generated by drawing K topics according to the weighting associated with the document and drawing N terms, according the weighting over terms associated with each topic
- A Gibbs sampling approach was employed to identify the appropriate parameters.  More detailed discussion of this model and methods for inference can be found in the literature

#### Statistical Analysis

##### Survival Analysis
- The following five multivariate Cox PH models were fit on the development dataset: eGFR and Recent Laboratory Tests (RLT) models and three time series models.  All models demographic variables and all other values.  At the time of stage III onset.  All variables were standardized to have zero mean and unit variance

##### eGFR Model
- This model included EGFR value, age, and gender data at stage III on-set as independent variables

##### RLT Model
- This model considered as independent variables the values for the 19 variables included in the study prior to stage III onset

##### Time series-based models
- In the eGFR and RLT model, a patient’s data immediately prior to CKD stage III onset is considered, but all other previous values are ignored
- Laboratory test results can be very variable depending on time of day, recency of meals, etc. 
- To address this variability and simultaneously make risk predictions that incorporate longitudinal patient data, the authors combined TSA and SA to construct these risk production models
- The TS model used in these experiments is a variant of the well-known Kalman filter (otherwise known as a linear dynamical system), a TS model for noisy temporal observations which is designed to infer a set of smooth latent (unobserved) states from which the noisy observation is based.  In this case the observations include the laboratory test results and the clinical documentation are the noisy results
- The model is employed such that the latent state inferred at the time of CKD three onset provides a representation of a patient at that time, rather than the observed independent variables
- After learning these model parameters, the latent state at the time of CKD and the per-patient bias term will be used as the final output of these models.  Parameter learning was achieved through an approximation technique known as mean field variational inference
- For each of the three time series models considered, a Kalman filter was fit using the development cohort data and applied to the validation data using the previously determined parameters.  When applied to the validation data, only observations prior to the onset of stage III CKD are used to estimate the state of the Kalman filters

##### Prediction model validation and Experimental setup
- The coefficients and the baseline hazard function for each of the Cox PH models were held constant and applied to the validation dataset.  Concordance, a measure of discriminatory power equivalent to the area under the ROC curve was used to evaluate model performance on the validation dataset
- Pairwise comparisons between the models were tested using the corrected resampled t-test and P-values were adjusted for multiple comparisons with the Holm-Bonferroni correction

### Results

#### Cohort Description
- Development set included 2617 patients, and the validation set included 291 patients.  There were 307 stage IV events in the development set and 35 events in the validation set

#### eGFR model 
- Low eGFR at onset of stage III CKD was associated with high risk of progression (P < .001)
- Younger age was associated with increased risk of progression (P < .005)
- Male gender (the variable for gender was encoded as 1 for male and 0 for female gender) was also associated with increased risk of progression (P = .005)

#### RLT Risk Model
- In the RLT model, elevated levels of BUN (P < .001), creatinine (P < .001), triglycerides (P = .0061), and urine protein (quantitative, P = .019; qualitative, P < .001), as well as decreased levels of hematocrit (P = .043), hemoglobin (P = .0034), calcium (P = .033), and serum protein (P = .017) were associated with increased risk of CKD progression.  When introducing laboratory variables at the time of onset in the survival model, age and gender are not associated with risk of progression anymore

#### Kalman Filter Risk models
- In the TKF Risk model, topics associated with a higher risk of progression are shown and contained terms related to heart failure (P < .001), diabetes (P < .001), and dialysis (P = .028).  Mention of these topic words throughout the course of the patient records indicated high risk for progression

#### Model Performances in the Validation Cohort
- The concordance ended up being the highest for the LTKF model (.849), followed by the LKF model (.836), RLT model (.819), eGFR model (.779), and TKF model (.733)

### Discussion
- Authors developed and performed an internal validation for five models for CKD progression from stage III to stave IV.  The models leverage different types of variables - demographic , laboratory and/or clinical documentation data that are collected routinely during the course of clinical care as part of the EHR 
- Authors found that text is a valuable predictor for CKD progression and that the use of tS models to characterize patient state can substantially improve predictive accuracy for progression
- Most developed classifiers use readily obtainable information, including age, demographics, and laboratory data.  Hence, laboratory data, comorbidities, and occasional vital signs are the sole dimensions of contemporary CKD classifiers (eg. bmi, serum calcium , bicarbonate, phosphate, or cholesterol)

### Conclusions
- A risk prediction model that takes longitudinal laboratory test can significantly
