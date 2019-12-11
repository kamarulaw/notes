## Automatic Feature Learning for Glaucoma Detection Based on Deep Learning

##### **X. Chen, Y. Xu, S. Yan, D. W. K. Wong, T. Y. Wong, J. Liu (2015)**

Summary: ALADDIN: Automatic feature Learning for glAucoma Detection based on Deep LearnINg, with deep CNNs for feature learning.  Different from traditional conv layers that use linear filters followed by a non-linear activation.  Results show AUC under the ROC of .838 and .898 in the two databases

### Dataset 
- **ORIGA**: online retinal fundus image database for glaucoma analysis and research.  Comprised of 168 glaucoma and 482 normal fundus images
- **SCES**: Contains 1676 images, and 46 images are glaucoma cases

### Definitions 
- **Ophthalmoscopy**: AKA funduscopy, is a test that allows a health professional to see the funds of the eye and other structures using an ophthalmoscope.  Done as part of an eye examination, and may be done as part of a routine physical examination.  Two major types are direct and indirect
- **Direct Ophthalmoscope**: instrument about the size of a small flashlight with several lenses that can magnify up to about 15 times.  Most commonly used during a routine physical examination
- **Indirect Ophthalmoscope**: Constitutes a light attached to a headband, in addition to a small handheld lens.  It provides a wider of the inside of the eye
- **C-CNN**: Contextualized Convolutional Neural Network takes the outputs from one learned CNN as the context input f its own fully-connected layer.  
  - Context takes the responsibility of adjusting the model learning of CNN
- **Template Matching**: Technique in digital image processing for finding small parts of an image which match a template image.  It can be used in manufacturing as a part of quality control

### Notes
- Glaucoma diagnosis is typically based on the medical history, intra-ocular pressure and visual field loss tests together with a manual assessment of the Optic Disc through ophthalmoscopy
- OD can be divided into two distinct zones 
  1.  Central bright zone called the optic cup
  2.  A peripheral region called the neuroretinal rim
- One of the important indicators is the enlargement of the cup with respect to OD, various parameters are considered and estimated to detect glaucoma, such as vertical CDR
  - All these measurements focus on the study of OD and most of them only reflect one aspect of the glaucoma disease
  - Effectively capturing the hierarchical deep features of the OD to boost the glaucoma detection is the main interest in this paper
- The proposed network has 6 layers: five multilayer perceptron convolutional layers, and one FC layer.  Response-normalization layers and overlapping layers are also employed in the proposed architecture
- The convolution filter in traditional CNN is a GLM.  Authors replace the GLM with a more potent nonlinear function approximator, which is able to enhance the abstraction ability of the local model
- Two reasons for using multilayer perceptron 
  1.  MLP is compatible with the structure of CNNs 
  2.  MLP can be a deep model itself, which is consistent with the spirit of feature re-use (type of layer is denoted as mlpconv, in which MLP replaces the GLM to convolve over the input.  ReLU is used as the activation function in the MLP)
- The C-CNN comprises the five mlp convolution layers of CNN1 and whole network for CNN2
- Optic disc is the main areas for glaucoma diagnosis => disc images are the input images of the proposed C-CNN
- **Disc Segmentation**
  - To segment the optic disc from a retinal funds image the authors used Template Matching
  - The DS method is based on peripapilary atrophy elimination, where the elimination is done though edge filtering, constraint elliptical Hough transform and peripapillary atrophy detection.
  - Then segmented disc image is down-sampled to a fixe resolution 256 x 256 
  - The mean value over all the pixels is the disc image is subtracted from each pixel to remove the influence of illumination variation among images
- **Dropout and Data Augmentation**
  - Authors reduced overfitting on image data by employing data augmentation to artificially enlarge the dataset using label-preserving transformations
  - Data augmentation consists of generating image translations and horizontal reflections.  Performed aug by extracting random 224 x 224 patches including their horizontal reflections from the 256 x 256 images, and training our network on these extracted patches.  Size of dataset was increased by a factor of 2048
  - Dropout was used in the fully-connected layer in the architecture
