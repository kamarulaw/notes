## Survival Analysis with Electronic Health Record Data: Experiments with Chronic Kidney Disease

##### **Y. Hagar, D. Alberes, R. Pivovarov, H. Chase, V. Dukic, N. Elhadad**

### Definitions  
- **CKD Stage III**: a moderate decrease in eGFR to the range of 30 - 59 mL/min/1.73m^2
- **CKD Stage IV**: eGFR in the range of 15 - 29 mL/min/1.73m^2
- **CKD Stage V (ESRD)**: eGFR below 15 mL/min/1.73m^2

### Abstract
- The analysis is based on the EHR data comprising almost two decades of clinical observations collected at New York-Presbyterian, a large hospital in New York City with one of the oldest electronic health records in the United States
- The SA surrounds around Bayesian multi resolution hazard modeling, with an objective to capture the changing hazard of CKD over time, adjusted for patient clinical covariates and kidney-related laboratory tests
- Special attention is paid to statistical issues common to all EHR data, such as cohort definition, missing data and censoring, variable selection, and potential for joint survival and longitudinal modeling, all of which are discussed alone and within the EHR CKD context

### 1 - Introduction
- The electronic health record (EHR) contains much data about patient care collected over time.  As such it presents a detailed and ubiquitous source of patient information
- While EHR data are not curated like traditional clinical research datasets, there is evidence that the patient record contains valuable information, which if used effectively, could help shorten delays in diagnosis and avoid severe consequences for patients
- Extracting meaningful information from the EHR is not a trivial step, however.  Because the data are collected with care as the primary goal, as opposed to research and clinical discovery  there are several challenges entailed in using the data directly in statistical analyses
- This paper explores the potential of using the EHR to extract information for predictive and explanatory statistical modeling of disease progression
- The key contributions of this article are (i) careful data preparation to mitigate some of the characteristics of EHR data, namely cohort definition, missing data, and censoring; (ii) the novel application of a survival analysis technique to EHR longitudinal data; and (iii) the comparison of two types of models: one based on patient demographics and healthcare process and one augmented with laboratory test variables
- The healthcare-based model uncovers valuable information for characterizing progression of disease; while the lab-augmented model identifies some key factors of disease progression in line with clinical literature for CKD.  

### 2 - Statistical Issues in the EHR Data
 - EHR data hold great potential for exploring long-term medical, epidemiological, public health, economic, and demographic questions and are rapidly becoming an ubiquitous and practical data source for both the purpose of guiding clinical practice and epidemiological research
- However, as with any non-controlled datasets, the processes under which EHR data are collected or generated are rich with structure that has to be carefully considered in statistical analyses
- EHR is especially complex as it represents a mixture of epidemiological, behavioral, and institutional processes, all of which may change over time, and do so depending on the outcome of interest
- Below we take a look at several of the major statistical issues in greater detail 

#### 2.1 - Generalizability
- An EHR population is often tied to a specific location, via the so-called ‘catchment area’ of the governing institution
- The size and sociodemographic makeup of the catchment area depend on the institutional infrastructure, and economic as well as administrative issues
- These issues, may manifest themselves in practice as differences in distributions of risk factors, such as race, socioeconomic status, and institutional and medical protocols, which could result in EHR-specific disease case severity and risk profile mixes.  When not accounted for, these differences could lead to analyses and results for the specific EHR population being non-representative of the population at large
- Complex examples include situations where prevalence or other aspects of the disease under study are linked to the EHR population characteristics in a confounded way, where confounders are unmeasured and potentially time-varying, such as relating healthcare patterns and the number of visits a subject has made to the doctor to disease severity

#### 2.2 - Cohort Definition
- The act of performing an EHR-based study requires an algorithm-driven extraction of a specific and well-defined EHR subpopulation; including all patients from the EHR is neither practical nor advisable.  This process requires decisions regarding inclusion and exclusion criteria, length of record, and amount and quality of data in the record.  The final cohort of patients is ideally well defined, and representative of the larger specific subpopulation it was chosen from
- Identifying patient cohorts from EHR data is an open research Problem.  For example, patients with CKD can be selected by their estimated glomerular filtration rate (eGFR; this variable is computed based on creatinine levels, a routine lab test) because eGFR is often the sole variable used to diagnose CKD.  For other conditions, however, selecting patients is more complex

