## Efficacy of a Deep Learning System for Detecting Glaucomatous Optic Neuropathy Based on Color Fundus Photographs

##### **Z. Li, Y. He, S. Keel, W. Meng, R. T. Chang, M. He (2018)**

Summary: Goal of the paper is to assess the performance of a deep learning algorithm for detecting referable glaucomatous optic neuropathy based on color fundus photographs

### Methods
- 48,116 fundus photographs for the development and validation of a deep learning algorithm
- Study recruited 21 trained ophthalmologists to classify the photographs.  Referable GON was defined as a vertical c-d-ratio of >= .7. 
- The reference standard was made until 3 graders achieved agreement.  A separate validation dataset of 8000 fully gradable fundus photographs was used to assess the performance fo the algorithm
- 70k fundus photographs were downloaded by random sampling from the online dataset LabelMe
- Because large number and variation of images were collected, so a number of preprocessing steps were taken to normalize the images for variation in the databases.  Image pixel values were scaled to values in a range of 0 through 1, and then downsized to 299 x 299
- Local space average color was subtracted to solve the issue of color constancy, which indicates a human observed could see the color or an object with consistency
- Data augmentation was performed to enhance the dataset.  Techniques included random horizontal shifts of 0 to 3 pixels and random rotations of 90, 180, and 270
- Adopted a an Inception-V3 architecture, which is a CNN composed of 11 inception modules
- Minibatch GD of size 32 was used for training, with an Adam optimizer learning rate of .002 
- An experienced ophthalmologist reviewed misclassified 

### Conclusions
- The algorithm achieved an AUC of .986 with a sensitivity of 95.6% and specificity of 92%
- Power of the algorithm is in identifying referable GON based on the fundus photographs that were investigated
- Several reports of automated methods for the evaluation of glaucoma have been published recently.  This study differs in that it learns directly from the images.  This prevents the need for feature technology extraction which can introduce errors in in the localization and segmentation
- The NN in the study was built with a large dataset, dropout, and model regularization to help reduce the possibility of overfitting
- Although the results were insightful, a further study is expected to validate 
- False positive results create unnecessary referral and burden to the healthcare system.  In this study, almost all the FP cases were a result of abnormalities of the optic disc or retina not related to GON
