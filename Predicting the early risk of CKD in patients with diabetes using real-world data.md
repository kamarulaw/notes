## Predicting the early risk of CKD in patients with diabetes using real-world data
##### **L.C. Cheng, Y.H. Hu, S.H. Chiou (2017)**

### Definitions  
- 

### Abstract
- Diagnostic procedures, therapeutic recommendations, and medical risk stratifications are based on dedicated, strictly controlled clinical trials
- However, a plethora of real-world medical data exists, whereupon the increase in data volume comes at the expense of completeness, uniformity, and control
- Here, a case-by-case comparison shows that the predictive power of a real-world data-based model for diabetes-related CKD outperforms published algorithms, which were derived from clinical study data

### Notes
- Controlled clinical trials are currently the gold standard of medical decision making.  These trials are usually conducted in an ideal setting with a preselected population
- The outcomes inferred from the expansion of the original scope of clinical study data therefore do not necessarily reveal the optimum pathways in terms of efficacy and effectiveness for real world populations 
- Authors carried out a direct comparison of algorithms derived from clinical data and real-world data and focused on the medically, socially, and economically important example of quantifying the risk of CKD as a microvascular long-term complication of diabetes
- Age, BMI, glomerular filtration rate as well as the concentrations of creatinine, albumin, glucose, and hemoglobin were selected as important predictors on the basis of a data-driven and medical selection
- Using these seven features authors deliberately chose logistic regression as the teaching method rather than a black box approach to allow for medical interpretation 
- The AUC of the algorithm was .7937 when applied to the overall independent validation data
- Although age is considered one of the major factors in kidney function, a log regression algorithm purely taught on age delivers an AUC of only .712 
- The use of real world data and in particular the inclusion of incomplete or erroneous data in the training set for the algorithm is a major difference compared with algorithms based on clinical study data
- The imputation of missing data provides a typical example of predictive analytics in RWD cohorts, whereas imputation is a delicate mean in a clinical study setting
- Authors speculate that it is the diversity of the RWD that makes the prediction algorithm better suited for generalization 

### Data
- The data extracted from the IBM Explores database for the investigation described herein originated from 522,416 people newly diagnosed with diabetes
