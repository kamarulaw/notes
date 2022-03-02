## Deep Learning 

##### **I. Goodfellow, Y. Bengio, A. Courville (2016)**

Summary: Free book on Deep Learning

### Definitions 
- **Precision**: The fraction of detections reported by model that were correct
- **Recall**: The fraction of true events that were detected (TPR)
- **F-Score**: One way to summarize the performance of the classifier with a single number as a function of precision and recall 
- **Coverage**: The fraction of examples for which the machine learning system can produce a response

### Chapter 10: Sequence Modeling - Recurrent and Recursive Nets
- Family of neural networks for processing sequential data
- Most recurrent networks can also process sequences of variable length 
- Some examples of important design patterns for RNNs include the following 
  1) RNN’s that produce an output at each time step and have recurrent connections between hidden units
  2) RNN’s that produce an output at each time step and have recurrent connections only from the output at one time step to the hidden units at the next time step
  3) RNN”s with recurrent connections between hidden units, that read an entire sequence and then produce a single output
- The BPTT algorithm is linear in runtime and space consumption.  Thus the network with recurrence between hidden layers is thus very powerful, but also very expensive
- The network with recurrent connections only from the output at one time step to the hidden units at the next time step is strictly less powerful because it lacks hidden-to-hidden recurrent connections
- Models that have recurrent connections from their outputs leading back into the model may be trained with teacher forcing.  Teacher forcing is a procedure that emerges from the MLE criterion, in which during training the model receives the ground truth output of time step t at time t + 1

#### 10.5: Deep Recurrent Networks 
- The computation in most RNNs can be decomposed into three blocks of parameters and associated transformations
  1) from the input to the hidden state
  2) from the previous hidden state to the next hidden state, and 
  3) from the hidden state to the output
- Evidence shows it is advantageous to introduce depth in each of these operations

#### 10.7: The Challenge of Long-Term Dependencies 
- The basic problem is that gradients propagated over many stages tend to either vanish (most of the time) or explode (rarely, but with much damage) 
- Write hidden function at time t as a recurrent relation with a weight matrix W
- If W admits an eigendecomposition, then the recurrence will result in the eigenvalues being raised to the power of t
- Whenever a model is able to represent long-term dependencies, the gradient of a long-term interaction has exponentially smaller magnitude than the gradient of a short-term interaction, so while it is not impossible to learn it might take a very long time to learn long-term dependencies (b/c the signal about these dependencies will tend to be hidden by the smallest fluctuations arising from short-term dependencies) 

#### 10.9 Leaky Units and Other Strategies for Multiple Time Scales
- One way to deal with long-term dependencies is to design a model that operates at multiple time scales (fine-grained vs. coarse)
- Various strategies for building both fine and coarse time scales are possible.  These include the addition of skip connections across time, “leaky units“ that integrate signals with different time constants, and the removal of some of the connections to model fine-grained time scales

##### 10.9.1 Adding Skip Connections through Time
- One way to obtain coarse time scales is to add direct connections from variables in the distant past to variables in the present
- This allows the learning algorithm to capture longer dependencies, although not all long-term dependencies may be represented well in this way

##### 10.9.2 Leaky Units and a Spectrum of Different Time Scales 
- Another way to obtain paths on what the product of derivatives is close to one is to have units with linear self-connections and a weight near one on these connections.  The parameter on the self connection can be used to determine how long past information is remembered (close to 1 => remembered for long time, close to 0 => discarded quickly)
- Self connections are a nice way to ensure this effect takes places smoothly bc we are adjusting the real-valued weight instead of the integer number of time steps like we do in skip connections

##### 10.9.3 Removing Connections
- Another approach to handle long-term dependencies is the idea of organizing the state of the RNN at multiple time-scales, with information flowing more easily through long distances at the slower time scales
- Different ways in which a group of recurrent units can be forced to operation at different time scales
  1) Make the recurrent units leaky, but to have different groups of units associated with different fixed time scales
  2) Have explicit and discrete updates taking place at different times, with a different frequency for different groups of units

#### 10.10 LSTM and other Gated RNNs
- The most effective sequence models used in practical applications are called gated RNNs.  These include the long short-term memory and networks based on the grated recurrent unit
- Gated RNNs are based on the idea of creating paths through time that have derivatives that neither vanish nor explode
- Like leaky units, gated RNNs are based on the idea of creating paths through times that have derivatives that neither vanish nor explode
- Leaky units did this with connection weights that were either manually chosen constants or were parameters.  Gated RNNs generalize this to connection weights that may change at each time stepd
- Gated RNNs generalize this to connection weights that may change at each time step

