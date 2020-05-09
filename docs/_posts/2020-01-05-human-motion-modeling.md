---
layout: post
title:  "Stuff on human motion prediction"
date:   2020-05-01 10:51:43 +0100
categories: topic review
---
<head>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
  </script>
  <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 
</head>

<h3>Hello everyone, welcome to my first topic review!</h3>


> <h4>Why should we be interested in Human motion prediction?</h4>

To answer this question let me borrow the words of the authors of the first paper I'm going to submit to your attention. *"An important component of our capacity to interact with
the world resides in the ability to predict its evolution over time. Handing an object to another person, playing sports, or simply walking in a crowded street would be extremely challenging without our understanding of how people move, and our ability to predict what they are likely to do in the following instants. Similarly, machines that are able to perceive and interact with moving people, either in physical or virtual environments, must have a notion of how people move."*<br>

And now let's answer the second question that may have crossed your mind: 

><h4>Why am writing this topic review?</h4> 

The reason is that I am starting a new project on the topic and I'm anyway going to read a few papers on the subject. As I've recently discovered during one of my experiments on productivity sharing your work usually has the effect of pushing your quality bar higher. <br>
Now let me cut here the premises here and dive into **Human Motion Prediction**. 

Just *a quick note* before jumping into the interesting stuff: I am not going to look at the whole history on the topic, but instead only at a carefully selected set approaches, which also happen to be among the latest and most successful ones up until now. <br>
<br>
<br>

## Human motion prediction
---- 


![Human motion data]({{site.baseurl}}/assets/images/hmp.png)
<div align="center"><i>Figure taken from Martinez et al.</i></div>
<br>

One of the best ways to introduce a problem, in my opinion, is to talk about the available data. <br>
No machine learning problem can exist without data. 
<br> 
<br> 