#### 2.3 - Missing Data
- EHR data are generally collected over long time periods, which means that missing data rre one of the most important statistical issues to consider
- Missingness in each of the tree processes has a potential for biasing the results of statistical analyses 
- In many scientific contexts, missingness might be relatively benign and non-informative.  However, in the EHR context, the missingness can be informative and, more specifically, related to the patient’s disease status itself
- In these situations, postulating the mechanisms underlying the missingness, as with Bayesian approaches, and formally assessing robustness of results with respect to the implicitly defined missingness mechanisms, via subsampling and comparisons with other large EHR populations and clinical studies, is paramount

#### 2.4 - Censoring Data
- Real patients vary in healthcare seeking behaviors, both reactively and proactively.  This means that measurement pattern is sometimes related to health state.  However, it is unknown whether lack of measurement implies absence of the disease, a severe case of an unrelated morbidity, a severe case of the disease itself treated by a different healthcare provider, or is a reflection of a patient’s attitude toward healthcare
- In the TTE context, this means that censoring is a complex issue, where different types of censoring may occur for each patient, and cannot always be assumed to be a separate, random process
- A lack of measurement does not imply health, and the information in the record only informs the lower bound on the time to progression
- One way to alleviate the effect of this challenge is to focus on a patient population for which EHR has enough longitudinal and representative information
- This may be an attainable goal for EHRs from institutions with captive populations which do not tend to move (long-term residents), or where patients are observed for all care, as may be the case with inner city populations and community hospitals

#### 2.5 - Variable Selection 
- Automatic variable selection in the EHR setting is remarkably complex because of the inconsistency that comes up during the recording process
- While there exist advanced methods for variable selection catered to large data, none developed to date are especially equipped to deal with EHR-specific issues

#### 2.6 - Statistical Models: Joint Survival and Longitudinal Modeling
- A significant amount of EHR appeal lies in its ability to provide joint modeling of patient’s time-varying processes and disease progression
- Within a large EHR, considerable diversity can often be observed in the frequency, quality, and type of longitudinal measurements
- Many individual patients have a burst of measurements over a short period of time, while allowing years to pass without being seen by the provider
- One way to deal with irregular measurements would be to follow an aggregation method.  Alternatively, interpolation or latent variable modeling of covariate processes may provide a solution for imputing the missing measurements over time
- There is a history of modified interpolation working well in the EHR context
- Research on joint modeling for observed covariate processes and disease progression in the EHR context promises to be a fruitful area of research

### 3 - Case Study: CKD

#### 3.1 - Chronic Kidney Disease
- Prevalence of the disease is increasing dramatically, and its costing the nation billions of dollars
- Patients are recommended to be referred to a nephrologist when reaching stage 3, so that an appropriate treatment can be devised to slow down the decrease in kidney function
- If CKD is left untreated eGFR decreases in a somewhat linear fashion
- In the author’s study of CKD progression, they focused on the survival analysis of progression from stage III to stage IV

#### 3.2 - Dataset and Data Preprocessing
- The dataset was assembled from a single institution, NYP hospital, in New York City 
- While the EHR contains much data, the authors focused on three types of variables: demographic variables, healthcare variables, and lab test variables
- Healthcare variables such as number of visits, are particularly attractive in investigating EHR data, as their dynamics reflect a mixture of processes: the disease-related process, the healthcare process, and the patient process 
- Authors hypothesize that healthcare variables, as derived from the EHR, provide valuable insight into disease progression.  As for lab variables, they have been the primary types of variables investigated in disease progression thus far and as such made much sense to be included.  The use of other EHR variables such as diagnosis codes and medication orders for future work

