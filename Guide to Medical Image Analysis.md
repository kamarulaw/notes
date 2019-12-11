## Guide to Medical Image Analysis 

##### **K. D. Toennies (2017)**

Summary: Application-focused introduction to medical image analysis

### Definitions 
- Image Segmentation: Process of partitioning a digital image into multiple segments (sets of pixels aka superpixels).  Goal is to simplify and/or change the representation of an image into something that is more meaningful and easier to analyze 
- Pyramid: Signal or image is subject to repeated smoothing and subsampling
  - Gaussian Pyramid: Subsequent images are weighted down using a Gaussian blur and scaled down 
- Image Registration: Is the process of transforming different sets of data into one coordinate system.  Data may be multiple photographs.  Necessary in order to be able to compare or integrate data obtained from different measurements
  - One image is referred to as the reference/src, and the others are referred to as the target/subject
  - Intensity-based methods compare intensity patterns in images via correlation metrics.  Feature-based methods establish correspondence between a number of especially distinct points in images

#### Chapter 4: Image Enhancement 
- Enhancement can (1) increase perceptibility of objects in an image to the human observer, or (2) it may be needed as a preprocessing step for subsequent automatic image analysis 
- Enhancement method requires a criterion by which its success can be judged
- In medical imaging, image enhancement essentially enhances contrast by reducing any noise in the image or by emphasizing differences between objects
- A good approach for measuring global contrast is the ***root-mean-square (rms)*** contrast (standard deviation of the pixel intensities)
  - Does not differentiate well between different intensity distributions
- ***Entropy*** as contrast includes histogram characteristics into the measure.  
  - Computed from the normalized histogram of image intensities
  - Gives the probability that a given pixel will appear
  - 

#### Chapter 6: Segmentation, Principles and Basic Techniques
- Segmentation strategies in medical imaging combine data knowledge with domain knowledge to arrive at the result
  - Data Knowledge: refers to assumptions about continuity, homogeneity, and local smoothness of image features with segments
  - Domain Knowledge: Info about the objects to be delineated
- After segmentation, every pixel is assigned to exactly one segment since every location in an image carries just one meaning
- Several ways to deal with missing info without sacrificing the assumption that a low-level segmentation criterion is valid everywhere in the image
  - Foreground segmentation: Focuses on a single object in the image.  Seg criteria create a good partitioning of foreground objects, whereas the quality of partitioning the background is irrelevant.  Later analysis is carried out solely on foreground segments
  - Hierarchical segmentation: Applies a multi-resolution concept for gradual refinement.  A 1st seg creates segs that are smaller than the smallest object.  Successful application of this strategy requires that meaningful segments can be defined by analyzing a common criterion at a single, but unknown scale
  - Multilayer segmentation: Multi-resolution technique.  Seg is carried out at different scales producing layers of segments.  Analysis of segments is more difficult because an appropriate scale for every segment has to be established independently from other segments
- in medical imaging different semantics stem from intentional choice of the acquisition technique
  - The search for occurrences of a specific structure by seg causes *foreground seg* to be more frequent in medical image analysis than in other image analysis tasks.  A model-driven approach 
- Popular segmentation techniques: various region growing techniques, application of implicit or explicit active contours and surfaces, active shape models
- Continuity in space and time are the two main data properties that are used for segmentation
- Temporal continuity can be used in the same fashion by treating time in as a 4th dimension.  It is usually exploited by computing a segmentation result at one time step and using it to constrain or initialize the segmentation at the next time step
  - Sometimes applied for segmenting a sequence of 2D images of a 3D scene
- Propagation of segmentation constraints along the time axis or a spatial axis makes the result dependent on the initial segmentation (initialization should be as good as possible)
- Carrying out n-dim segmentation by imposing continuity constraints between adjacent segmentations in (n-1)-dim space reduces subsequent segmentation to a registration task if a 1-1 correspondence exists
- Spatial and temporal continuities can be characterized by homogenous local appearance
  - Pixels or voxel intensities of a structure of interest often vary little throughout the segment
- Noise can be detrimental to segmentation tasks.  Pre-processing might help, but it might also remove small details 
- Domain knowledge consists of information: 
  1.  The appearance of boundaries between segments 
  2.  The location of an object within an image 
  3.  Orientation/size of the object
  4.  Spatial relationships 
  5.  The shape and appearance of the object
