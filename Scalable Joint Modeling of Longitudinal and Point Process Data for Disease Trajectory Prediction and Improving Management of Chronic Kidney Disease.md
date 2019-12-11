## Scalable Joint Modeling of Longitudinal and Point Process Data for Disease Trajectory Prediction and Improving Management of Chronic Kidney Disease

##### **J. Futoma, M. Sendak, C. B. Cameron, K. Heller**

### Definitions
- **Accountable Care Organizations (ACOs)**: are organizations that bear financial responsibility for the quality and total cost of healthcare services provided to a defined population of patients.  They need personalized prediction tools that identify individual patients in their populations at greatest risk of having poor clinical outcomes 
- **Mean Field Theory**: studies the behavior of large and complex stochastic models by studying a simpler model.  Such models consider a large number of small individual components
- **Posterior probability**: the conditional probability that is assigned after the relevant evidence or background is taken into account
- **Prior Probability**: probability distribution that expresses one’s beliefs about this quantity before some evidence is taken into account
- **Stochastic Optimization**: optimization methods that generate and use random variables.  Stochastic optimization methods generalize deterministic methods for deterministic problems
- **Variational methods**: a general class of approximation techniques with wide application throughout applied mathematics.  these methods are particularly useful when applied to highly-coupled systems
- **Point Process**: a collection of mathematical points randomly located on some underlying mathematical space such as the real line, the Cartesian plane, or more abstract spaces
 
### Abstract 
- A major goal in personalized medicine is the ability to provide individualized predictions about the future trajectory of a disease
- Moreover, for many complex chronic diseases, patients simultaneously have additional co-morbid conditions.  Accurate determination of the risk of developing serious complications associated with a disease or its co-morbidities may be more clinically useful than prediction of future disease trajectory in such cases
- The paper proposes a novel probabilistic generative model that can provide individualized predictions of future disease progression while jointly modeling the pattern of related recurrent adverse events
- Model is fit by using a scalable variational inference algorithm, and then fitting the algorithm on a large dataset of longitudinal electronic patient health records

### 1- Introduction 
- Most organizations lack the necessary capabilities they need, but with the widespread adoption of electronic health records (EHRs), much of the data needed to build such tools are being collected during the course of routine medical care
- In order to be clinically useful, such tools should be flexible enough to 
  1. accommodate the limitations inherent to operational EHR data
  2.  Update predictions dynamically as new information becomes available
  3. Scale to the massive size of modern health records 
- Authors collaborated with DukeConnectedCare, the ACO affiliated with the Duke University Health System, to develop predictive tools for CKD
- Most patients with advanced CKD pass away from cardiovascular complications before the onset of kidney failure.  This article focuses on tw common types of adverse cardiovascular events: heart attacks and strokes
- Paper develops a joint model that flexibly captures the eFGR trajectory of CKD progression, while simultaneously learning the association between disease trajectory and cardiovascular events.  Approach is formulated as a hierarchical latent variable model 

### 2 - Proposed Joint Model for Electronic Health Records

#### Electronic Health Records
- The EHR contains a large quantity of longitudinal patient data.  The vast majority of the data are unstructured, contained within free-text notes and reports.  Structured data include demographics, diagnosis and procedural codes, orders, lab results, and objective clinical observations (such as vital signs and various nursing assessments)
- In contrast to diagnosis codes, which capture clinicians’ subjective diagnostic impressions, lab tests provide objective clinical data.  A single medical encounter may include dozens, hundreds, or (in the case of hospitalizations) thousands of discrete lab test results
- Identifying and grouping relevant lab test results can be difficult due to lack of standardization and changing conventions over time
- Identifying and grouping relevant lab lab rest results can be difficult due to lack of standardization and changing conventions over time
- The model is agnostic to the particular algorithm used to identify clinical events.  After cleaning and transforming the raw EHR data, we obtained a longitudinal set of eGFRs for each patient, and dates of CVA and AMI diagnoses

#### Proposed Model
- The proposed hierarchical latent variable model jointly models longitudinal and point process data by creating different submodes for each type of data, with shared latent variables for each patient including dependencies between their two data types

#### Longitudinal Submodel
- Authors used a recently proposed model for disease trajectory for the longitudinal sub-model, that was shown to be extremely flexible and accurate at modeling continuous functions of disease progression
- Given the set of latent variables for patient I, the longitudinal variables are conditionally independent
- The model assumes each observed longitudinal value is a normally distributed random variable containing a population component, a subpopulation component, an individual component, and a structured noise component