##### 3.2.1 Patient Cohort Identification
- Starting pool of patients consisted of all patients who visited the outpatient clinic at NYP at least three times between 2006 and 2012
- A threshold of at least three clinic visits was chosen to ensure that enough information would be present in the patient records; while not pruning away too many patients the original number of patients with at least one visit to the clinic was 23751 patients
- Patients with a transplant or a diagnosis of HIV-AIDS were filtered out, as their kidney function is well known to decline in a predictable manner
- Patients who had any acute kidney failure (i.e a sudden severe drop of eGFR over a few days) were also filtered out of the cohort
- Because the definition of stage 3 CKD is based on variables routinely collected in the EHR for all patients, the most accurate fashion to select patients is based on their eGFR values, as opposed to the presence of a CKD diagnosis in the record
- Thus authors relied on eGFR calculations to select patients (namely the Modification of Diet in Renal Disease (MDRD) formula calculation).  The final patient cohort contained 1008 patients

##### 3.2.2 Variables
- In this article the authors focused on three types of variables even though there was more they could have selected from (demographic variables, laboratory variables, and healthcare variables) which are described as follows
  - Demographics: Authors selected gender and age.  Race is potentially useful, but overall poorly documented in the EHR
  - Laboratory Test Values: Based on a literature review of CKD progression and the input of our nephrologist on the team, the authors selected the following 17 laboratory tests to investigate in the models: 25-OH vitamin D, bicarbonate, blood urea nitrogen, calcium, ionized calcium, chloride, high-sensititivy C-reactive protein, hematocrit, hemoglobin, potassium, magnesium, microalbumin, creatinine ratio, sodium phosphorus, parathyroid hormone, triglycerides, and uric acid.  The names of these lab tests are not standardized, and as a result are often too granular to be used directly in data mining and statistical analysis.  The authors hired medical students to identify and map the different low-granularity data elements as stored in the EHR to each laboratory test, resulting in a time series for each type of test
  - Laboratory Test Indicators: To accommodate patients with multiple laboratory measurements, the authors calculated the average of each test in the year prior to stage 3 diagnosis.  The inclusion of these patients is important, because there is information in a missing test, as a test is ordered based on a variety of reasons
  - Healthcare: The authors experimented with different ways to define a visit.  
    - A visit of type 1 was defined as any date for which there is at least one lab measurement in the longitudinal record.  So the number of visits corresponds to the number of unique dates at which tests were taken.
    - A visit of type II was defined as any consecutive set of measurements within 3 days of each other

### 4 - Survival Analysis of CKD
- Survival analysis, TTE data analysis, lifetime data analysis, or reliability theory, all generically refer to a class of statistical models used to describe how the distribution of time to an event of interest changes as a function of certain factors in a given population 

#### 4.1 - MRH Estimator
- Bayesian multi resolution models have recently been adapted to hazard functions, and equipped to deal with censoring and truncation.  These models assume a binary partition tree structure for time-axis partition.  A Gamma prior is placed on the total cumulative hazard function over a finite time interval, and a set of Beta priors are used to describe the recursively defined partition parameters

- Rest of section has a lot of theory.  Will come back to if necessary

### 5 - Results

#### 5.1 - Experimental Setup
- The goal of the analysis is to identify variables associated with progression of CKD, as derived from our patient cohort.  Because of the nature of the EHR dataset, the authors experimented with different settings for the survival analysis and survival models.  Table 3 shows descriptive statistics of the different settings
- **Effect of left censoring**.  To investigate the effect of left censoring on the SA, the authors experimented with 3 sub cohorts from the dataset: all patients (n = 1008), patients with at least one lab measurement at least 3 months prior to CKD diagnosis (3m group, n = 639), and patients with at least one lab measurement at least 6 months prior to CKD diagnosis (6m group, n = 609)
- **Effect of right censoring**.  Out of the 1008 patients, 11.8% (120) had an observed transition from stage 3 to stage 4 CKD, 4.8% (n = 49) had an observed death, and the remaining patients (n = 839) were right censored.  To investigate the effect of right-censoring on the SA, the authors experimented with two alternative observation times for the right-censored patients: an UB observation time with a censoring date as the end of the data collection time for the entire dataset, and a LB observation time with censoring date defined as the time of last lab measurement for a given patient

#### 5.2 - Basic Healthcare Model
- 
