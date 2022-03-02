## Applied Predictive Modeling  

##### **M. Kuhn, K. Johnson (2013)**

Summary: Data analysis book w/ a focus on predictive modeling.  

### Chapter 1: Introduction
- Objectives of the book are to provide foundational principles for building predictive models, intuitive explanations of commonly used PM methods for classification and regression, principles and steps for validating a PM, computer code to perform the necessary foundational work to build and validate PMs
- Primary interest of PM is to generate accurate predictions, a secondary interest may be to interpret the model and understand why it works
- *Prediction vs. Interpretation tradeoff*: As we push towards higher accuracy, models become more complex and their interpretability becomes more difficult
- Best models are influenced by a modeler with expert knowledge and context of the problem
- Availability of large quantities of records is not protection against an uninformed use of data
- Part I: Foundations for model building, Part II: Modern Regression Techniques, Part III: Classification Techniques, Part IV: other important considerations when building a model or evaluating its performance

### Chapter 2: A Short Tour of the Predictive Modeling Process
- Resampling: Different subversions of the training data are used to fit the model
- ***MARS: Multivariate Adaptive Regression Spline***
  - Fits separate LR lines for different ranges of engine displacement.  
  - Has tuning parameter which cannot be directly estimated from the data.  
  - No analytical equation that cam be used to determine how many segments should be used to model the data
- **Themes**
  - Data Splitting: How the training and test sets are determined should reflect how the model will be applied
  - Predictor Data: Using more predictors, it is likely that the RMSE for the new models can be driven down further.  Feature selection is discussed later
  - Estimating Performance: Quantitative and visual assessments to assess model performance before testing
  - Evaluating Several Models: “NFL” Theorem states without substantive info about the problem, there is no single model that will always do better than any other model
  - Model Selection: One goal of the book is to help give intuition regarding the strengths and weaknesses of different models

### Chapter 3: Data Pre-processing 
- DPP generally refers to addition, deletion, or transformation of training set data
- This chapter outlines approaches to unsupervised data processing
- Feature engineering, how the predictors are encoded, can have a significant impact on model performance
- Example feature: Submission date of grant
  - number of days since a reference date
  - isolating the month, year, and day of the week as separate predictors ;) 
  - whether the date was within the school year (as opposed to holiday or summer sessions)
- In some models, multiple encodings of the same data may cause problems, while some models contain built in feature selection (only includes predictors that maximize accuracy)
- Data Transformations for Individual Predictors: some modeling techniques require that the values be on the same scale
  - Centering & Scaling
    - Centering: Average predictor value is subtracted from all the values
    - Scaling: Each value of the predictor variable is divided by its standard deviation
  - Transforms to Resolve Skewness: Right skew (lots of data on left), Left Skew (opposite)
  - Highest / Lowest value > 20 roughly => skew
    - Replacing the data with log, square root, or inverse may help to remove the skew
  - Skewness: Close to 0 if data is roughly symmetric
  - Box and Cox: Family of transformations indexed by a parameter lambda.  Can identify square, square root, inverse, and others.  Applied independently to each predictor that contains values greater than 0 
- Data Transformations for Multiple Predictors: Transformations act on groups of predictors, typically the entire set under consideration 
  - Transformations to Resolve Outliers: 
    - ***Tree Based Classifiers*** are resistant to outliers.  ***SVM’s*** generally disregard a portion of the training set samples when creating a prediction equation
    - If model sensitive to outliers, then one data transformation that can minimize the problem is spatial sign.  Has the effect of making all the samples the same distance from the center of the sphere
- Data Reduction and Feature Extraction
  - Reduce the data by generating a smaller set of predictors that seek to capture a majority of the information in the original variables.  
  - ***PCA*** is a commonly used data reduction technique.  
    - Seeks to find LCs of the predictors known as PCs which capture the most possible variance
    - PCA seeks predictor-set variation without regard to any further understanding of the predictors (e.g., measurement, scale, distributions)
    - Eg: If the original predictors are on measurement scales that differ in order of magnitude, say demographic predictors (income and height), then the first few components will focus on summarizing the higher magnitude predictors, while latter components will do lower variance
    - This means that PCA will be focusing its efforts on identifying the data structured based on measurement scales rather than based on important relationships within the data for the current problem
    - cav1: It is best to first transform skewed predictors, and then center and scale the predictors prior to performing PCA.  
    - cav2: PCA does not consider the modeling objective or response variable when summarizing variability.  If the relationship between between the predictors and response is not connected to the predictors’ variability, then the derived PCs will not provide a suitable relationship with the response.  In this case a supervised technique, like PLS will derive components while simultaneously considering the corresponding response
    - Heuristic approach for determining the number of components to retain is to create a scree plot, which plots the ordered component number against the summarized variability
    - Plot the first few components against each other and the plot symbols can be colored by relevant characteristics.  Such as class labels if ***PCA*** a sufficient amount of information in the data, then this type of plot can demonstrate clusters of samples or outliers the may prompt closer examination
