## Predicting Cardiovascular Risk Factors from Retinal Fundus Photographs using Deep Learning

##### **M. Zaharia, M. Chowdhury, M. J. Franklin, S. Shenker, I. Stoica (2010)**

Summary: Using models trained on data from 284,335 patients, and validated on two independent datasets of 12,026 and 999 patients, the authors predict cardiovascular cardiovascular risk factors not previously thought to be present or quantifiable in retinal images, such as age (3.26 years), gender (.97 AUC), smoking status (.71 AUC), HbA1c (within 1.39%), systolic blood pressure (within 11.23 mmHg) as well as major adverse cardiac events (.70 AUC).  Authors also show that the models used distinct aspects of the anatomy to generate the individual predictions

### Definitions  
- **MACE**: Major Adverse Cardiovascular Events

### Notes 
- Risk stratification is key to identifying and managing groups at risk for cardiovascular disease, which remains the leading cause of death globally
- Availability of cardiovascular disease risk calculators is fairly wide spread, but there are many efforts to improve risk predictions
- Coronary artery calcium is one example in which additional signals from imaging have been shown to improve risk stratification 
- Most current cardiovascular risk calculators are some combination of the following variables: age, gender, smoking status, blood pressure, body mass index (BMI), glucose, and cholesterol levels.  But often times these parameters are missing
- Authors propose to see if signals for risk can be extracted from the retinal images, which can be obtained quickly, cheaply, and non-invasively in an outpatient setting

### Model  
- Study employed the Inception-v3 architecture
- Preprocessed the images for training and validation, and trained the network following the same procedure as Gulshan et al, but for multiple predictions simultaneously
- Study had a lot of params (22 million), so early stopping criteria was employed
- To evaluate the model performance: 
  - For continuous predictions: (age, systolic and diastolic blood pressure, hba1c), the authors used MAE
  - For binary classification: (gender, smoking status) the authors used AUC 
  - For multiclass: authors used Cohen’s kappa

### Statistical Analysis
- Employed bootstrapping to assess the statistical significance of the results  

### Results
- MAE for predicting the patient’s age was 3.26 years, versus baseline 7.06 in the UK BB validation set.  For the EyeP the MAE was 3.42 vs. baseline 8.48 
- Predicted age vs. actual age was have a fairly linear relationship across datasets
- Given that retinal images alone were sufficient to predict several cardiovascular risk factors to varying degrees, the authors reasoned that the images could be correlated directly with cardiovascular events, which led to the authors training a model to predict the onset of MACE within 5 years
- MACE prediction results were only available for the UK BB (631 events occurred within 5 years of of retinal imaging—150 of which were in the clinical validation set).  Despite the limited # of events model achieved an AUC of .70 from retinal fundus images alone, compared to the AUC of .72 for the European SOCRE risk calculator
- Authors used soft attention to identify the anatomical regions that the algorithm might be using to make predictions
  - Models trained to predict age, smoking, and SBP primarily highlighted the blood vessels 
  - Models trained to predict gender primarily highlighted the optic disk
  - Models trained to predict primarily highlighted perivascular surroundings
  - Models trained to predict diastolic blood pressure and BMI were non-specific (suggesting signals might be dispersed throughout the image)

### Dataset 
- Retinal fundus images from 48,101 patients from UK Biobank, and 236,234 patents from EyePACS
- Models using 12,026 patients from UK Biobank and 999 patients from EyePACS.  
  - Mean age: 56.9 +/- 8.2 on UK BB (predominantly Caucasian)
  - Mean age: 54.9 +/- 10.9 on EyePACS (predominantly Hispanic, hba1c available for 60% of data)
- EyeP consisted mostly of diabetic patients so hba1c was higher than average (8.2 +/- 2.1%)
- Because the lack of an established baseline for predicting these features from retinal images, we use the average value as the baseline for continuous predictions (e.g age)
