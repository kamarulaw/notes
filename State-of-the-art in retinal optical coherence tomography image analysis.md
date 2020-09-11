## State-of-the-art in retinal optical coherence tomography image analysis

### Abstract
- OCT is an emerging imaging modality that has been used widely in the field of biomedical imaging
- OCT is able to non-invasively produce cross-sectional volumetric images of the tissues which can be used for analysis of tissue structure and properties.  Due to the underlying physics, OCT images suffer from a granular pattern, called speckle noise, which restricts the process of interpretation
- This requires specialized noise reduction techniques to eliminate the noise while preserving image details
- Another major step in OCT image analysis involves the use of segmentation techniques for distinguishing between different structures, especially in retinal OCT volumes
- Lastly, movements of the tissue under imaging as well as the progression of disease in the tissue affect the quality and the proper interpretation of the acquired images which require the use of different image registration techniques 
- This paper reviews various techniques that are currently used to process raw image data into a form that can be clearly interpreted by clinicians

### Introduction
- In this paper we will discuss the three main aspects of automated retinal OCT image processing: noise reduction, segmentation, and image registration

### OCT imaging systems
- The first widely available OCT imaging system in biomedical imaging is called time-domain OCT (TD-OCT) in which a reference mirror is translated to match the optical path from reflections within the sample 
- The second most common type is FD-OCT.  For this type of OCT, there is no need for moving parts in the design of the imaging system to obtain the axial scans since the reference path length is fixed and the detection system is replaced with a spectrometer

### Noise reduction
- Speckle is a fundamental property of signals and images acquired by narrow-band detection systems like SAR, ultrasound, and OCT
- Two types of speckle are present in OCT images: signal-carrying speckle which originates from the sample volume in the focal zone; and signal-degrading speckle which is created by multiple-scattered out-of-focus light.  The latter kind if is what is considered speckle noise 
- OCT noise reduction techniques can be divided into two major classes: (I) methods of noise reduction during the acquisition time and (II) post-processing techniques
- Post-processing techniques are preferred, and make the most sense for what we’re working on

### Image segmentation
- The most difficult step in any medical image analysis system is the automated localization and extraction of the structures of interest
- The signal strength in OCT images rises from the intrinsic differences in optical properties of tissues
- Used of pattern recognition based methods is a new direction that attracted researchers for more explorations
- Two/three dimensional graph-based methods are probably the best among all of the segmentation methods since they are relatively fast, more accurate, and unlike the classification based techniques, don’t need training data sets for segmentation of new datasets

### Image registration
- Having two input images, a template image and a reference image, image registration trees to find a valid optimal spatial transformation to be applied to the template image to make it more similar to the reference image
- There are two different classes of image reg techniques.  (I) parametric approaches and (II) non-parametric approaches
- There are several different applications for using image registration approaches in OCT image analysis.  One basic application is in speckle noise reduction
- Motion correction of OCT images is another area in which image registration techniques are of high interest
- Several involuntary eye movements that can happen during the fixation: tremor, drifts, and microsaccades 
- Image mosaicing is another example of using image reg methods in OCT image progressing
  - OCT systems can acquire large amounts of data in short periods of time; but still, the field of view is very limited
  - Stitching several volume data points from a patient can improve the interpretation of data significantly
