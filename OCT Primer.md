## OCT Primer

### Sources
- A Complete List of Ocular Diseases w/ Optical Coherence Tomography (Daniel Ephstein) 

### Wikipedia Article
- Subfield of ML in which multiple learning tasks are solved at the same time, while exploiting commonalities and differences across tasks
- Works by learning tasks in parallel while using a shared representation
- MTL works because regularization induced by requiring an algorithm to perform well on a related task can be superior to regularization that prevents overfitting by penalizing all complexity uniformly
- Helpful if the tasks share significant commonalities and are generally slightly under sampled, but it has also been shown to be beneficial for learning unrelated tasks
- In many tasks, joint learning of unrelated tasks which use the same input data can be beneficial.  The reason is that prior knowledge about ask relatedness can lead to sparser and more informative representations for each task grouping
- Additionally, the programmer can impose a penalty not asks from different groups which encourages the two representations to be orthogonal
- Related to MTL is the concept of knowledge transfer.  Traditional MTL implies a shared representation learned concurrently, TOK implies a sequentially shared representation
  - An image-based object classifier, can develop robust representations which may be useful to further algorithms learning related tasks
- Traditionally MTL and transfer of knowledge are applied to stationary learning settings.  Their extension to non-stationary environments is termed Group online adaptive learning (GOAL)
  - Sharing info could be particularly useful if learners operate in continuously changing environments.  Such group-adaptive learning has numerous applications from predicting financial time-series, through content recommendation systems

### An Overview of MTL in DNN”s 

#### Abstract
- Article gives a general overview of MTL, particularly in DNN’s 
- Seeks to help ML practitioners apply MTL by shedding light on how MTL works and providing guidelines for choosing appropriate auxiliary tasks 

#### Introduction
- MTL has been used successfully across all applications of ML from NLP and speech recognition to computer vision and drug discovery
- Generally, as soon as you find yourself optimizing more than one loss function, you are effectively doing MTL.  In those scenarios, it helps to think about what you re trying to do explicitly in terms of MTL and to draw insights from it
- “MTL improves generalization by leveraging the domain-specific information contained in there training signals of related tasks”
- **S2**: MTL motivation, **S3**: Two Most frequently employed methods for MTL in deep learning, **S4**: Description of mechanisms that illustrate why MTL works in practice. **S5**: literature in ML

#### Two MTL methods for DL
- Most common ways to perform MTL in DNN’s is typically done with either hard or soft parameter sharing of hidden layers
- Hard parameter sharing is the most commonly used approach to MTL in NN’s.  It is generally applied by sharing the hidden layers between all tasks, while keeping several specific output layers
- In soft parameter sharing each has its own model with its own parameters.  The distance between the parameters of the model is then regularized in order to encourage the parameters to be similar for instance using l_2 distance for regularization 

#### Why does MTL work? 
- Inductive-bias obtained through MTL seems intuitively plausible, but in order to understand MTL we must look at the mechanisms
- MTL effectively increasers the sample size that we are using for training a model.  Different tasks have different noise patterns, so a model that simultaneously is able to to learn a more general representation
- MTL can help the model focus its attention on those features that actually matter as other tasks will provide additional evidence for the relevance or irrelevance of those features

#### Recent work on MTL for Deep Learning 
- While many recent DL approaches have used MTL - either explicitly or implicitly - as part of their model they all employ the two approaches mentioned earlier
