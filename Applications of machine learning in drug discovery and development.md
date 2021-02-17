## Applications of machine learning in drug discovery and development

##### **J. Vamathevan et al.**

### Abstract
- Opportunities to apply ML occur in all stages of drug discovery
- Examples include target validation, identification of prognostic biomarkers and analysis of digital pathology data in clinical trials
- Lack of interpretability and repeatability of ML-generated results limits their application in this space

### Stages of Drug Discovery Development

#### Target identification and validation

##### Successful applications in drug discovery
- Target identification and prioritization based on gene-disease associations 
- Target draggability predictions, and identification of alternative targets

##### Required data characteristics 
- Current data are highly heterogeneous: need standardized high-dimensional target-disease-drug association of data sets
- High confidence associations from the literature
- Metadata from successful and failed clinical trials

#### Compound screening and lead discovery

##### Successful applications in drug discovery
- Compound design with desirable properties
- Ligand-based compound screening 

##### Required data characteristics 
- Large amounts of training data needed
- Gold standard ADME data

#### Preclinical development

##### Successful applications in drug discovery
- Tissue-specific biomarker identification
- Classification of cancer drug-response signatures
- Prediction of biomarkers of clinical endpoints

##### Required data characteristics 
- Biomarkers: reproducibility of models based on gene-expression data
- Dimension reduction of single-cell data for cell type and biomarker identification
- Proteomic and transcriptomic data of high quality and quantity 

#### Clinical development

##### Successful applications in drug discovery
- Determination of drug response by cellular phenotypic in oncology
- Precise measurements of the tumor microenvironment in immune-oncology

##### Required data characteristics 
- Pathology: Well curated expert annotations for broad-use cases (cancer vs. normal cells)
- Gold standard data sets to improve interpretability and transparency of models
- Sample size: high number of images per clinical trial


### General Notes
- The advent of high-throughput approaches to biology and disease presents both challenges and opportunities to the pharmaceutical industry, for which the aim is to identify plausible therapeutic hypotheses from which to develop drugs
- Uptake from pharmaceutical industry has lagged until recently in the ML space
- It is well known that the success rate for drug development is very low across all therapeutic areas and across the global pharmaceutical industry (as defined from phase I clinical trials to drug approval)
- There a wide number of challenges associated with compound structure representation in ML methods

### Applications in drug discovery (target identification and discovery)
- The pre-eminent approach in DD is to develop drugs (small molecules, peptides, antibodies or newer modalities including short RNAs or cell therapies) that will alter the disease state by modulating the activity of a molecular target
- Although the ultimate validation of the target will only come later, through clinical trials, early target validation is crucial to focus efforts on potentially successful projects
- The first step in target identification is establishing a causal association between the target and the disease 

### Applications in drug discovery (small-molecule design and optimization)
- Discovery of drug candidates that can black or activate the target protein of interest involves extensive virtual and experimental high-throughput screening of large compound libraries

### Applications in drug discovery (predictive biomarkers)
- ML-based biomarker discovery and drug sensitivity predictive models are demonstrated approaches to help improve clinical success rates, to better understand the mechanism of action of a drug, and to identify the right drug for the right patients
