 ## RETOUCH: The Retinal OCT Fluid Detection and Segmentation Benchmark and Challenge

##### **H. Bogunovic (2019)**

Summary: Goal of the paper is to assess the performance of a deep learning algorithm for detecting referable glaucomatous optic neuropathy based on color fundus photographs

### Definitions
- *Dice Score*: Measures voxel overlap of two samples
- *Absolute volume difference (AVD)*: Represents a clinically relevant parameter

### I. Introduction
- Accumulated fluid causing macular edema can be readily imaged using OCT
- OCT volumes are prone to eye motion artifacts and to low signal-to-noise ratio due to speckle.  To address this, device vendors have to device on a tradeoff between achieved SNR, image resolution and scanning time
- Worth noting that the highest image quality was produced by Spectralis scanner, which reduces the noise by averaging multiple B-scans of the same anatomical location at the expense of acquiring fewer B-scans
- Three types of fluid are clinically distinguishable on OCT images and are considered relevant imaging biomarkers for visual acuity and retreatment indication
  1. Intraretinal Fluid (IRF): Consists of contiguous fluid-filled spaces containing columns of tissue as the arrangement of such spaces is determined by the Muller fires, which are vertical
  2. Subretinal Fluid (SRF): It corresponds to the accumulation of a clear or lipid-rich exudate in the sub retinal space, i.e between the neurosensory retina and the underlying retinal pigment epithelium (RPE) that nourishes photoreceptors
  3. Pigment Epithelial Detachment (PED): Represents detachment of the RPE along with the overlying retina from the remaining Bruch’s membrane due to the accumulation of fluid or material in sub-RPE space.  It is specific to AMD as it is associated with the choroidal neovascularization 
- This challenge was a multi-group collaborative effort and featured data annotations from two clinical centers, images acquired with the three most common OCT device vendors and from patients with two different retinal diseases, which allowed for the first time to make an analysis of method performance and multi-center grading agreement for all three retinal fluid types and along those components of variability
- The 112 OCT volumes (11,334 B-scans) and manual segmentations for training are publicly available
 
### II. Related Prior Work
- The first work on fluid segmentation in OCT was a semiautomated 2D approach based on active contours.  To segment IRF and SRF, one user interaction per lesion and B-scan was necessary to initialize the contour.  A similar level set approach but using a fast Split Bregman solver was later used to generate all candidate fluid regions automatically which were then manually discarded or selected
- Lots of other methods were presented

### III. Challenge Setup
- Challenge aimed at creating a representative benchmark which can be used for evaluating algorithms for detecting and segmenting all of the three fluid types across retinal diseases and OCT vendors

### IV. Evaluation Framework

#### OCT Imaging Dataset
- Authors collected and anonymized a total of 112 macula-centered OCT volumes of 112 patients from MUV and ERASMUS.  Half of the patients had macular edema secondary to AMD and half of them had edema secondary to RVO
- Each Cirrus OCT consisted of 128 B-scans with a size of 512 x 1024 pixels
- Spectralis OCT consisted of 49 B-scans with 512 x 496 pixels
- Topocon OCT consisted of 128 B-scans with a size of 512 x 885 (T-2000) or 512 x 650 (T-1000) pixels
- All the OCT volumes were covering a macular area of 6x6 mm^2 with axial resolutions 
- The training set had 70 OCT volumes, with 24, 24, and 22 volumes acquired with Cirrus, Spectralis, and Topcon respectively
- The test set consisted of a set of 42 OCT volumes, with 14 volumes corresponding each of the three device vendors

#### Reference Standard
- Reference standard was obtained form manual vowel-level annotations of the IRF, SRF, and PED fluid lesions in each of the B-scans of each individual OCT volume
- Manual annotation tasks were distributed to human graders from two clinical centers, and all annotations were performed on a B-scan plane

#### Evaluation and Ranking
- Evaluation consisted of forming two main leaderboards corresponding to the two tasks: decision and segmentation
- For each leaderboard, a dense ranking was used where teams with equal scores receive the same ranking number, and the next team receives the immediately following number without creating gaps in the ranking
- *Detection Task*: 
  - The teams submitted for each case probability of presence of each fluid type
  - For each of the three fluid types, receiver operating characteristics (ROC) curve was created across the test set images with inter-center agreement, and AUC was calculated
- *Segmentation Task*: 
  - The teams submitted for each case a volume containing the segmentation results with each fluid type represented with its voxel label
  - The results were compared to the reference standard, where voxels masked for exclusion due to inter-center disagreement
  - For each OCT volume and fluid type the similarity of two samples, segmentation (X) and (Y), was measured using the Dice score and the Absolute volume difference