- Dealing w/ Missing Values: Could be structurally missing, or could have not been determined at the time of model building.  It is important to understand why values are missing.  Missing values are more often related to predictor variable than the sample.  
  - Informative Missingness: The pattern of missing data is related to the outcome.  Can induce significant bias in the model
  - Censored data: The exact data is missing, but something is known about its value
  - In large data sets, removing samples based on missing values is a non-prob.  In smaller data sets it is more serious.  Alternatives are below 
    - A few predictive models, especially ***tree-based techniques*** can account for missing data (amounts to a predictive model inside a predictive model)
    - Missing data can be imputed
  - Imputation: Another layer of modeling where we try to estimate values of the predictor variables based on other predictor variables
    - Use the training set to build an imputation model for each predictor in the data set.  
    - If we use resampling to select tuning parameter values or estimate performance, then imputation should be incorporated within the resampling
    - If the number of predictors affected by missing values is small, EDA of the relationships between the predictors is a good idea.  ***Visualization*** or ***PCA*** might be useful
    - ***kNN*** is a popular imputation technique.  Advantage: imputed data are confined to be within the range of the training set values.  Disadvantage: Entire training set is required every time a missing value needs to be imputed + # of neighbors is a tuning parameter, as is the method of determining a distance metric.  Although this modeling is fairly robust to tuning parameter
- Removing Predictors
  - Potential Advantages: fewer predictors => lower comp time and complexity, two predictors correlated => measuring same underlying property and removing one might make more interpretable model, models can be crippled by predictors w/ degenerate distributions 
  - Remove zero variance predictors (case analysis => either way you can drop it)
  - Ratio of the most frequent occurrence to the 2nd one tells us imbalance 
  - Good rule of thumb: fraction of unique values over the sample is low ( < 10%) && ratio of 1st to 2nd is large ( >= 20)
- Between-Predictor Correlations 
  - *Collinearity* is the technical term for the situation where two features have a high correlation.  When this occurs with multiple predictors at once it is termed *multicollinearity*
  - When the data set consists of too many predictors to examine visually, techniques such as ***PCA*** can be used to characterize the magnitude of the problem.  EG: 1st PC accounts for a large % of variance => at least one group of predictors 
  - Using highly correlated predictors in techniques such as ***LR*** result in highly unstable models, numerical errors, and degraded predictive performance
  - Since collinear predictors can impact the variance of parameter estimates in ***LR***, a statistic named the variance inflation (VIF) can be used to identify predictors that are impacted.  Beyond ***LR***, this method may be inadequate for several reasons: it was developed for linear models, requires more samples than features, does not determine which should be removed to resolve the problem
  - Algorithm
    1.  Calculate correlation matrix of the predictors
    2.  Determine the two predictors associated with the largest absolute pairwise correlation (A and B)
    3.  Determine the average correlation between A and the other variables.  Do the same for B
    4.  If A has a larger average correlation, remove it; o/w remove B
    5.  Repeat 2 - 4 until no absolute correlations are above the threshold
  - EG: a model that is sensitive to between-predictor correlations could have a threshold of .75
  - ***PCA*** good too, but unsupervised so no guarantee the resulting features have any relationship with the outcome
- Adding Predictors 
  - Categorical is encoded via dummy-variables. Models that include an intercept term would have numerical
  - ***LogR*** well known classification method can have non linear terms added to it 
  -  One technique to add a complex combination of the data is to compute the “class centroids“, the centers of the predictor data for each class.  Then for each feature, the distance to each class centroid can be calculated and these can be used in the model
- Binning Predictors: Method to avoid
  - Many of the modeling techniques are good at finding complex patterns and relationships between the data.  Binning results in (1) potential significant loss of performance in the model, (2) loss of precision in the predictions when the predictors are categorized, (3). Research has shown that categorizing predictors can lead to a high rate of false positives  
  - This argument applies to manual categorization of predictors.  Several methods, such as ***CART*** and ***MARS*** that estimate cut points in the process of model building.  Difference is that these models use all the predictors to derive bins based on a single objective (minimizing error)  

### Chapter 4: Over-Fitting and Model-Tuning
- Overfitting: When a model overemphasizes patterns that are not reproducible
- Practical note: all model building efforts are constrained by the existing data.  Remainder assumes the data quality is sufficient and representative of the population
- Most PM techniques have tuning parameters that yield the best and most realistic predictive performance 
- Train set: used to build and tune the model.  Test set: used to estimate the model’s performance
- Poor values for tuning parameters can result in a model that overfits
- There are different approaches for searching for the best parameters
- A general approach that can be applied to almost any model is: 
  1.  Define a set of candidates values
  2. Generate reliable estimates of model utility across the candidate values
  3.  Choose the optimal settings
  4.  Once candidate parameter values have been selected, we obtain trustworthy estimates of model performance.  The performance on the hold-out is then aggregated into a performance profile, which is then used to determine the final tuning parameters
- Common steps in model building: (1) Pre-processing the predictor data, (2) estimating model parameters, (3) selecting predictors for the model, (4) evaluating model performance, and (5) Fine tuning class prediction rules (via ROC curves, etc.).  Modeler must decide how to “spend” their data points
- When the # of samples is not large, a case can be made that a test set should be avoided because every sample may be needed for model building
- Nonrandom methods for splitting data are sometimes appropriate.  EG when we are interested in using old spam techniques to catch new spam methods
- Ways to split data
  - Simple Random Sample: Doesn’t control for any attributes 
  - Stratified Random Sample: Applies random sampling within subgroups
