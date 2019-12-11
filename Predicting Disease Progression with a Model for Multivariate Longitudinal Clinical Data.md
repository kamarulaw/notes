## Predicting Disease Progression with a Model for Multivariate Longitudinal Clinical Data

##### **J. Futoma, M. Sendak, C. B. Cameron, K. Heller**

### Definitions
- **CKD**: Characterized by a slow and typically symptomless loss of kidney function over time that results in complications that can cause poor health, premature death, increased health service utilization, and excess economic costs
 
### Abstract 
- Accurate prediction of the future trajectory of a disease is an important challenge in personalized medicine and population health management.  However, many complex chronic diseases exhibit large degrees of heterogeneity, and furthermore there is not always a single readily available biomarker to quantify disease severity
- Paper proposes a novel probabilistic generative model for multivariate longitudinal data that captures dependencies between multivariate trajectories of clinical variables
- This is done by using a Gaussian process based regression model for each individual trajectory, and builds off ideas from latent class models to induce dependence between their mean functions
- Authors develop a scalable variational inference algorithm that we use to fit our model to a large dataset of longitudinal electronic patient health records
- The model performs a lot better than a lot of the other state of the art algorithms currently on the market
- 

### 1- Introduction 
- Patients with multiple chronic conditions are among both the most expensive and highest utilizers of healthcare services.  However, predicting future disease trajectory is an extremely challenging problem.  One difficulty is the many underlying sources of variability that can drive the different potential manifestations of the disease
- One big challenge is the fact that observations are irregularly sampled, synchronous, and episodic, precluding the use of many time series methods developed for data regularly sampled at discrete time intervals
- The large degree of missing data, especially when modeling multivariate longitudinal data, also presents complications.  The task is made even more difficult when the primary data source is the EHR rather than a curated registry
- In order to be clinically relevant the prediction methods will need to be flexible enough to accommodate the inherent limitations of operational EHR data, dynamically update predictions as more information is collected and scale seamlessly to the massive size of modern health records
- A person’s kidney function can be approximated by their estimated glomerular filtration rate (eGFR), which is calculated using a common clinical laboratory test (serum creatinine or cystaatin C) and demographic information (age, sex, and race)
- Despite the fact that CKD can be easily identified using simple eGFR-based lab criteria, it is often not recognized and providing optimal care for CKD patients is notoriously difficult
- Aim of paper is to develop a flexible, broadly applicable statistical model for multivariate longitudinal data.  Ideally such a method would not only accurately model related time-varying clinical variables, but also leverage information from them to improve prediction of the disease trajectory of interest.  For instance, in our particular application to CKD, while eGFR is the primary biomarker of interest, prediction of other labs can also be clinically useful
- Authors accomplished this by utilizing a hierarchical latent variable model that captures dependencies between multivariate longitudinal trajectories
- The stochastic variational inference algorithm that the authors developed to fit the model scales well to the large dataset, and our model’s predictions have significantly lower error than a state-of-the-art univariate method for predicting disease trajectories

### 2- Related Work
- Tangri and et al. (2011) is a commonly referenced Cox proportional hazards model for predicting time to kidney failure
- Within the stats/ml communities, Markov models of many varieties are frequently used to generate dynamic predictions, e.g. autoregressive models, HMMs, and state space models
- However, the methods stated above are generally only applicable in settings with discrete, regularly-spaced observation times, and in most applications the data consists of a single set of multivariate time series, not a large collection of sparsely sampled multivariate time series 
- Section mentions a lot of different papers with techniques that were used to deal with the problem, or similar problems

### 3 - Proposed Multivariate Disease Trajectory Model
- The proposed hierarchical latent variable model jointly models each patient’s multivariate longitudinal data by using a GP for each individual variable, with shared latent variables inducing dependence the mean functions
- The model reduces to the method presented in “Schulam and Saria (2015)” in the univariate setting

