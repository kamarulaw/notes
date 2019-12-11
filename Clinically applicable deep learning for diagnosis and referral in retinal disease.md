## Clinically applicable deep learning for diagnosis and referral in retinal disease

##### **(2018)**

Summary: 

### Notes 
- In ophthalmology, the widespread availability of optical coherence tomography (OCT) has not been matched by the availability of expert humans to interpret scans and refer patients to the appropriate clinical care

#### Results
- Authors developed an architecture in the context of OCT imaging for ophthalmology.  The approach was tested for patient triage in a typical ophthalmology clinical referral pathway, comprising more than 50 common diagnoses for which OCT provides the definitive imaging modality
- OCT is a three-dimensional volumetric medical imaging technique analogous to three-dimensional ultrasonography but measuring the reflection of near-infrared light rather than sound waves at a resolution for living human tissue of ~ 5 micrometers
- Automated diagnosis of a medical image, even for a single disease, faces two main challenges: technical variations in the imaging process, and patient-to-patient variability in pathological manifestations of disease.  Existing DL approaches tried to deal with all combinations of these variations using a single end-to-end black-box network, thus typically requiring millions of labeled scans
- By contrast, the author’s framework decouples the two problems (technical variations in the imaging process and pathology variants) and solves them independently.  A deep segmentation network creates a detailed device-independent tissue-segmentation map.  Subsequently, a deep classification network analyses this segmentation map and provides diagnoses and referral suggestions
- The segmentation network uses a a three-dimensional U-Net architecture to translate raw OCT scan into a tissue map with 15 classes including anatomy, pathology and image artifacts.  It was trained with 877 clinical OCT scans (Topcon 3D OCT, Topcon) with sparse manual segmentations.  
- Only approximately three representative lives out of the 128 slices of each scan were manually segmented.  This sparse annotation procedure allowed us to cover a large variety of scans and pathologies with the same workload as approximately 21 dense manual segmentations
- This sparse annotation procedure allowed us to cover a large variety of scans and pathologies with the same workload as approximately 21 dense manual segmentations
- To construct the training set for this network, authors assembled 14,844 OCT scan volumes obtained from 7,621 patients who were referred to the hospital with symptoms suggestive of macular pathology.  These OCT scans were automatically segmented using our segmentation network
- The resulting segmentation maps with the clinical labels built the training set for the classification network
- A central challenge in OCT-image segmentation is the presence of ambiguous regions, where the true tissue type cannot be deduced from the image, and thus multiple equally plausible interpretations exist.  To address this issue, authors trained not one but multiple instances of the segmentation network
- Each network instance creates a full segmentation map for the given scan, resulting in multiple hypotheses 
- Analogous to multiple human experts, these segmentation maps agree in areas with clear image structures but may contain different (but plausible) interpretations in ambiguous low-quality regions 
- These multiple segmentation hypotheses from the authors network can be displayed as a video, in which the ambiguous regions and proposed interpretations are clearly visible (see *Methods ‘Visualization of results in clinical practice’*)
- Framework uses an ensemble of five segmentation and five classification model instances
- The accumulated number of diagnostic errors does not fully reflect the clinical consequences that an incorrect referral decision might have for patients, which depends also on the specific diagnosis that was missed
- The framework demonstrated an area under the ROC curve that was over 99% for most of the pathologies (and over 96% for all of them)
- As with earlier evaluations, performance of the experts improved when they were provided also with the fundus image and patient summary notes
- This improvement was most marked in pathologies classed as “routine referral”, for example geographic atrophy and central serous retinopathy
- To evaluate the effect of a different scanning device type, authors initially fed the OCT scans from device type 2 into the framework, which was trained only on scans from device type 1.  The segmentation network was clearly ‘confused’ by the changed appearance of these structures and attempted to explain them as additional retinal layers.  Consequently, performance was poor with a total error rate for referral suggestions of 46.6%
- After retraining the system (adapted segmentation network and unchanged classification network) achieved a similarly high level of performance on device type 2 as on the original device

#### Methods
- To facilitate viewing of the results in routine clinical practice, the authors display the obtained three-dimensional segmentation maps as two-dimensional thickness maps overlaid on a projection of the raw OCT scan
- The thickness maps for all tissue types are displayed side-by-side in the interactive OCT viewer (Fig. 5b and Supplementary Video 1)
- In most common clinical scenarios, the algorithm will both provide the diagnosis with a high degree of certainty and highlight classical disease features (for example, ‘wet’ AMD; Supplementary Video 2)
- OCT scan sets containing severe artifacts or marked reductions in signal strength to the point at which retinal interfaces could not be identified were also excluded from the study (Supplementary Fig. 10)