#### The H3.6M dataset
[Human 3.6M](http://mocap.cs.cmu.edu/) (H3.6M) is a large-scale publicly available dataset including 3.6 million 3D motion-caption (abbreviated to mocap) data. 
H3.6M has constituted a widely used benchmark in human motion analysis and only [recently]() new datasets started emerging. 

The data consists of the recording of seven different actors performing 15 varied activities, such as walking, smoking, engaging in a discussion, and taking pictures. While performing these activities in front of multiple cameras, the humans wear a black jumpsuit with 41 markers taped on. The recordings are then processed to obtain a 3D representation of the movement in terms of markers position or joints-rotation for each frame.

Formally speaking, the input data ${X}$ is a sequence of length $T$ ,$\{(x_1, x_2, ..., x_T)\}$, where a frame $x_t \in{R^N}$ denotes the $N$-dimensional body pose. 
$N$ depends on the number of joints in the skeleton, $K$, and the size $M$ of the per-joint representation, i.e. $N = K · M$. <br>
A number of joint-representations has been proposed over the past few years and that of the representation scheme is a choice that has been and still is extensively discussed.<br>
A quite common (and easy to come up with) representation is known as *angle-axis* or *exponential maps*: each joint is associated with an axis $w$ (for which you need 3 numbers), and an angle $\alpha$ (at least 3 more numbers in $R^3$). Usually, the input is standardized, discarding the information on the exact rotation vector of each joint, to focus on relative rotations between joints, since they contain information of the actions. Other representation schemes include *rotation matrices, quaternions* (we'll get to this later), *or 3D positions*. 
<br> 
<br> 

#### Problem formulation 
Given a fixed length input sequence, as the one I've just introduced to you, the problem of human motion prediction can be phrased as the prediction of the future motion of the subject, both short-term and long-term. More formally, to goal of Human Motion Prediction is to find a mapping $P$ from an input sequence of length $T_1$ to an output sequence of length $T_2$.
<br> 
<br> 

#### Two different tasks
From the research on the field it has emerged that the short-term and the long-term prediction are two very different problems, and should hence be treated separately. <br>
Historically, the best models on short-term prediction perform poorly on the long-term variant and vice versa. The reason for this is that while short-term prediction is mainly about completing a movement realistically (not breaking the rules of physics for instance), long-term prediction is more about being able to recognize and continue the intention of the human actor : if someone is about to make a phone call, you don't expect them to start running, no matter how realistic the movement may seem!<br>
Short-term tasks are hence often referred to as prediction tasks and can be assessed quantitatively by comparing the model prediction to a reference recording through a distance metric. <br> 
On the other hang long-term tasks are often referred to as generation tasks and are harder to assess quantitatively. For these cases, the prediction quality is usually evaluated *by hand*.
<br>
<br> 



## The papers 
----

It's finally time to dive into some of the most insightful deep-learning approaches in the history of the topic. In this section I am going to discuss 6 selected papers, focusing on their main contributions to the field. <br> 

<div align="center">
<img src="https://media1.tenor.com/images/b6d83d66859b0cf095ef81120ef98e1f/tenor.gif?itemid=5531028"/>
</div>

<br>
<br>


### On human motion prediction using recurrent neural networks
by Julieta Martinez, Michael J. Black, and Javier Romero. <br>


I enjoyed reading this paper: the authors proposed many (though not too many) interesting ideas, that are clearly justified by a critical analysis of those that were state-of-the-art models in 2017. Too easily the scientific discourse can spiral down a dead-end, dragged by its own weight. Papers that can stir the discourse attention into a new direction, like this one, are necessary to its very progress.
<br>
<br>

# The architecture 

![The architecture]({{site.baseurl}}/assets/images/Martinez1.png)
<div align="center"><i>Figure 2 from the paper.</i></div>
<br>
The architecture is a rather classic **seq2seq** model with *GRU* cells, with the addition of *residual connections* in the decoder and, interestingly, *parameter sharing* between encoder and decoder.
> In seq2seq, two networks are trained;<br>(a) an encoder that receives the inputs and generates an internal representation, and <br>(b), a decoder network, that takes the internal state and produces a maximum likelihood estimate for prediction


# Take-away points

#### The simplicity bias 
The first and most elegant idea to take-away from this work is probably that of **simplicity**. 
> We have found a very simple baseline that outperforms sophisticated state-of-the-art methods based on deep RNNs for short-term motion prediction. 

Simplicity has been a guideline to the development of this approach, and led the authors to at least three important choices: 
- Avoiding solution whose effectiveness depend on the *art* of hyper-parameters tuning. 
- Avoiding deeper and hence more computationally expensive models. 
- Extending the specificity of the network.


And the experiments' results show that this simplicity-biased mindset has repaid their trust, reporting new state-of-the-art results for the short-term motion prediction (up to 400 ms).

This simplicity-first approach also brought them to take into consideration an easy-to-implement but until then ignored baseline, which is still today hard to beat: the **zero velocity** baseline. Behind this fancy name is simply the concept of predicting zero movement, aka predicting stillness. To the surprise of the authors this baseline beated all state-of-the-art model back then: 
>We have found a very simple baseline that outperforms sophisticated state-of-the-art methods based on deep RNNs for short-term motion prediction.


#### Sampling-based loss 

One of the problems that this model aims at solving is the typical incapacity of RNN models to recover from their own mistakes, which leads to a substantial discrepancy between training and testing performances, and hence to low robustness. <br>
To address the issue they feed to the decoder its own samples, instead of the ground truth, at each training time-step.


#### Residual connections 

This idea is the one that has brought the highest improvements on the results of their experiments, and it tackles the huge discontinuity between the conditioning sequence and prediction (aka between the first predicted frame and the last observed input). 

The idea is simple and effective: instead of modelling all possible conditioning poses so that the first frame prediction is continuous, directly model the velocity of the model. <br>
And to model velocity they propose a residual architecture, obtained by simply adding a residual connection between the input and the output of each RNN cell. 
<br> 
<br>


### Learning Human Motion Models for Long-term Predictions
by Partha Ghosh, Jie Song, Emre Aksan, Otmar Hilliges<br>


A fundamental issue that needs to be tackled when working with Human motion is that of constraining the prediction to a feasible subset of the output space. The *skeletal constraints* to human motion restrict the set of possible poses to a limited set. The problem arises when you need to force the predictions of your network into this subset.<br>
In this paper Ghosh et al. give up on directly producing a correct prediction and instead focus on training a second network to correct the mistakes of the first one.
<br>
<br>

# The architecture 

<div align="center">
<img src ="{{site.baseurl}}/assets/images/Ghosh.png/" alt="The architecture" width="500" align="center"/>
<br>
<br>
<i>Figure 1 from the paper.</i></div>
<br>

As I anticipated, the architecture is composed of two different networks: a recurrent network (*LSTM3RL*) and a *de-noising* auto-encoder (*DAE*). The former is responsible for the processing the input sequence and producing a prediction on the future sequence. Each frame of the *LSTM3RL* output is fed into the auto-encoder to be rectified. <br>
*LSTM3L* is a classic recurrent network, with 3 layers of LSTM cells (hence the name *3RL*), while *DAE* is an auto-encoder with increased dimensionality in the latent space. <br>
The two components trained separately in order to de-correlate their errors and only once trained, they are fine-tuned in an end to end fashion.
<br>


# Take-away points

#### Integrating domain knowledge 
There's still a question to solve: how can our de-noising auto-encoder incorporate domain knowledge? <br>
The answer is, in my opinion, very ingenious and simple at the same time: **dropout**! The authors incorporated dropout in the *DAE* units, and hence trained it to reconstruct the input only by looking at a fraction of the total number of joints. This approach could not have worked if the joints were completely independent of one another: by randomly masking part of the input they forced the auto-encoder to learn the relationship between different joints. <br>
With this training the *DAE* is then employed as a *spatial filter*: the corrupted pose predicted by the *LSTM3RL* network is first expanded into the latent space and then projected back into the input space by the decoder, which will recover an admissible spatial configuration.
<br>


#### No representation learning
An interesting point made by the authors of the paper is that there is no need to learn a condensed representation for the input in cases where it is already available in joint angle form. <br>
The reasoning behind this claim is fairly straightforward: 
> [...] unlike image data, mocap data in its original representation is smooth and continuous while there exists no guarantees of these properties in the learnt latent space.

As a consequence, their model directly operates in joint-angle space, without pre-processing the input poses.
<br>


#### A metric for naturalness
Their third contribution worth mentioning is a metric to asses the *naturalness* of the pose. As I said before, there is no well-defined quantitative measure of the humans-faithfulness of a motion sequence (at least for now). <br>
The authors circumnavigated the definition of a quantitative metric by training a separate classifier to predict the class to which the observed action belongs to. The idea is that the longer a sequence can be classified to belong to the same action category as the seed sequence the higher the quality of the prediction.<br>
Again they managed to find a way to teach a network to do what before could  *by hand*. 
<br>


# Similarities with previous works 
The method described in this paper has two main contact points with Martinez et al. : 
1. The use of a sampling-based loss: at inference time Ghosh et al. feed back into the recurrent cell at time-step $t+1$ the output of the cell at time-step $t$ to give the model the chance to learn from its mistakes.
2. The general-purpose network: following the steps of the before mentioned paper, the authors avoided training a task-specific model on each subtask by instead training a single, general-purpose model. <br>
<br>


### Modeling Human Motion with Quaternion-based Neural Networks
by Dario Pavllo, Christoph Feichtenhofer, Michael Auli, David Grangier<br>



This paper extensively explores the weaknesses of the available representations for three-dimensional poses and extends the domain making use of a new abstraction: *the quaternion*.


I will not discuss the architecture for this paper because I believe that the main source of innovation here is in the in-depth analysis of the representation problem.
<br>
<br>


# Quaternions?
Let me first clear out any source of confusion. Although the name evokes some sort of esoteric concept, quaternions are just 4-tuples. As [Wikipedia](https://en.wikipedia.org/wiki/Quaternion) puts it : 
>Quaternions are a number system that extends the complex numbers. [...]<br> 
Quaternions are generally represented in the form $a + bi + cj + dk$, where $a, b, c$, and $d$ are real numbers, and $i, j$, and $k$ are the fundamental quaternion units.


<br>

<div align="center">
<img src="https://media.giphy.com/media/zjQrmdlR9ZCM/giphy.gif" width="500"/>
</div>
<br> 


The question running through your head at this point might be something along the line: 
> What do quaternions have to do with human motion? 

To answer this question, let me first present to you the major shortcomings of the other available representations. Before quaternions were first introduced as a viable alternative two widely used parametrization schemes for rotations in the 3D Euclidean space were *Euler angles* and *Exponential maps*.

Euler angles represent orientations as successive rotations around the axes of a coordinate system. As you might guess, there are multiple ways to compose rotations in order to obtain the same final result. This is why applications that use Euler angles must agree on the particular order convention. A typical Euler rotation vector is a triplet that indicates the rotation around each axis in radians. There are two drawbacks in adopting a Euler angles representation: 
1. Each rotation is not uniquely identified, since $x$ and $x + 2\pi$ represent the same angle;
2. The representation space is discontinuous, since it jumps back to $0$ every time the rotation angle reaches $2\pi$.

The exponential map parametrization scheme, also known as *axis angle representation* was proposed as a more practical alternative to Euler angles. Intuitively, an axis-angle rotation is described by an axis $\hat{e}$ (a 3D unit vector) and a rotation angle $\theta$ around this axis. 
I am sorry to disappoint you, but exponential maps have the same weaknesses of Euler angles, namely: 
1. Singularities in the representation;
2. Discontinuity in the parameter space. 
3. There is also a third issue with the exponential map representation: <br>
Multiple rotations cannot be composed into one single rotation. This is maybe the most annoying disadvantage when it comes to apply such a parametrization in the field of human motion prediction, since composition is a fundamental operator for forward kinematics.


Now we have enough material to justify quaternions in the context of human motion modeling. Analogously to exponential maps, quaternions describe a 3D rotation with an angle $\theta$ and an axis $\hat{e}$. A rotation of $\theta$ radians around an axis $\hat{e}$ is encoded as a = $cos(\theta/2)$ and $bcd = \hat{e}\sin(\theta/2)$. This representation has multiple features that help it overcome the above mentioned limitations of the alternatives. I'll summarize its advantages in the following list: 
- No singularities: each rotation is uniquely identified with a quaternion representation.
- No discontinuities in the parameter space: this is particularly useful when regressing on rotations.
- Fully composable  : in the quaternions domain rotations can be composed and used to compute transformations.
- Interpolation: quaternions are also surprisingly handy to perform interpolation between rotations.
- Numerical stability: last but not least, quaternion based parametrization schemes are numerically stable, and are more computationally efficient than other representations.

So far so good. Unfortunately, quaternions have disadvantages too. The main issue when quaternions arises from the so-called *antipodal representations*: two opposite quaternions $q$ and $-q$ describe the same 3D rotation. Nevertheless, this dual parametrization represents improvement over the infinite set of equivalent representations in the Euler angles and exponential maps space.

This was it for quaternions, now let's proceed with the next paper. 
<br>
<br>



### Adversarial Geometry-Aware Human Motion Prediction
by Liang-Yan Gui, Yu-Xiong Wang, Xiaodan Liang, and Jos´e M. F. Moura<br>
 
Finally we discuss one of the most influential and successful papers on human motion prediction. The *Adversarial Geometry-Aware Encoder-Decoder*  model (abbreviated *AGED*) reached SOTA results at the time of publication. The approach proposed in this paper builds mainly upon the work of Martinez et al., enriching the solution with the surprisingly powerful adversarial framework. 

# The architecture 

![The architecture]({{site.baseurl}}/assets/images/AGED.png )
<div align="center"><i>Figure 2 from the paper.</i></div>
<br>

- An encoder-decoder sequence constitute the predictor. Trained to minime Euclidean btw predicted and true. The encoder learns a hidden representation from the input sequence. The hidden representation and a seed motion frame are then fed into the decoder to produce the output sequence. 
Other modifications such as attention mechanisms [3] and bi-directional encoders [42] could be also incorporated into this general architecture.
#### Note! Nobody tried with attention? 
- Residual connections 
- Ground truth one hot labels
- frame level Geodesic loss 
- Our entire model thus consists of a single predictor and two discriminators
-We integrate the geodesic (regression) loss and the two adversarial losses, and obtain the optimal predictor by jointly optimizing the following minimax objective function: P∗ = argmin
max λ Lf P Df ,Dc  adv (P, Df) + Lc adv (P, Dc) + Lgeo (P) 




# Take-away points

#### Human-like motion prediction
global sequence-level fidelity and continuity

#### geodesic loss
Idea: the distance between two 3D rotations.

Geometric structure aware loss function at the frame level, 
that is the shortest path between two rotations.
This geometrically more meaningful loss leads to more precise distance measurement and is computationally inexpensive. 

#### Adversarial training at the sequence level
This is partially because using a frame-wise regression loss solely cannot check the fidelity of the entire predicted sequence from a global perspective

We introduce two global discriminators to validate the prediction while casting our predictor as a generator.

Intuitively, the fidelity discriminator aims to examine whether the generated motion sequence is humanlike and plausible overall, and the continuity discriminator is responsible for checking whether the predicted motion sequence is coherent with the input sequence without a noticeable discontinuity between them.

The quality of the predictor P is then judged by evaluating how well bX fools Df
and how well the concatenated sequence {X, bX} fools Dc

### Similarities with previous works 

#### Martinez architecture for predictor

#### Residual connections for smoothness



<br>
<br>

### Learning Trajectory Dependencies for Human Motion Prediction
by Wei Mao, Miaomiao Liu, Mathieu Salzmann, Hongdong Li<br>


# The architecture 

![The architecture]({{site.baseurl}}/assets/images/Mao.png )
<div align="center"><i>Figure 2 from the paper.</i></div>
<br>
It consists of 12 residual blocks, each of which comprises 2 graph convolutional layers and two additional graph convolutional layers


- DCT temporal encoding.
given a temporal sequence X1:N, we first replicate the last pose, xN, T times to generate a temporal sequence of length N + T. We then compute the DCT coefficients of this sequence, and aim to predict those of the true future sequence X1:N+T. This naturally translates to estimating a residual vector in frequency space and was motivated by the zero-velocity baseline in [17]. we aim to learn the residuals
between the input and output DCT representations.
- Graph convolutional layer.
Using a different learnable A for every graph convolutional layer allows the network to adapt the connectivity for different operations.


# Take-away points

#### A feed-forward approach 
Here, instead, we propose to directly encode the temporal nature of human motion in our representation and work in trajectory space.

Let us denote by
x˜k =
(xk,1, xk,2, xk,3, · · · , xk,N) the trajectory for the kth joint across N frames.
we propose to adopt a trajectory representation based on the Discrete Cosine Transform (DCT). The main motivation behind this is that, by discarding the high frequencies, the DCT can provide a more compact representation, which nicely captures the smoothness of human motion, particularly in terms of 3D coordinates.

To make use of the DCT representation, instead of treating motion prediction as the problem of learning a mapping from X1:N to XN+1:N+T, we reformulate it as one of learning a mapping between observed and future DCT coefficients

#### Learned graph representation of the human body 
spatial dependency of human pose is encoded by treating a human pose as a generic graph (rather than a human skeletal kinematic tree) formed by links between every pair of body joints. Instead of using a pre-defined graph structure, we design a new graph convolutional network to learn graph connectivity automatically. This allows the network to capture long range dependencies beyond that of human kinematic tree.




### Similarities with previous works 



<br>
<br>

### Structured Prediction Helps 3D Human Motion Modelling
by Emre Aksan,Manuel Kaufmann, Otmar Hilliges<br>


<br>
The proposed layer is agnostic to the underlying network and can be used with existing architectures for motion modelling


# Take-away points

#### AMASS 
A new dataset.
The dataset contains 8593 sequences, which comprise a total of 9084918 frames sampled at 60 Hz. This is roughly equivalent to 42 hours of recording, making AMASS about 14 times bigger than H3.6M (632894 frames at 50 Hz).

#### SPL 
A structure prediction layer.
successfully exploiting spatial priors in human motion modelling.

xt ∈ RN is a concatenation of K joints x(k)
xt = [x(hip) t
t ∈ RM: , x(spine)
t . . . x(lwrist) t , x(lhand) t ]

the SP-layer takes
a context representation ht as input. Here, ht is assumed to summarize the motion sequence until time t. Without loss of generality, we assume this to be a hidden RNN state or its projection. 
While existing work typically leverages several dense layers to predict the N-dimensional pose vector xt from ht, our SP-layer predicts each joint individually with separate smaller networks:

pθ(xt) =K k=1
pθ(x(k) t | parent(x(k) t ), ht)

pθ(xt) = pθ(x(hip) t | ht)pθ(x(spine) t | x(hip) t , ht) · · ·

In this formulation each joint receives information about its own configuration and that of the immediate parent both explicitly, through the conditioning on the parent joint’s prediction, and implicitly via the context ht

First, the proposed factorization allows for integration of a structural prior in the form of a hierarchical architecture where each joint is modelled by a different network. This allows the model to learn dedicated representations per joint and hence saves model capacity.

Because the per-joint decomposition leads to many small separate networks, we can think of an SP-layer as a dense layer where some connections have been set to zero explicitly by leveraging domain knowledge

<div align="center">
<img src="{{site.baseurl}}/assets/images/Aksan.png" width="500"/>
</div>
<div align="center"><i>Figure 2 from the paper.</i></div>

#### Per joint loss

We additionally propose to perform a similar decomposition in the objective function that leads to further improvements


<br>


### Similarities with previous works 
<br>
<br>

# Further reading 
Structural-rnn: Deep learning on spatiotemporal graphs. 