### 4 - Inference
- The computational problem associated with fitting our model is estimation of the posterior distribution of latent variables and model parameters given the observed data.  As is common with complex PGMs, exact computation of the posterior is intractable and requires approximation to computer.  To this end, the authors developed a mean field variational inference algorithm to find an approximation to the posterior
- VI converts the task of posterior inference into an optimization problem of finding a distribution q in some approximating family of distributions that is close in KL divergence to the true posterior.  The problem is equivalent to maximizing what is known as the evidence lower bound (ELBO) 

#### Variational Approximation
- Section goes over a lot of theory that will be more understandable after going through the 10-708 notes on variational methods

#### Solving the Optimization Problem
- Authors used the automatic differential package autogrid in Python to compute exact gradients in order to optimize the lower bound, since the lower bound has an analytic closed-form expression
- At each iteration of the algorithm, we optimize the local variational parameters using exact gradients
- To accomplish this we use stochastic optimization, a commonly used method for variational inference

### 5 - Empirical Study

#### Dataset 
- The dataset contains laboratory values from about 45,000 patients with stage 3 CKD or higher extracted from the Duke University Health System EHR
- Authors started with an initial cohort of about 600,000 patients that had at least one encounter in the health system in the year prior to Feb. 1, 2015.  This included inpatient, outpatient, and emergency department visits over a span of roughly 20 years
- From this, the authors filtered to patients who had at least five recorded values for serum creatinine, the laboratory value used to create eGFR
- Finally the authors selected patients that had stage 3 CKD or higher, which is indicative of moderate to severe kidney damage and defined as two eGFR measurements less than 60 mL/min separated by at least 90 days
- Authors also chose to model five related lab values that have important clinical significance for CKD.  
  - The first, **serum albumin**, is an overall marker of health and nutrition.  
  - The second, **serum bicarbonate**, can indicate acid accumulation from inadequate acid elimination by the kidney.  
  - The third, **serum calcium** , can indicate improperly functioning kidneys if levels are too high.  
  - The fourth, **serum phosphorus**, can indicate phosphorus accumulation due to inadequate elimination by the kidney, and is associated with cardiovascular death and bone disorders
  - Finally, **urine albumin to creatine ratio** is a risk factor and cause of kidney failure 
- While all 44,519 patients have at least five eGFR measurements, there are 884, 242, 78, 16159, and 4321 patients who have no recorded values for the other labs, and the median number of measurements for each lab is 14, 8, 11, 12, 3, and 4, respectively
- As a final preprocessing step, since the recorded eGFR values are extremely noisy and eGFR is only a valid estimate of kidney function at steady state, we take the mean of eGFR readings in monthly time bins for each patient

#### Evaluation
- After learning a point estimate for the global model parameters during training, they are held fixed.  Then, an approximate posterior over the local latent variables is learned for each patients in the held-out test set
- Predictions about future lab values are made by drawing samples from the approximate posterior predictive
- Methods are compared to the mere method of (Schulam and Saria (2015)), trained independently to each of the 6 labs.  For each test patient, we learn their parameters three times, using data up until times t = 1, t = 2, and t = 4, each time recording the mean absolute error of predictions for each lab in future time windows.  These values are then averaged over all patients in the test set, and finally averaged over 10 cross validation folds to produce

#### Results
- Current clinical practice for managing care of CKD patients uses clinical judgement alone to predict future disease status.  Incorporation of a model such as the one in this paper to predict eGFR trajectory and other labs would provide a useful tool for providers to assess the risk of future decline in kidney function

### 6 - Discussion
- In this paper, the authors proposed a flexible model for multivariate longitudinal data, and applied it to disease trajectory modeling in CKD.  The model yields good performance on the task of predicting future kidney function and related lab values
- Although the work is a promising early work for developing ML models from EHR data and applying them to real clinical tasks, many inherent limitations to working with operational EHR data must be overcome in order for the methods to be widely used in practice
- Diabetes and cardiovascular disease are frequently co-morbid with CKD
- Given that much of the data recorded in the EHR is in the form of administrative billing codes, future work should incorporate these into the models as well, perhaps in an unsupervised fashion 
