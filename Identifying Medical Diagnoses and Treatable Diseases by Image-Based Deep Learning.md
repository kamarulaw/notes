## Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning

##### **Lots of authors (2018)**

Summary: Authors use transfer learning on an OCT dataset in order to diagnose age-related macular degeneration and diabetic macular edema.  In addition they provide a more transparent and interpretable diagnosis by using soft attention.  

### Definitions
- **Drusen**: Yellow deposits under the retina.  Made up of lipids, a fatty protein.  Likely do not cause AMD, but having druse increases a person’s risk of developing AMD

### Notes 
- In the study, the authors sought to develop an effective transfer learning algorithm to process the medical images to provide an accurate and timely diagnosis of key pathology in each image.  The primary illustration of this technique involved OCT images of the retina, but the algorithm was also tested in a cohort of pediatric chest radiographs to validate the generalizability of the technique across multiple imaging modalities
- OCT imaging is now a standard of care for guiding the diagnosis and treatment of some of the leading causes of blindness worldwide: including AMD and DME
- Visualization of individual retinal layers is impossible with clinical examination by the human eye or by color fundus photography
- Initially obtained ~ 207k OCT images, 108k images (37k w/ CNV, 11k w/ DME, 9k w/ druse, and 51k normal ones) from 4.7k patients passed initial initial image quality review and were used to train the AI system The model was tested with 1,000 images (250 from each category) from 633 patients
- In a multi-class comparison between the eye pathologies listed above the authors achieved an accuracy of 96.6%, with a sensitivity of 97.8% and a specificity of 97.4%
- ROC curves were generated to evaluate the model’s ability to distinguish urgent referrals (CNV or DME) from drusen and normal exams.  AUC was 99.9% 
- Also trained a “limited model” classifying between the same four categories but only using 1,000 images randomly selected from each class during training to compare transfer learning performance using limited data compared to results
- The ROC curves distinguishing urgent referrals (i.e distinguishing images with choroidal neovascularization or diabetic macular edema from normal images had an AUC of 98.8%)
- Binary classifiers were also implemented to compare CNV/DME/Drusen from normal images using the same datasets in order to determine a breakdown of the model’s performance
- The classifier distinguishing CNV from normal images achieved an accuracy of 100%, with a sensitivity and specificity of 100%
- DME vs. Normal: 98.2% accuracy, 96.8% sensitivity, 99.6% specificity, 99.87% AUC
- Drusen vs. Normal: 99% accuracy, 98% sensitivity, 99.2% specificity, 99.96% AUC

### Methods
- OCT images were acquired using the Spectralis OCT from Heidelberg.  They were selected from retrospective cohort of adult patients from the Shiley Eye Institue 
- They searched all EMR medical record databases for diagnoses of CNV, DME, Drusen, and normal to initially assign images
- OCT examinations were interpreted to confirm a diagnosis, and referral decisions were made thereafter (“urgent referral” for diagnoses of CNV or DME, “routine referral” for drusen and “observation only” for normal)
- Using TF the authors adapted an Inception V3 architecture pertained on the imagent dataset.  Retraining consisted of initializing the convolutional layers with loaded pertained weights and retraining the final, softmax layer to recognize classes from scratch
- Attempts at “fine-tuning” the convolutional layers by unfreezing and updating the pretraine weights on the medical images decreased performance, most likely due to overfitting
- Training of all layers was performed by SGD in batches fo 1,000 images per step using Adam optimizer w/ a LR of .001
- Training on all categories was run for 10,000 steps, or 100 epochs, since training of the final layers will have converged by then for all classes