- Resampling Techniques: Generally they operate similarly.  Subset of samples are used to fit a model, and the remaining are used to estimate how well the model performs
  - ***kF CV***: Samples randomly partitioned into k sets of roughly equal size.  Model is fit using all samples except the first fold.  First fold is predicted using the model.  Repeat this process for all k folds.  The resampled estimates of performance are summarized and used to understand 
    - k usually =  5 or 10.  As k gets larger the *bias* decreases, but large values of k are computationally inefficient
    - Another important aspect of a resampling technique is the variance.  Unbiased method might estimate the correct value, but have a high price in uncertainty.  Means repeating the resampling procedure may produce a very different value
  - ***Bootstrap***: Random sample of the data taken with replacement.  This means that, after a data point is selected for the subset, it is still available for selection.  Bootstrap sample is the same size as the original dataset. 
    - Bootstrap has less uncertainty than that ***kf CV***.  On average about 2/3 of the data points are represented in the bootstrap sample at least once => bias similar to similar to ***kF CV*** for k = 2.  Problematic for small datasets.  Tolerable for larger ones
- In general it is good to favor simpler models over more complex ones
  - The “one standard error“: Find the numerically optimal value and its corresponding standard error.  Then pick the simplest model within 1 std error of the best val
  - Another approach: Choose simpler model within a certain tolerance of the numerically best val.  % dec - (X - O) / O, where X = performance val and O = numerically optimal val
- Data splitting recommendations 
  - A strong case can be made against a single, independent test set
  - No resampling method is absolutely better than the others.  
    - If the size is small, we recommend repeated 10-fold CV for several reasons: 
    - If the goal is to choose between models, then a bootstrap method is a good idea (they have low variance)
- Choosing between models 
  - After the settings for the tuning parameters have been determined for each model we must choose between multiple models.  Below is a recommended scheme
    1.  Start with several of the least interpretable models.  EG: ***Boosted trees***, ***SVM*** (most accurate methods)
    2.  Investigate simpler models that are less opaque, such as ***MARS***, ***partial least square***, ***GAMS***, or ***NB***
    3.  Consider using the simplest model that reasonably approximates the performance of the more complex models

### Chapter 5: Measuring Performance in Regression Models
- Different ways to measure accuracy each with own nuance. Visualization of the model fit, particularly residual plots are critical to understanding whether the model is fit for purpose
- *RMSE*: Usually interpreted as either how far (on average) the results are from zero or as the average distance between the observed values and the model predictions
- *R^2*: Can be interpreted as the proportion of the information in the data that is explained by the model (Note: measure of correlation not accuracy)
- *Rank Correlation*: Takes the ranks of the observed outcome values and evaluates how close there are to ranks of the model predictions.  This metric is commonly known as Spearman’s rank correlation
- Generally true that complex models have very high variance, and tend to overfit.  Also true that simple models tend to under-fit more if they are not flexible enough (high bias).  Models will be discussed that can increase the bias in the model to greatly reduce the model variance as a way to mitigate the problem of collinearity

### Chapter 6: Linear Regression and its Cousins
- In addition to LR, these types of models include Partial Least Squares (***PLS***), penalized methods such as ***Ridge Regression***, ***LASSO***, and ***Elastic Net***
- Each of these seeks to find parameter estimates so that the sum of square error, or some function of the sum of square errors is minimized
- These methods fall along the spectrum of the bias-variance trade-off.  ***OLR***, at one extreme, finds parameter estimates that have minimum bias, whereas ***Ridge Regression***, ***LASSO***, and ***Elastic Net*** find estimate that have lower variance 
- Advantage of these models is that their mathematical nature allows us to compute standard errors of the coefficients, provided that we make certain assumptions about the distributions of the model residuals
- Linear regression-type models are highly interpretable, but can have limited utility in practical situations
- Methods are appropriate when the relationship between the predictor and response fall along a hyperplane.  If there is a curvilinear relationship between the predictors and response, then linear regression models can be augmented with additional predictors that are functions of the original predictors
- Plot the individual predictors vs. the response outcomes
- (X^T * X)^(-1): proportional to the covariance matrix 
- if # of predictors > # of samples 
  - Use pre-processing techniques to remove.  This pre-processing step may not completely eliminate collinearity, since one or more predictors may be functions of two or more of the other predictors.  Diagnose multicollinearity in the context of LR using the *variance inflation factor (VIF)*
    - if # of predictors still outnumbers the observations, then we have to use other measures.  Other remedies include simultaneous dimension reduction and regression via PLS or using parameter shrinkage methods