- Domain knowledge should be discriminative, generalizable, and computable 
- Domain knowledge may be introduced via an adjustable model that is included in the segmentation method or interactively at runtime
- Each of the five types of domain knowledge listed above can be represented by a *parameterized description*, *samples description*, or an *implicit description*
- Properties described by samples require a sufficiently high sampling rate
- Properties described implicitly as function of the image domain require a number of base functions on this domain which can be combined to describe the object boundary 
- Property values of any of the descriptions may be combined in a property vector describing expected features of a segment
- Training samples are usually taken from the same kind of images that are to be segmented later
- Interactive input may happen directly on the image or it may be an adjustment of segmentation params
- Performance of user depends on a lot of factors, so often times interacting is supported by low-level techniques 
- *Thresholding* is an often-used tool in segmenting medical images
  - Produces a separation image pixels into foreground pixels (s = 1) and background pixels (s = 0)
- Thresholding is a simple technique which is fast, easy to implement, and easy to understand
- Segmentation based on local intensity homogeneity does not require an absolute threshold, but a local variance criterion 
- Two basic segmentation algorithms for this are region merging, and the split-and-merge algorithm
- The watershed transform is a popular way to use edge information as criterion to separate segments

#### Chapter 7: Segmentation in Feature Space
- 

#### Chapter 10: Registration and Normalization 
- Info about an object from different sources can be combined if a transformation allows mapping data from one source to data of the other source
- In medical imaging, the two sources, are image acquisition systems.  If the two sources depict the same subject, this process is called *registration*.  Different subjects => normalization
- *Registration* computes  transformation between two or more pictorial representations of the same subject.  The transformation is carried out on one image, the moving image, so that it optimally fits to the other image, the reference image.  
- *Normalization* is the operation to compare images from two or more different subjects.  The use of the term normalization refers to the mapping of images of different subjects onto some common norm image (in this case the reference image)
- Finding the appropriate transformation between two scenes f and g depends on four aspects 
  1.  The *feature space* determines features F in f and g that will be used for correspondence
  2.  The *similarity criterion* S defines what is meany by correspondence between registered scenes
  3.  The *search space* contains all possible transformations T among with an optimal transformation is searched
  4.  The *search strategy* tells how - given some transformation producing some similarity - the next transformation is found
- In order to make the search for the correct registration function a minimization risk, the similarity measure is often defined such that it is low when feature locations correspond given some transformation 
- Establishing correspondences between reference image f and moving image g requires finding a measure by which equivalence between locations in the two scenes is quantified.  Two points in the two images are assumed to be equivalent if they have the same location with respect to some unknown patient coordinate system
- The relation between the known scanner coord system and the patient coord system is influenced 
  -  by different positioning of the patient in the scanner 
  -  by distortions caused by the imaging system
  -  by any movements of organs due to physiology 
  -  by changes caused from interventions taking place between creating f and g
- Equivalent locations in the two images are assumed to have similar appearance
- Features commonly used for measuring correspondence in medical imaging registration are as follows: 
  -  Extrinsic markers 
  -  Intrinsic features: landmark points, curves, or regions 
  -  Voxel intensities or gradients 
- Extrinsic and intrinsic markers are assumed to be describable by a finite set of feature points which are distinguishable in the two scenes.  Feature function assigns the label 1 to feature points and the label 0 to all other locations in the image.  Feature similarity c_c simply notes the incidence of coinciding feature points with the same label
- Local correspondence can be relaxed by introducing some minimum distance eps that must not be exceeded for declaring two feature point positions to be equal
- Markers should be small for simplifying computation of the feature point location from a segmented marker region.  They should be opaque with respect to the imaging technique and produce a high-contrast signal 
- Intrinsic landmarks are feature points of anatomical or geometrical nature.  
- The spatial relationship between landmarks and the scenes to be registered need to be known and should be simple for inferring a scene registration transformation from landmark registration
- num of landmark locations is small or localization is unreliable or search space for a transformation is complex => landmark-based registration may not be suitable
- Image-based registration features can provide a denser map of corresponding locations 
  - May not be reliable everywhere in the scene, but on average the number of reliable feature locations is larger than in landmark-based feature computation.  Image-based features are not unique, though, because values of low-level features cannot serve as unique label
  - In the simplest case intensity may serve as a feature, but using intensity difference may be hampered by noise, especially if the contrast in the image is low 
  - Computing sign changes instead is a more stable criterion.  Has been commonly used for 2d image registration 
- Straightforward idea is to use intensity gradient as a feature.  Boundary segments are treated as landmark curves or landmark regions for which the average distance between 
- Extensive investigation about the accuracy of registration methods has found *mutual information* to deliver the best results.  Since the quality of the measure deteriorates with increasing noise, images are often preprocessed for noise removal  
- Mutual information measures brightness correspondence in the two images.  Similar to correlation, but not as specific
- Finding a registration transformation means to minimize the similarity criterion.  The registration problem has a number of properties that have to be accounted for  