#### Point Process Submodes
- Authors chose to model the times (vector u) that a [erspm has am adverse event as a Poisson process
- A common choice for the rate function from related literature in survival analysis corresponds to the hazard function from the Cox proportional hazards model.  Decision was made based on simplicity and also computational efficiency  

### 3 - Related Work
- There is a rich literature, mostly from biostatistics, on joint models typically for longitudinal data and TTE data with right censoring
- Within the medical literature, there have been numerous studies on predicting adverse events such as kidney failure, death, or cardiac events in patients with CKD; for instance, but almost all of the models developed are Cox proportional hazard models for TTE data, or logistic regression
- The paper closest to the work of the authors in application is [Perotte et al., 2015], which explores using time-series models to predict a TTE (progression from CKD stage III to stage IV)

### 4 - Inference Algorithms
- As with most complex probabilistic generative models, the computational problem associated with fitting the model is estimation of the posterior distribution of latent variables and model parameters given the observed data
- Exact computation of the posterior is intractable, and requires approximation to compute
- Variational methods transform the task of posterior inference into an optimization problem.  The optimization problem posed by variational inference is to find a distribution q in some approximating family of distributions that is close in KL divergence to the true posterior 
- The goal of our variational algorithm is to learn optimal variational parameters for each individual, as well as a point estimate for the model parameters.  In practice, we optimize the Cholesky decompositions for the covariance matrices

#### Solving the Optimization Problem
- In traditional settings for variational inference, the objective function is iteratively optimized by maximizing the variational parameters associated with each latent variable or parameter, holding the rest fixed
- To optimize the global parameters the authors employ stochastic optimization 

### 5 - Empirical Study
- The dataset was comprised of longitudinal and cardiac event data from 23,450 patients with stage 3 CKD or higher within the university health system 
- Method for creating dataset 
  1. Started off by creating an initial cohort of roughly 600,000 patients that had at least one encounter in the health system prior to 2/1/2015.  This was based on all types of encounter in the system, including inpatient, outpatient, and emergency department
  2. Filter the patients who had at least ten recorded values for serum creatine (the laboratory value required to calculate eGFR)
  3. Filter patients that had Stage 3 CKD or higher, indicative of moderate severe kidney damage (defined as two eGFR measurements) less than 60 mL/min separated by at least 90 days
  4. Finally, since the recorded eGFR values are extremely noisy and eGFR is only a valid estimate of kindest function at steady state, we take the mean of eGFR readings in monthly time bins for each patient.  
- Rapid fluctuations in acute illness are related to long term risk, but the authors have not yet explicitly incorporated this into the modeling
- After this preprocessing, on average each patient has ~23 eGFR readings
- In order to align the patients on a common time axis for each patient we fix t = 0 to be observed 
- For the experiments, the authors used ten fold cross val with training sets of 21,105patient records and test sets of 2,345 records.  Authors fit separate join models for CVA events and AMI events

#### Evaluation Metrics
- For the longitudinal sub-model, we compute the MSE and the MAE for predictions about held-out eGFR values 
- For the point process submodes, we view the problem of predicting whether any event will occur in a given future time window as a binary classification problem.  We end up reporting the AUROC and the AUPR as evaluation metrics for each binary classification task

#### Hyperparameters
- Because the model was trained jointly with the point process submodes we do not in general learn the same model parameters, since the parameters for the learned trajectories also influence by the event data
- For the point process model, the authors compare against two standard baselines.  The first is a simple Cox PH model from SA, where we s

### 6 - Discussion
- This paper has proposed a new joint model for longitudinal and point process data, and applied it to disease trajectory modeling and prediction of adverse events in patients with CKD.  The authors developed the first variational inference algorithm for this call of models, allowing us to fit our model to a large set of longitudinal patient data that is over an order of magnitude the size of datasets used by related methods
- The authors found that the model yields good performance on the tasks of predicting future kidney function and predicting cardiovascular events
- Model is good, but in reality there are many limitations to EHR data.  The data quality is often poor, complicated by inaccurate, inconsistent, or missing information.  EHR at a single organization may fail to capture the full patient story and all relevant outcomes of interest, which seems obvious
- Authors hope to expand work by adding other vitals data
- Jointly modeling multiple event processes will allow us to learn correlations between different types of events.  More flexible models, particularly for the event processes, should improve model performance for instance using Gaussian Process’, for instance using Gaussian Process modulated Poisson processes, or Hawkes processes instead of employing the proportional hazards of summation as we do In this work