- Visual cue to understanding if the relationship between predictors is not linear is to examine basic diagnostic plots.  Curvature in the pred. vs res plot is a primary indicator of non linearity
- Problem w/ LR is that it is prone to chasing outliers (since it is trying to minimize SSE).  Observations that cause significant changes in the parameter estimates are called *influential*
- When using resampling techniques such as ***bootstrapping*** or ***kf CV***, you still have to think about the relative number of samples to predictors
- For many real data sets, features can be correlated and contain similar predictive information.  If the correlation among predictors is high, then the ***OLS*** solution for ***MLR*** will have high variability and will become unstable.  In other cases # features > # samples which will also result in ***OLS*** being unable to create a unique set of coefficients
- Solutions: (1) removal of highly correlated predictors, (2) PCA
  - (1) Removing highly correlated features ensures that pairwise correlations among features are below a threshold, but does not ensure the LC’s of features are uncorrelated with other features => removal of correlated features may not guarantee a stable least squares solution
  - (2) Using PCA guarantees that that resulting components, but because the LC’s are combinations of the original predictors, the practical understanding of the new features can be unclear 
- ***PCA*** before regression is known as principal component regression ***PCR***.  Technique commonly used when # predictors > # of observations or if inherently highly correlated predictors or problems
  - Method can be easily misled.  DR via ***PCA*** does not necessarily produce new predictors that explain the response => ***PCR*** can have identifying a predictive relationship when one might exist
- ***PLS*** originated with Herman Wodl’s nonlinear iterative partial least squares (***NIPALS***) algorithm.  Briefly the algorithm iteratively seeks to find underlying relationships among the features which are highly correlated with the response.  ***PLS*** LCs of features are chosen to maximally summarize covariance with the response.  This means ***PLS*** finds components that maximally summarize feature variation, while also maximizing correlation with the response
- ***PCR*** produces models with similar similar predictive ability to ***PLS***. |***PCR*** comps| >= |***PLS*** comps|  
- The NIPALS algorithm works well for datasets of small to moderate size < 2,500 samples && < 30 features 
- MSE is a combination of bias and variance.  Possible to produce model with smaller MSE’s by allowing the parameter estimates to be biased

### Chapter 7: Nonlinear Regression Models
- Last chapter talked about intrinsically linear methods.  Many of these models can be adapted to nonlinear friends in the data by manually adding model terms but to do this one must know the specific nature of the nonlinearity in the data
- There are a # of regression models that are inherently nonlinear in nature.  These models do not require an explicit nonlinearity form to be specified prior to modeling
- ***Neural Nets***: powerful nonlinear regression techniques inspired by theories about the brain
  - Hidden unit is a LC of the original predictors, but unlike ***PLS*** they are not estimated in a hierarchical fashion
  - The LC Is usually transformed by a nonlinear function 
- Note that, unlike the LC’s in ***PLS***, there are no constraints that help define the LC’s.  As a result it is difficult to determine what information the coefficients in each unit represent
- Treating this model as a non-linear regression model, the parameters are usually optimized to minimize the sum of squared residuals
  - Can be challenging optimization problem since there are no constraints on the parameters of this complex model
  - Common that a solution to the equation is not a *global* solution
- ***NN’s*** have the tendency to overfit.  Add *weight decay* to regularize
- Reg coeffs are being summed, they should be on the same scale; hence features should be centered and scaled before modeling
- ***NN*** models have often been negatively affected by high correlation among features
- ***MARS*** uses surrogate features instead of the original features, but does so by creating two contrasted versions of a predictor to enter the model
  - The scheme creates a piecewise linear model where each new feature models an isolated portion of the original data
  - *How are cut points determined?*…Each data point for each predictor is evaluated as a candidate cut point by creating a linear regression model with the candidate features, and the corresponding model error is calculated
  - The predictor/cut point combo that achieves the smallest error is then used for the model
  - One predictor has all values less than the cut point set to zero and values greater than the cut point are left unchanged.  The second feature is the mirror image of the first.  Instead of the old data, these two new features are used to predict the outcome in a regression model
  - Pair of hinge functions written as h(x-a) and h(a-x)
- Prior to pruning, each pair of hinge functions is kept in the model despite slight reduction in estimated RMSE
- Once the full set of predictors has been created, the alg sequentially removes individual features that do not contribute significantly to the model
- The process above is a description of an additive MARS model, but we can build ***MARS*** models where the features involve multiple predictors at once 
  - With a second-deg ***MARS*** model, the alg conducts the same search of a single term that improves the model, and, investigates another search after the initial pair of features
- There are two tuning params associated with ***MARS***: the degree of the features that are added to the model and # of retrained terms
- Several advantages to ***MARS***: 
  - (1) automatic feature selection, ***MARS*** potentially thins the feature set using the same alg that builds the model
  - (2) interpretability, each hinge feature is responsible for modeling a specific region in the predictor space using a (piecewise) linear model (case on whether model is additive or not to show interpretability property holds)
  - (3) the technique requires very little pre-processing (e.g.: zero var pred will never be selected for a split)
- One method to help understand the nature of how the predictors affect the model is to quantity their importance to the model
  - One technique to do this is to track the reduction in the RMSE (by measuring the GCV statistic)
- ***SVMs*** are a class of powerful, highly flexible modeling techniques
  - For regression this technique is motivated in the context of *robust regression* 
- Recall LR tries to find params that minimize SSE.  Drawback is that outliers can have a huge effect on parameter choice
  - Alt loss function: Huber function (uses squared residuals when they are “small” and uses absolute residuals when the residuals are large)
