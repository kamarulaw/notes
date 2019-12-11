## Macular OCT Classification Using a Multi-Scale Convolutional Neural Network Ensemble

##### **R. Rasti, H. Rabbani, A. Mehridehnavi, F. Hajzadeh (2018)**

Summary: Due to the increasing use of retinal optical coherence tomography (OCT) imaging technique, a CAD system in retinal OCT is essential to a assist ophthalmologists in the early detection of ocular diseases and treatment monitoring.  This paper presents a novel CAD system based no a mult-scale convolutional mixture of expert (MCME) ensemble model to identify normal retina, and two common types of macular pathologies, namely dry age-related macular degeneration and diabetic macular edema.  The proposed MCME modular model is a data-driven neural structure, which employs a new cost function for discriminative and fast learning of image features by applying CNNs on multiple-scale sub-images.  

### Definitions
- **Pyramid (Image Processing)**: A type of multi-scale signal representation developed by the computer vision, image processing, and signal processing communities.  Image/signal is subject to repeated smoothing and subsampling
  - **Gaussian Pyramid**: Subsequent images are weighted down using a Gaussian average (Gaussian blur) and scaled down.  Each pixel containing a local average that corresponds to a pixel neighborhood on a lower level of the pyramid (commonly used in texture synthesis)
- **Texture Synthesis**: Process of algorithmically constructing a large digital image from a small digital 

### Notes 
- The main sensory region for this purpose is the macular which is located in the central part of the retina
- Macula healthiness can be affected by a number of pathologies, including age-related macular degeneration (AMD), and diabetic macular edema (DME)
- OCT is a non-invasive imaging technique, which captures cross-sectional images at microscopic resolution of biological tissues
- 3-D image interpretation is a time-consuming and tedious process for ophthalmologists 
- 1) in the preprocessing step, a graph-based curvature correction algorithm is used to remove the retinal distortions, and to yield a set of standard region/volume of interests, 2) In the classification step, a data-driven representation solution with a full-training approach is introduced and used
- The classification step makes use of a new deep ensemble method based on CNNs
- Main contributions of the present research: 
  1) Analyze the proposed MCME model to address a fully automatic classification approach with minimum pre-processing requirements
  2) the capability of multi-slice analysis of volumetric OCTs, including different slicing and acquisition protocols
  3) promising sensitivity
  4) robustness in the retinal OCT diagnostic problem 
- The proposed CAD system is defined to analyze the macular OCT volumes.  
- For this purpose, the MCME model in the classification stage performs a slice-based analysis by assessing the retinal B-scans in volumes and making a final diagnosis decision on cases.  Two main reasons 
  1) Slice-based analysis of retinal OCT volumes is the clinical routine in ophthalmology
  2) Different imaging protocols in retinal OCT don’t yield consistent slicing and unique volume sizes to design a full 3-D diagnostic system

### Datasets 
- Proposed algorithm was designed and evaluated on two different datasets acquired by Heidelberg SD-OCT imaging systems
  1) Acquired at *Noor Eye Hospital* in Tehran consisting of 50 normal, 48 dry AMD, and 50 DME OCT’s
  2) Publicly available dataset containing 45 OCT acquisitions obtained at Duke, Harvard, and University of Michigan.  Consists of volumetric scans with non-unique protocols of normal, dry AMD, and DME classes which includes 15 subjects for each class
- 4142 B-scans in dataset 1, and 3247 B-scans in dataset 2 were annotated by an ophthalmologist experienced in OCT imaging in addition to labeling at the patient levels
- Dataset 1: 862 DME, 969 AMD; Dataset 2: 856 DME, 711 AMD

### Data Preprocessing and VOI Extraction
- Preprocessing algorithm consists of the following steps
  1) Normalization: All the B-scans in different volumes are resized to 496 x 512 pixels (In addition to handle the intensity variations in OCT images from different patients, a normalizations tip is done to remove the intensity mean value fo each B-scan to have mean = 0 and st. dev = 1)
  2) Retinal Flattening Algorithm and Image Cropping: In OCT images, due to the anatomical structures of the acquisition distortions, the retinal layers in B-scans may be shifted or oriented randomly.  Consequently, it causes high variability in locations in B-scans.  Graph based geometry curvature correction algorithm is used for the retinal flattening based on the detection of the hyper-reflective complex
  3) ROI Selection, VOI Generation, and Augmentation Strategy: In this step, the ROI is selected by cropping a centered 128 x 470 pixels bounding box from each B-scan in a given case.  In the next step all the extracted ROIs in all B-scans are resized to 128 x 256 pixels and concatenated to generate the case VOI.  In the learning phase and for training VOIs, in order to have an efficient training process, the centered bounding box is horizontally flipped and/or translated by +/- 20 pixels.  This increases the number of samples, and decreases the chance of overfitting 
  4) Multi-Scale Spatial Decomposition: According to the following motivations, the multi-scale spatial pyramid (MSSP) decomposition is applied to macular OCT B-scans before feeding them to convolutional ensemble models
    - Some retinal pathologies such as DME exhibit key characteristics at different scales; therefore, it is expected that multi-scale views of the retina should be presented to the model
    - Suppose some B-scan *I* is represented by a 2-D array with C columns and R rows.  This image is the zero level (I_0).  Each level within I_l is computed as.a weighted average of values in level I_l-1 within a 5xx5 window

### Classification Methodology
- Modular ensemble method based on a mixture of CNN experts is used as a robust image-based classifier (divide and conquer approach in ml literature…reminds me of bagging)
- This model combines the outputs of several expert networks by training a gating network.  Given an input pattern, each expert network estimates the conditional posterior distribution not he partitioned feature space which is separated by a gating network 
- The gating network plays a weighting role and causes the overall model to execute a competitive learning process for experts.  To do that, the module tires to maximize the likelihood function of training dataset and ground truth by considering a Gaussian mixture model
- In the proposed MCME method for retinal classification 
- The MCME model learns a set of scale-dependent convolutional expert networks 
- 
