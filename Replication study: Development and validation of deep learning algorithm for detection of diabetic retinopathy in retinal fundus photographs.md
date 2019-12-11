## Replication study: Development and validation of deep learning algorithm for detection of diabetic retinopathy in retinal fundus photographs

https://arxiv.org/pdf/1803.04337.pdf

##### **M. Voets, K. Mollersen, L. A. Bongo (2018)**

Summary: Paper attempts to replicate the experiments from the original study JAMA paper.  Authors achieved a significantly lower AUC on the respective datasets and made claims that the original study missed details regarding hyper-parameter settings for training and validation.  

### Definitions
- **Refractive Error**: The shape of your eye does not bend light correctly   

### Notes 
- Paper claims that to prove that proposed methods are general enough to apply in practice, the used methods should be evaluated on many datasets with public source code.  The paper makes an assessment on the reproducibility of the deep learning methods used in the original study
- 

### Algorithm 
- TensorFlow was used in the training and evaluation of the algorithm
- Deep neural network that combines a ResNet and a soft-attention architecture
- Attention maps suggested that the foveal region was one of the most important areas used by the algorithm to make this prediction, though other regions also contribute to this prediction
- Preprocessed the images for training and validation, and trained the NN
- Used an early stopping criterion to help avoid overfitting and to terminate training when the model performance (such as MAE) once the tuning dataset stopped improving 
- To further improve results, they averaged 10 NN models that were trained on the same data
- Optimized the meaning absolute error (MAE) to evaluate model performance for predicting refractive error.  Calculate the R^2, but this was not used to select the operating points for model performance.  - Further examined the algorithms by seeing how frequently the algorithm predictions fell within a given error margin 
- Assessed statistical significance of these results non-parametric bootstrap procedure.  By repeating this sampling and evaluation 2,000 times we obtain a distribution of the performance metric.  Further assessed statistical significance by performing hypothesis testing using a one-tailed binomial test for the frequency of the model’s prediction lying within several error margins for each prediction
- To visualize the most predictive eye features, they integrated a soft-attention layer into their neural network

### Results
- Algorithm had a a mean absolute error of .56 diopters for estimating spherical equivalent on the UK Biobank dataset, and .91 diopters for the AREDS dataset
- The baseline expected MAE was 1.81 diopters and UK Biobank, and 1.63 for AREDS

### Datasets 
- UK Biobank: ongoing observational study that recruited 500,000 participants between 40 and 69 across the UK between 2006 and 2010.  Completed lifestyle questions, and underwent a series of health measurements 
- Age-Related Eye Disease Study (AREDS): Clinical trial in the United States that investigated the natural history and risk factors of ager-related macular degeneration and cataracts.  Enrolled participants between 1992 and 1998 and continued clinical follow-up until 2001 at 11 specialty clinics