- ***SVM*** use a similar function to Huber loss, with an important difference.  Given a threshold set by the user (eps), data points with resids < threshold do not contribute to the regression fit while data points with resids > threshold contribute a linear scale amount
= If the threshold is set to a relatively large value, then the outliers are the only points that define the regression line
- Points worth noting
  1. From the standpoint of classical regression this model would be considered *over-parameterized*; typically better to estimate fewer params than data points
  2. The individual training set data points are required for new predictions.  When the training set is large, this makes the prediction equations less compact than other techniques. 
    - However, for some % of the training set samples, the alpha_i params will be zero => no impact on on the pred
    - The data points associated with an alpha_i = 0 are the training set examples within +/- eps of the regression line => subset of training set data points with alpha =/= 0, are needed for prediction.  These points are used to determine the regression line, and as a result are called *support vectors* 
- *Which kernel should be used?*…Radial basis function has been shown to be very effective, but if the true relationship is linear the linear kernel is a better choice.  # of tuning parameters dependent on which kernel is selected
- Cost parameter is the main tool for adjusting the complexity of the model 
- Can tune eps, but in practice fixing eps and tuning cost is better
- Center and scale predictors to make sure distance metrics for ***KNN*** are not
  - Method has problems for missing data b/c all values are needed
  - Cross val for K 
- ***KNN*** can have poor predictive performance when local predictor structure is not relevant to the response.  Irrelevant or noisy predictors can cause distance between “close” samples => removing irrelevant features is a key pre-process step

### Chapter 8: Regression Trees and Rule-Based Models
- Tree-based models consist of one or more nested if-then statements for the predictors that partition the data
- *CART*: For regression, the model begins with the entire data set, S, and searches every distinct value of every predictor to find the predictor and split value that partitions the data into two groups (S_1 and S_2) such that the overall SSE is minimized
  - In practice, the process continues within sets S_1 and S_2 until the number of samples in the splits falls below some threshold (such as 20 samples) 
  - Once full tree has been grown, the tree may be very large and is likely to over-fit the training set.  Tree is then *pruned* back to a potentially smaller depth.  One method is *cost-complexity tuning* (penalize error rate using size of tree)
- Tree models intrinsically conduct feature selection.  If a predictor is never used in a split, the prediction equation is independent of the data 
- 2 preds very correlated => the choice of which to use is somewhat random 
- Single regression trees tend to perform sub-optimally
- Trees suffer from selection bias, predictors with a higher number of distinct values are favored over more granular preds 
- Limitation of simple regression tress is that each terminal node uses the average of the training set outcomes in that node for prediction
- We examine *model* tree approach called M5, which is similar to regression trees except 
  - The splitting criterion is different 
  - The seminal nodes predict the outcome using a linear model 
  - When a sample is predicted, it is often a combination of the predictions from different models along the same path through the tree
- Like simple regression trees, the initial split is found using an exhaustive search over the predictors and training set samples, but unlike those models, the expected reduction in the node’s error rate is used  
- Create complete set of linear models, and then use simplification procedure to drop terms
- Model trees incorporate a type of smoothing to decrease the potential for over-fitting.  Technique based on “recursive-shrinking“ methodology.  
- This type of smoothing can have a significant positive effect on the model tree when the linear models across nodes are very different.  
- Several possible reasons that the linear models may produce very different predictions
  1.  The # of training set samples available at a node will decrease as new splits are added.  Can lead to nodes which model very different regions of the training set and, thus, produce very different linear models 
  2.  The linear models derived by the splitting process may suffer from significant collinearity
- *Bagging (bootstrap aggregation)*, was originally proposed by Leo Breiman and was one of the earliest ensemble techniques
  - Advantage: Aggregating over many versions of the training data reduces variance.  The decrease in variance makes the prediction more stable
- *Bagging* Algorithm: 
  1.  ***for i = 1 to m do*** 
  2.  *Generate a bootstrap sample of the original data *
  3.  *Train an unpruned tree model on this sample *
  4.  ***END***
- Bagging stable, lower variance models like ***LR*** and ***MARS***, offers less improvement in predictive performance
- Another advantage of of bagging models is that they can provide their own internal estimate of predictive performance that correlates well with cv estimates or test set estimates (using the out-of-bag examples)
- In basic form, the user has one choice to make for bagging: the num of bootstrap samples to aggregate, m. Most improvement is made with a small num of trees (m < 10)
- Small improvements can be made up to m = 50.  No improvement after that?….use more powerful methods (***RF*** or ***Boosting***)
- Disadvantages: 
  1.  Computational costs can be annoying, which is where using parallel architecture comes in handy.  Since all bootstrapped samples can be done independently of one another
  2.  Lack of interpretability 
