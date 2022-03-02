## Pattern Recognition and Machine Learning 

##### **C. Bishop (2006)**

Summary: Free book on Deep Learning

### Definitions 
- **Generative model**: a statistical model of the joint probability distribution on X x Y 
- **Discriminative model**: Model of the conditional probability of the target Y, given an observation x (classifiers computed without using a probability model are also loosely referred to as discriminative)
- **Memorylessness (Markov processes)**: An assumption which implies that the properties fo RVs related to the future depend only on relevant information about the current time, not on information from further in the past
- **State space models**: Models that use state variables to describe a system by a set of first-order differential or difference equations, rather than by one or more nth-order differential or difference equations.  State variables can be reconstructed from the measured input/output data, but are not themselves measured during an experiment 
- **Mixture model**: A probabilistic model for representing the presence of subpopulations within an overall population, without requiring that an observed data set should identify the sub-population to which an individual observation belongs
- **Emission probabilities**: Output probabilities of a HMM
- **Stochastic**: Randomly determined; having a random probability distribution or pattern that may be analyzed statistically but may not be predicted precisely


### Chapter 13 Sequential Models
- For many applications the i.i.d assumption will be a poor one.  One such class of datasets where this is the case is sequential datasets
- They often arise through measurement of time series, for example the rainfall measurements on successive days at a particular location, or the daily values of a currency exchange rate, or the acoustic features at successive time frames used for speech recognition
- In the **stationary** case, data evolves in time but the distribution from which it is generated remains the same (we primarily examine this case)
- For the more complex **non-stationary** situation, the generative distribution itself is evolving with time
- It would be impractical to consider a general dependence of future of observations because the complexity of such a model would grow without limit as the number of observations increases
- This leads us to consider Markov models in which we assume that future predictions are independent of all but the most recent observations
- Here we focus on the two most important examples of state space models, namely the hidden Markov model, in which latent variables are discrete, and linear dynamical systems, in which the latent variables are Gaussian

#### Markov Models
- Easiest thing to do is ignore the sequential aspects and treat the observations as i.i.d, but this would fail to exploit sequential patterns int he data, such as correlations between observations that are close in the sequence
- To express such effects in a probabilistic model, we need to relax the iid assumption, and one of the simplest ways to do this is to consider a Markov model
- A first order Markov chain of observations in which the distribution of a particular observation is conditioned on the value of the previous observation
- We can represent sequential data using a Markov chain of latent variables, with each observation conditioned on the state of the corresponding latent variable (can similarly consider extensions to an M-th order Markov chain)
  - If the variables are **discrete**, and if the conditional distribution are represented by general conditional probability tables, then the number of parameters grows exponentially with M
  - For **continuous** variables, we can use linear-Gaussian conditional distributions in which each node has a Gaussian distribution whose mean is a linear function of its parents.  This is known as an autoregressive or AR model
- Alternative approach is to use a parametric model for p(x_n | x_{n - M}, …, x_{n-1}) such as a neural network.  This technique is sometimes called a tapped delay line because it corresponds to delaying the previous M values of the observed variable in order to predict the next value 
  - The number of parameters can then be much smaller than in a completely general model, although this is achieved at the expense of a restricted family of conditional distributions
- If we want to build a model for sequences that is not limited by the Markov assumption to any order and yet that can be specified using a limited number of free params we can achieve this by introducing additional latent variables to permit a rich class of models that can be constructed out of simple components 
  - For each observation x_n, we introduce a corresponding latent variable z_n (can be of different type or dimensionality to the observed variable) Ltapp
- There are two important models for sequential data that are described by this graph.  If the latent variables are discrete, then we obtain the hidden Markov Model (HMM).  Not the observed variables can be discrete or continuous, and a variety of different conditional distributions can be used to model them
- If both the latent and the observed variables are Gaussian (w/ a linear-Gaussian dependence of the conditional distributions on their parents), then we obtain the linear dynamical system

#### Hidden Markov Models
- If we examine a single time slice of the model, we see that it corresponds to a mixture distribution, with component densities given by p(x|z)
- HMM is commonly used for the analysis of biological sequences such as proteins and DNA 
- As is the case of a standard mixture model, the latent variables are the discrete multinomial variables z_n describing which component of the mixture is responsible for generating the corresponding observation
- Sometimes useful to take a state transition diagram and unfold it over time.  This gives an alternative representation of the transitions between latent spaces, known as *lattice* or *trellis* diagram
- The specification of the probabilistic model is completed by defining the conditional distributions of the observed variables 
