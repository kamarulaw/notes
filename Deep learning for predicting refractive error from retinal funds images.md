## Deep learning for predicting refractive error from retinal funds images

##### **A. Varadarajan, R. Poplin, K. Blumer, C. Angermueller, J. Ledsam, R. Chopra, P. A. Keane, G. S. Corrado, L. Peng, D. R. Webster (2017)**

Summary: RE can be treated by treatments as simple as prescribing sunglasses.  The authors trained a deep learning algorithm to predict RE from funds photographs from participants in the UK Biobank cohort.  The model used the “attention” model to identify features that are correlated with refractive error.  Attention maps suggested that the foveal region was one of the most important areas used by the algorithm to make this prediction, though other regions also contribute to this prediction.  The ability to estimate RE with high accuracy from retinal fundus photographs is a new concept.  

### Definitions
- **Refractive Error**: The shape of your eye does not bend light correctly   
- **Mean Absolute Error (MAE)**: Measure of difference between two continuous variables
- **Bootstrap Procedure**: From the validation set of N instances, sample N instances with replacement and evaluate the model on this sample
- **Spherical Equivalent**: A summary metric for refractive error, can be calculated using the formula spherical power + .5 * cylindrical power.  Spherical equivalent was available for both 

### Notes 
- One of the most common causes of visual impairment worldwide.  Very treatable, but most people live in low-income countries
- Deep learning can also characterize signals that medical experts can not typically extract from images alone, such as age, gender, blood pressure, and other cardiovascular health factors
- Study trained a deep learning model to predict refractive error from funds images using two different datasets.  Used attention techniques to visualize and identify new image features associated with the ability to make predictions

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