- Variance reduction provided by bagging can be improved since the trees are correlated (even though they are unique)
- De-correlating trees is the logical next step 
- Random Forest Alg: Same as bagging all, but after generating a bootstrap sample select k ( < P) of the original predictors and select the best predictor among the k predictors and partition the data.  Then use typical tree model stopping criteria to determine when a tree is complete (but do not prune)
- Practically speaking, the larger the forest, the more computational burden we will incur to train and build the model.  As a starting point, we suggest using at least 1,000 trees
- If the CV performance profiles are still improving at 1,000 trees, then incorporate more trees until performance levels off
- Compared to bagging, random forests is more computationally efficient on a tree-by-tree basis since the tree building process examines only a fraction of the original predictors at each split (but in general more trees are needed for random forests)
- Combining this attribute with the ability to parallel process tree building makes random forests more computationally efficient than boosting
- Like ***bagging***, CART or conditional inference trees can be used as the base learner in ***random forests***.  Both of these base learners were used
- *m_try*: The number of predictors being split on at each point in the tree
  - models tend to be robust to choice of parameter after *m_try* = 10.  
  - To get a quick assessment of how well the RF model performs, the default tuning param is P / 3.  
  - If there is a desire to maximize performance, tuning this value may result in a slight improvement
- Ensemble learning nature of ***RF*** makes it impossible to gain an understanding of the relationship between the predictors and response, but we can quantify the impact of predictors in the ensemble
  - Diff in predictive performance between the non-permuted sample and the permuted sample for each feature is recorded and aggregated across the forest
  - Another approach: Measure improvement in node purity based on the performance metric for each predictor at each occurrence of that predictor across the forest 
- Between pred correlations dilute the importances of key predictors
- ***Boosting*** began with the AdaBoost algorithm and is the boosting alg of choice among practitioners 
- Boosting can be interpreted as a forward stagewise additive model that minimizes exponential loss.  Initially used to help build better classification models, but can also be applied to regression problems
- Because ***boosting*** requires a weak learner, almost any technique with tuning params can be made into weak learners
  - Trees make great candidates for weak learners bc they can be made weak by simply restricting the depth of the tree
  - When reg trees are used as the base learner, simple gradient boosting for regression has two tuning params: tree depth (interaction depth in this context) and # of iterations 
- ***GBM*** uses a greedy algorithm to select the best weak learner at each stage => might not pick best optimal weak learner.  Remedy for greediness is regularization, for shrinkage
  - Instead of adding the predicted value for a sample to previous iterations predicted value, only a fraction of the current predicted value is added to the previous iteration’s predicted value => lambda is a tuning parameter
- Variable importance for boosting is a function of the reduction in squared error.  Specifically, the improvement in squared error due to each predictor is summed within each tree in the ensemble
  - The improvement values for each feature are then averaged across the entire ensemble to yield an overall importance value
- The importance profile for boosting has a much steeper importance slope that the one for RFs (because the trees in this ensemble method are dependent on each other)
- Not a bad thing, just different perspectives
- ***Cubist***
  - If the variance of the parent model’s error is larger than the covariance, the smoothing procedure tends to weight the child more than the parent
  - Conversely, if the variance of the parent model is low, the model is given more weight
  - Collects the sequence of linear models at each node into a single, smoothed representation of the models so that there is one linear model associated with each rule
  - Unlike boosted tree model, there is more gradual decrease in importance for data; there is not a small subset of features that dominate the model fit
  - Uses Manhattan distances to determine the nears 

### Chapter 11: Measuring Performance in Classification Models 
- This chapter examines strategies for evaluating classification models using statistics and visualizations
- Like regression models, classification models produce a continuous valued prediction, which is usually in the form of a probability.  In addition to a continuous probability, classification models generate a predicted class, which comes in the form of a discrete category
- In some applications, the desired outcome is the predicted class probabilities which are then used as inputs for other calculations
- Softmax: no probability statement is being created by the outcome.  It just makes sure that the predictions have the same mathematical qualities as probabilities
- One way to assess the quality of class probs is using a *calibration plot*. For a given set of data, this plot shows some measure of the observed probability of an event versus the predicted class probability
  - One approach: score a collection of samples with known outcomes (preferably a test set) using a classification model.  Bin the data into groups based on their class probs.  For each bin, determine the observed event rate.  The plot would display the midpoint of the event on the x-axis, and the observed event rate on the y-axis.  45 deg line => well calibrated probabilities
- Three or more classes => use *heat map* of class probabilities to gauge the confidence in the predictions
- Can improve classification performance by using an *intermediate zone* (where class is not formally predicted when the confidence is not high)
- Assuming a fixed level of accuracy for the model, there is usually a trade-off between sensitivity and specificity
  - These tradeoffs might be good depending on the different penalties associated with each type of error
- The most common method for combining sensitivity and specificity into a single value uses the ROC curve
- Sensitivity and specificity are conditional measures.  The person using the model predictions is typically interested in unconditional queries
- ROC curves were designed as a general method that, given a collection of continuous data points, determine an effective threshold such that values above the threshold are indicative of a specific event.  Here we examine how the ROC curve can be used for determining alternate cutoffs for probabilities (changing threshold)
- Altering the threshold only has the effect of making samples more positive (or negative as the case may be).  In the confusion matrix it cannot move samples out of *both* off-diagonal table cells
- Superimpose ROC curves to visually compare different models.  Comparing ROC curves can be useful in contrasting two two or more models with different predictor sets (for the same model), different tuning parameters (for model comparisons), or different classifiers (between models)
- One advantage of using ROC curves to characterize models is that the curve is insensitive to disparities in class proportion
- One disadvantage of using the AUC to evaluate models is that it obscures information.  Common for no ROC curve to be better than another
- Lift charts are a visualization tool for assessing the ability of a model to detect events in a data set with two classes
- Lift charts rank the samples by their scores and determine the cumulative event rate as more samples are evaluated

