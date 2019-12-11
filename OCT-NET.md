## OCT-NET: A CNN for Automatic Classification of Normal and Diabetic Macular Edema Using SD-OCT Volumes

##### **O. Perdomo, S. Otalora, F.A. Gonzales, F. Meriaudeaux, H. Muller (2018)**

Summary: Lack of tools for automatic image analysis for supporting disease diagnosis is still a problem.  CNNs have shown outstanding performance when applied to several medical image analysis tasks.  This paper presents a model, OCT-NET, based on a CNN for the automatic classification of OCT volumes for DME diagnosis using a LOOCV strategy obtaining an accuracy, sensitivity, and specificity of 93.75% 

### 1. Introduction 
- Spectral domain OCT (SD-OCT) is a common noninvasive imaging technique useful to make DME diagnosis more objective and reliable.  The diagnosis is performed measuring five patterns of structural chances as follows: diffuse retinal thickening (DRT) cystoid macular edema (CMD), serious retinal detachment (SRD), posterior hyaloidal traction (PHT) with/without tractional retinal detachment (TRD)
- Alsaih et al. combine the extraction of histograms of oriented gradients with local binary patterns (LBPs) in order to detect DME volumes
- Lu et al. propose a method for AMD diagnosis detecting the B-scan frames containing fluid regions and converting each slice into a bag of features
- Methods based on DL also reported a good performance in classification and segmentation tasks
- Roy et al. props a fully convolutional deep architecture (ReLayNet) with a joint loss function for the segmentation of retinal layers in OCT scans
- Lee et al. developed an automated segmentation based on CNNs that detect DME and Ade-related Macular Degeneration on OCT volumes
- Karri et al. presented a Dl approach that fine tunes a pre-trained GoogLeNet on a dataset of OCT images with DME, with dry AMD and normal cases
- This paper presents an e2e CNN for automatic DME diagnosis using SD-OCT volumes

### 2. Methods
- The OCT-NET is an e1d DL model composed of 2 main blocks: the first contains 4 sub-blocks with convolutional and max-pooling layers with a different number of filters to extract features learned, and the last block has two FC and one and one dropout layers to perform the scan classification among OCT volumes

#### 2.1 Cropping and Resizing OCT Volumes
- Cropping and resizing images are common pre-processing operations applied to the input of CNN models for medical image analysis.  The goal is to remove irrelevant information of the image for extracting the Region of Interest (RoI) 
- The RoI of an OCT can be cropped by a manual segmentation by medical experts or automatically detected by a computer-aided detection system.  However, the location of layers inside an OCT scan oscillates among the top, middle, or bottom position, making it difficult to perform an automatic detection.  Because of this, a median filter on every scan was implemented in order to highlight and ease the extraction of relevant information through layers
- Specifically, bounding boxes ranged between 512 x 512 and 300 x 512.  Finally, bounding box images were centered without scaling and preserving the surrounding region with a resolution of 224 x 224 and every scan was stacked twice to handle an input size of 224 x 224 x 3 

#### 2.2 OCT-NET deep learning model
- The OCT-NET is a 12 layer CNN model that receives as an input scans with a resolution of 224 x 224 x 3 to classify an OCT volume.  Our method is based on 4 sub-blocks that contain ten convolutional layers with an incremental number of filters from 32 to 256 in order to extract and learn different data-representations
- The parameters used are as follows: ten convolutional layers with kernel size 3x3 and stride of 1x1, three max-pooling layers with pool size of 2x2 and stride of 2x2, one dropout layer, and two FC layers

### 3. SERI Dataset

#### 3.1 OCT-NET deep learning model
- The Singapore Eye Research Institute (SERI) database contains 32 SD-OCT volumes acquired by SERI following the requirements according to the ethic committee
- The database is composed of 16 normal and 16 DME OCT volumes.  Each volume contains 128 B-scans or cross-sectional scans with a resolution of 1,024 x 512 px
- OCT volumes were captured with a CIRRUS TM SD-OCT manufactured by Carl Zeiss Meditec
- OCT volumes were analyzed and labeled by experts as normal or DME volumes, according to findings among the layers, retinal thickening, locations of exudates and hard-exudates, intraretinal cystoid space information, and sub retinal fluid
- The dataset was cropped and resized to a dimension of 224x224 pixels

#### 3.2 Evaluation
- Proposed CNN was trained using the RMSprop optimizer with a decay term of commonly used value p = .9
- Batch size and learning rate were explored with a grid search 
- A DL approach combined with ensemble classifiers was chosen as baseline.  In this work, a pre-trained VGG-16 CNN was used and every OCT volume of 128 scans that was triplicated to handle an input size of 224x224x3 
- The feature vectors from the CF layers: the first two (4096 units) and the last one (1000 units), were classified using two KNNs (with K=1 and K=3), and one Random Forest (100 trees) classifier 
- The proposed method uses two considerations reported for the baseline method to classify an OCT volume as follows: the SERI dataset was split into 32 independent folds using k-fold cross validation (leave-one-patient-out), where one fold contains 31 volumes in the raining set and one volume to test the model
- The classification of one OCT volume was performed using a quorum rule, where if 65 or more scans were classified as class 0, the volume was labeled to class 0