##### 10.10.1 LSTM 
- The idea of introducing self-loops to produce paths where the gradient can flow for long durations is a core contribution of the initial LSTM model 
- A crucial addition has been to make the weight on this self-loop conditioned on the context, rather than fixed
- By making the weight of the self-loop gated the time scale of integration can be changed dynamically
- In this case, we mean that even for an LSTM with fixed parameter, the time scale of integration can change based on the input sequence, because the time constants are output by the model itself
- (See Fig 10.16) Cells are connected recurrently to each other, replacing the usual hidden units of ordinary recurrent networks
  - An input feature is computed with a regular artificial neuron unit.  Its value can be accumulated into the state if the sigmoidal input gate allows it
  - All of the gating units have a sigmoid nonlinearity, while the input unit can have any quashing nonlinearity.  The state unit can also be used as an extra input to the gating units
- Instead of a unit that simply applies an element-wise nonlinearity to the affine transformation fo inputs and recurrent units, LSTM recurrent networks have “LSTM  cells” that have an internal recurrence (a self-loop) in addition to the outer recurrence of the RNN
- Te most important component is the state unit (s_i^(t)) that has a linear self-loop similar to the leaky units described in the previous section.  However here, the self-loop weight is controlled by a forget gate that sets weight to a value between 0 and 1 via a sigmoid unit
- The external input gate unit g_i^(t) is computed similarly to the forget gate, but with its own parameters
- The output h_i^(t) of the LSTM cell can also be shut off, via the output gate q_i^(t) which also uses a sigmoid unit for gating
- There are multiple variants one can choose to use the cell state

##### 10.10.2 Other Gated RNNs
- Which pieces of the LSTM architecture are actually necessary?  What other successful architectures could be designed that allow the network to dynamically control the time scale and forgetting behavior of different units? 
- Some answers to these questions are given with the recent work on gated RNNs, whose units are also known as gated recurrent units or GRUs.  The main difference with the LSTM is that a single gating unit simultaneously controls the forgetting factor and the decision to update the state unit
- Several investigations over architectural variations of the LSTM and GRU found no variant that would Cleary beat both of these across a wide range of tasks
- One paper found that a crucial ingredient is the forget gate, while another paper found that adding a bias of 1 to the LSTM forget gate makes the lSTM as strong as the best of the explored architectural variants

#### 10.11 Optimization for Long-Term Dependencies
- A continuing theme in machine learning is that it is often much easier to design a model that is easy to optimize than it is to design a more powerful optimization algorithm

### Chapter 11: Practical Methodology
- Successfully applying deep learning techniques requires more than knowledge of how the algorithms work
- During day to day development of machine learning systems, ML Engineers need to decide whether to gather more data, increase or decrease model capacity, add or remove regularizing features, improve the optimization of a model, improve approximate inference in am model, or debug the implementation of the model 
- Follow this general system 
  1. Determine goals - error metric to use? target value for this error metric? should be driven by the problem that application is intended to solve 
  2.  Establish a working end-to-end pipeline ASAP, including the approximation of the performance metrics
  3.  Diagnose which components are performing worse than expected and whether it is due to overfitting, underfitting, or a defect in the data or software
  4.  Repeatedly make incremental changes such as gathering new data, adjusting hyper parameters, or changing algorithms, based on specific findings 
- Default Baseline Models 
- Depending on complexity, you want not want to start with deep learning.  Try choosing a few linear weights, or simple logistic regression  
- Choose the general category of your model based on the structure of your data
  1.  Supervised learning w/ fixed-size vectors as inputs: FF NN w/ FC layers 
  2.  Input has known topological structure: Use a CNN 
  3.  In these cases use some kind of piecewise linear unit 
  4.  I/O is a Sequence: use a gated recurrent net (LSTM or GRU)
- Optimization Algorithms
  1.  SGD w/ momentum with a decaying learning rate (popular decay schemes: linear decay, exponential decay, decrease learning rate by a factor of 2-10 each time validation error plateaus)
  2.  Adam 
  3.  Batch Normalization (works especially well w/ CNN’s and networks w/ sigmoidal nonlins)
- With the exception of using tens of millions of training examples, some type of regularization should be used
 - In domains such as computer vision, current unsupervised techniques do not bring a benefit, except in the semi-supervised setting
- After the first end-to-end system is established, it is time to measure the performance of the algorithm and determine how to improve it.  Usually better to gather more data than to improve the learning algorithm
- training performance acceptable => measure test performance 
- test performance acceptable => done 
- test performance unacceptable => gather more data (tends to be effective solution, but can be costly)
- Alternative to gathering more data is to reduce the size of the model or improve regularization, by adjusting hyper parameters such as weight decay coefficients
- |train - test| gap too large after hyperparemter adjustment => more data
- Some HP’s affect the time and memory cost of running the algorithm.  Some of these HP’s affect the quality of the model
- Two approaches: (1) Choose HP’s manually (2) Choose them automatically 
- Automatic selection is usually more costly 
- LR is perhaps the most important learning parameter.  Effective capacity of the model is highest when it is correct - not when it is especially high or especially small
- When LR is too large, GD can inadvertently increase instead of decreasing the training error
- When LR is too small, training may become permanently stuck with a high training error 
- Tuning params other than LR requires analysis of train/test error to diagnose whether your model is overfitting or underfitting
- In many problems, optimization does not seem to be a significant barrier, provided that the model is chosen appropriately