### Chapter 12: Discriminant Analysis and Other Linear Classification Models 
- This chapter focusers on separating samples into groups based on characteristics of predictor variation
- One of the first steps of the model building process is to transform, or encode, the original data structure into a form that is useful for the model (*feature engineering*)
- Some methods are sensitive to data that is sparse and unbalanced, so often times two different sets of predictors are used depending on the model
- One common method for quantifying the predictive ability of as binary predictor is the odds
- Data splitting scheme should take into account the *application domain of the model*  
- Maximum likelihood parameter estimation is a technique that can be used when we are willing to make assumptions about the probability distribution of the data.  Based on the theoretical prob distribution and the observed data, the likelihood function is a prob statement that can be made about a particular set of parameter values
- ***Logistic regression*** and ordinary linear regression fall into a larger class of techniques called ***GLMs***
  - Linear in the sense that some function of the outcome is modeled using the linear predictors, but they often have nonlinear equations
- Using the training data, the model fitting routine searches across different values of parameters to find a combination that given the training data, maximizes the likelihood of the binomial distribution 
- An effective logistic regression model would require an inspection of how the success rate related to each of the continuous predictors may parameterize the model terms to account for nonlinear effects
- For ***LogR***, formal statistical hypothesis tests can be conducted to assess whether the slope coefficients for each predictor are statistically significant.  A *Z* statistic is commonly used for these models, and is essentially a measure of the signal-to-noise ratio: 
  1. The estimated slope is divided by its corresponding error
  2. Use the statistic to rank the predictors and see which terms had the largest effect on the model
- ***LogR*** can be effective when the goal is only prediction, but it does require the user to identify effective representations of the predictor data that yield the best performance.  
  - There are methods that empirically derive these relationships during training
  - Model will only be utilized for prediction => these techniques may be more advantageous
- When using ***LDA*** it is common to assume the predictors follow a multivariate normal distribution.  Further, if we assume that the means of the groups are unique, but the covariance matrices are identical across groups, we can find the linear discriminant function of the lth group
- Fisher thought of this as finding LCs of the features such that the between-group variance was maximized relative to the within-group variation 
- The mathematical optimization constrains the max # of discriminant functions that can be extracted to be the lesser of the number of predictors or 1 less than the # of groups.  Similarly to ***PCA***, the eigenvalues in this problem represent the amount of variation explained by each component
  - ***LDA*** will not work when # preds > # samples || features are highly correlated
  - As a result people usually perform PCA before using LDA
- In practice, the # of discriminant vectors is a tuning parameter that we would estimate with cross val
- ***LDA*** finds an optimal discriminant vector.  Geometrically, if the # of samples equals the # of predictors (or dimensions), then we can find at least one vector that perfectly separates the samples.  As long as the samples are not in the same location, then we can find a vector that perfectly separate the two samples.  This can lead to *poor* class prob estimates
  - ***LDA*** should only be used on data sets that have at least 5 - 10 times more samples than predictors
- Transforms of or interactions among predictors should only be included if there is good reason to add them.  If there is a good reason to believe that meaningful predictive information exists through the additional predictors and/or interactions
  - Suspect nonlinear structure exists, but not sure how?  Use non-linear methods.  The predictive power of ***LDA*** quickly degrades when uninformative features are used
- ***PLS*** is, seeking to find optimal group separation while being guided by between-group info.  In contrast, ***PCA*** seeks to reduce dimension using the total variation as directed by the overall covariance matrix of the features
- Like ***PLS*** for regression there is one tuning parameter, the # of latent variables
- In practice the softmax approach does not produce probabilities that are close to 0 or 1.  Alternative approach is to use *Baye’s Rule*, which is great for specifying prior probabilities for severely imbalanced classes

### Chapter 13: Nonlinear Classification Methods 
- The techniques in this chapter can be adversely affected when a large number of non-informative predictors are used as inputs (domain knowledge important with these techniques)
- In pure mathematical optimization terms, ***LDA*** and ***QDA*** each minimize the total probability of misclassification assuming that the data can truly be separated by hyperplanes or quadratic surfaces.  In reality the decision boundary may be somewhere in between this, which is where ***RDA*** comes into play
  - If a model is turned over lambda, then a data-driven approach can be used to choose between linear or quadratic boundaries as well as boundaries that fall between the two
  - ***RDA*** makes another generalization of the data: the pooled covariance matrix can be allowed to morph from its observed value to one where the predictors are assumed to be independent
  - Tuning this model over lambda and gamma enables the training set data to decide the most appropriate assumptions for the model 
- ***MDA*** was developed as an extension of ***LDA***: allows each class to be represented by multiple multivariate normal distributions.  Can have different means but, like LDA, the covariance structures are assumed to be the same
  - The class-specific distributions are combined into a single multivariate normal distribution by creating a per-class mixture
- ***NN***: Entropy or cross-entropy, has more theoretical validity than the squared error approach
- ***FDA***: Many models such as LASSO, RR, and MARS can be extended to create discriminant variables 
- ***FDA*** Alg 
  1.  Create a new response matrix of binary dummy variable columns for each of the C classes 
 2.  Create a multivariate regression model using any method that generates slopes and intercepts for predictors or functions of the predictors 
  3.  Post-process the model parameters using the optimal scoring technique
  4.  Use the adjusted regression coefficients as discriminant values
- Bagging ***FDA*** and ***MARS*** has marginal impact 
FINISH LATER

### Chapter 16: Remedies for Severe Class Imbalance
- When modeling discrete classes, the relative frequencies of the classes can have a significant impact on the effectiveness of the model
- When classes are balanced they are unlikely to have similar ROC and Lift plots
- There are a multiple strategies for overcoming class imbalances: 
  - Model tuning can be used to increase the sensitivity of the minority class
  - Alternate probability cutoffs can be derived from the data to improve the error rate on the minority class.  This is post-processing the model predictions to redefine the class predictions
    1.  If there is a particular target that must be set for the sensitivity or specificity, this point can be found on the ROC curve and the corresponding cutoff can be determined
    2. Find the point on the ROC curve that is closest to the perfect model (100% sens., 100% spec.)
    3. Youden’s J Index: measures the proportion of correctly predicted samples for both event and nonevent groups
- You could also adjust the prior probabilities and the unequal case weights
- When there is a priori knowledge of class imbalance, one straightforward method to reduce its impact on model training is to select a training set sample to have roughly equal event rates during the initial data collection.  This eliminates the imbalance problem that plagues model training, but the test set should be sampled in a way that is reflective of the imbalance
- Two general post-hoc techniques are *down-sampling* and  *up-sampling* 
- *Down-sampling*: selects the data points from the majority class so that the majority class is roughly the same size as the minority class.  There are several approaches 
  - Randomly sample the majority classes so that all classes have approximately the same size
  - Another approach is to take a bootstrap sample across all cases such that the classes are balanced in the bootstrap set.  The advantage of this approach is that the bootstrap selection can be run many times so that the estimate of variation can be obtained about the down-sampling
  - One implementation of random forests can inherently down-sample by controlling the bootstrap sampling process within a stratification variable
- Instead of optimizing the typical performance measure, such as accuracy or impurity some models can alternatively optimize a cost or loss function that differently weights specific types of error
  - EG: It may be appropriate to believe that misclassifying true events is X times as costly as incorrectly predicting non-events
  - Unlike alt cutoffs, unequal costs can affect the model parameters and thus have the potential to make true improvements to the classifier
- FINISH LAST FEW PAGES AFTER CHAPTER 13

### Chapter 19: Introduction to Feature Selection 
- Models with less features may be more interpretable and less costly (especially if there is a cost to measuring the predictors)
- Some models are naturally resistant to non-informative features.  Tree- and rule based methods, MARS and the LASSO, for example, intrinsically conduct feature selection
- Supervised vs. Unsupervised methods…both have different problems
- ***LASSO***, ***MARS***, and ***Tree*** based methods tend to not be effected very little by additional uninformative predictors
- Parametrically structured models such as ***LR***, ***PLS***, and ***NN** are severely effected
- *Wrapper methods*: evaluate multiple models using procedures that add and/or remove predictors to find the optimal combination that maximizes model performance.  Search algs that treat features as inputs and use model performance as the output to be optimized
- *Filter methods*: Evaluate the relevance of the predictors outside of the predictive models and then only model the predictors that pass some criterion  
- *Wrapper Method*
  - Forward selection: greedy algorithm that determines whether or not predictors should be added
  - Backward selection: Initial model has all predictors, which are then removed to determine which are not significantly contributing to the model
  - These methods can be improved using non-inferential criteria, such as the AIC statistic
- For ***LR*** a commonly used statistic is the *AIC*.  Penalized version of sum of squares (simplicity favored over complexity)
- Methodologies based on dubious statistical principles can still result in great models.  The key is to to have a thorough validation process
- *Recursive feature elimination* is a backward selection method that avoids refitting many models at each step of the search
- *Genetic algorithms*: Optimization tool based on evolutionary principles of population biology
  - In the context of feature selection, the chromosome is a binary vector that has the same length as the number of predictors in the data set.  
  - Every binary entry of the chromosome, or gene represents the presence or absence of each predictor in the data
  - GAs are tasked with finding optimal solutions from the 2^n possible combinations of predictor sets
  - Random perturbation of the genetic material helps ensure the alg doesn’t get trapped in a local optimum, but does not 
- Most *filter based* methods are univariate, which often results in highly correlated predictors.  Collinearity problems can rise as a result
- The uncertainty induced by feature selection can be much larger than the uncertainty of the model (once the features have been selected).  
- Error: When the feature selection is not considered as part of the model-building process
  - It should be included within the resampling procedure so that the variation of feature selection is captured in the results
- To properly resample the feat. selection process, an outer resampling loop is needed that encompasses the entire process.  
- Risk of over-fitting not confined to recursive feature selection or wrappers in general.  Other situations that increase the likelihood of selection bias: 
