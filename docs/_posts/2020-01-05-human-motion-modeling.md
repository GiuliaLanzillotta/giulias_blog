---
layout: post
title:  "Human motion prediction"
date:   2020-05-01 10:51:43 +0100
categories: topic review
---
Hello everyone, welcome to my first topic review!<br>
**First things first**: *why should we be insteresed in Human motion prediction*? 
To answer this question let me borrow the words of the authors of the first paper I'm going to propose to your attention : 
> An important component of our capacity to interact with
the world resides in the ability to predict its evolution over time. Handing an object to another person, playing sports, or simply walking in a crowded street would be extremely challenging without our understanding of how people move, and our ability to predict what they are likely to do in the following instants. Similarly, machines that are able to perceive and interact with moving people, either in physical or virtual environments, must have a notion of how people move.<br>

*Why am writing this topic review?* The reason is that I am starting a new project on the topic and I'm anyway going to read a few papers on the subject, and -as I've recently discovered during one of my experiments- sharing your work usually has the effect of pushing your quality bar higher. <br>


Now let me cut the premises here and dive into Human Motion Prediction. 

A quick note before continuing: this is not intended to be a complete tour on the history of the topic, but instead a look at some of the latest and most succesful approaches. 
<br> 
<br>

# HuMoP 

### Two different tasks
- short term prediction 
- long term prediction
the literature often distinguishes between short-term and long-term prediction tasks. Short-term tasks are often referred to as prediction tasks and can be assessed quantitatively by comparing the model prediction to a reference recording through a distance metric. Long-term tasks are often referred to as generation tasks and are harder to assess quantitatively. For these cases, the prediction quality can be evaluated by human evaluation studies.

### Inherent constraints 

- Skeletal configurations 
- Temporal coherence

### Loss quantification 

- qualitative 
- quantitative 

### The dataset 
**Human3.6M**

Each pose at time t consists of joint angles Xt = [x1, x2, ...xn]


## On human motion prediction using recurrent neural networks
by Julieta Martinez, Michael J. Black, and Javier Romero. <br>

---- 

I enjoyed reading this paper: the authors proposed many (though not too many) interesting ideas, that are clearly justified by a critical analysis of those that were state-of-the-art models in 2017. Too easily the scientific discourse can spiral down a dead-end, dragged by its own weight. Papers that can stir the discourse attention into a new direction, like this one, are necessary to its very progress.

### The architecture 

![The architecture]({{site.baseurl}}/assets/images/Martinez1.png )
<div align="center"><i>Figure 2 from the paper.</i></div>
<br>
The architecture is a rather classic **seq2seq** model with *GRU* cells, with the addition of *residual connections* in the decoder and, interestingly, *parameter sharing* between encoder and decoder.
> In seq2seq, two networks are trained;<br>(a) an encoder that receives the inputs and generates an internal representation, and <br>(b), a decoder network, that takes the internal state and produces a maximum likelihood estimate for prediction


### Take-away points

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


#### Residual architecture 

This idea is the one that has brought the highest improvements on the results of their experiments, and it tackles the huge discontinuity between the conditioning sequence and prediction (aka between the first predicted frame and the last observed input). 

The idea is simple and effective: instead of modelling all possible conditioning poses so that the first frame prediction is continuous, directly model the velocity of the model. <br>
And to model velocity they propose a residual architecture, obtained by simply adding a residual connection between the input and the output of each RNN cell. 
<br> 
<br>


## Learning Human Motion Models for Long-term Predictions
by Partha Ghosh, Jie Song, Emre Aksan, Otmar Hilliges<br>

---- 



### The architecture 

![The architecture]({{site.baseurl}}/assets/images/Ghosh.png )
<div align="center"><i>Figure 1 from the paper.</i></div>
<br>

Two components trained separately to de-correlate the erros and finally fine-tuned in an end to end prediction

- DAE
- LSTM3LR


### Take-away points

#### Integrating domain knowledge 
sone with the dropout in DAE 
The autoencoder is used as a spatial filter 
on data driven recovery of the spatial configuration unlike previous attempts

#### Filterning the noise out 

The second use of denoising autoencoder 
Only operates in the spatial domain!

#### No representation learning

Hence, in cases where the input is already available in joint angle form, we argue that an additional representation learning step is not necessary. In consequence, our method employs a spatio-temporal component that directly operates in joint-angle space

unlike image data, mocap data in its original representation is smooth and continuous while there exists no guarantees of these properties in the learnt latent space.

#### A metric for "naturalness"

To assess naturalness we propose to train a separate classifier to predict action class labels. Intuitively the longer a sequence can be classified to belong to the same action category as the seed sequence the higher the quality of the prediction.


### Similarities with previous works 

#### Sampling based loss 
Output of previous time step is input to next one.

#### A single model 
Contrary to previous work [17, 19] we train a single, unified model to perform all actions and do not require task specific networks
<br>
<br> 


## Modeling Human Motion with Quaternion-based Neural Networks
by Dario Pavllo, Christoph Feichtenhofer, Michael Auli, David Grangier<br>

---- 

### The architecture 

![The architecture]({{site.baseurl}}/assets/images/Pavllo.png )
<div align="center"><i>Figure 1 from the paper.</i></div>
<br>
we use an RNN to model sequences of threedimensional poses. 
The network takes as input the rotations of all joints (encoded as unit quaternions
If employed for the latter purpose, the model includes additional inputs (referred to as “Translations” and “Controls”

### Take-away points

#### Quaternions! Wait, what? 

the central innovation is here in the representation of the data. 


[Quaternions](https://en.wikipedia.org/wiki/Quaternion) are a 4D extension of complex numbers that form the S3 group, and can be described as realvalued 4-tuples wxyz such that q = w + xi + yj + zk,

Pros : 
- No singularities: 
- No discontinuities: 
- They can be composed and used to compute transformations: Unlike Euler angles, which employ trigonometric functions to compute transformations, quaternion transformations are based on linear operators 
- They present a simple and elegant way to perform interpolation between rotations
- more numerically stable, and are more computationally efficient than other representations

Cons: 
- A disadvantage of quaternions is that they encode
half-angle rotations, giving rise to the so-called antipodal representations: two possible representations for the same 3D orientation, q and −q.


Euler representation: 
A typical Euler rotation vector is a triplet that indicates the rotation around each axis in radians
2 drawbacks: 
- infinite number of representations for the same rotation
- representation space to be discontinuous

Axis angle representation: 
exponential map, this representation again uses 3 parameters and is proposed as a more practical alternative to Euler angles. Intuitively, an axis-angle rotation is described by an axis e and a rotation angle θ around this axis.
2 disadvantages: 
- Discontinuity
- No way to compose rotations: Composition is a fundamental operator for forward kinematics


#### Convolutional networks
Compared to RNNs, convolutional networks have a
number of advantages. First, they are more efficient on modern hardware, second less likely to suffer under exploding or vanishing gradients such as RNNs. 
in practice they tend to focus on local dependencies rather than long-term relationships. In convolutional models, the receptive field can be drastically increased through dilated convolutions. 

Architecture is an adaptation of its RNN-based counterpart, in which we replace the backbone (GRU and linear layers, yellow block in Figure 1) with a sequence of convolutional layers.

+an exponentially increasing dilation factor




### Similarities with previous works 

#### Autoregressive model 
at each time step, the model takes as input the previous recurrent state as well as features describing the previous pose in order to predict the next pose.

#### GRUs
we selected GRU for their simplicity and efficiency. In line with the findings of Chung et al. (2014), we found no benefit in using long short-term memory (LSTM), which require learning extra gates.

#### Residual connections 
We take inspiration from residual connections applied to Euler angles (Martinez et al., 2017), where the model does not predict absolute angles but angle deltas and integrates them over time. For quaternions, the predicted deltas are applied to the input quaternions through quaternion product (Shoemake, 1985) (QMul block in Figure 1). Similar to Martinez et al. (2017), we found this approach to be beneficial for short-term prediction, but we also discovered that it leads to instability for long-term generation.

<br>
<br>


## Adversarial Geometry-Aware Human Motion Prediction
by Liang-Yan Gui, Yu-Xiong Wang, Xiaodan Liang, and Jos´e M. F. Moura<br>

---- 

### The architecture 

![The architecture]({{site.baseurl}}/assets/images/AGED.png )
<div align="center"><i>Figure 2 from the paper.</i></div>
<br>

### Take-away points

#### Human-like motion prediction
global sequence-level fidelity and continuity

#### geodesic loss
Geometric structure aware loss function at the frame level, 
that is the shortest path between two rotations.
This geometrically more meaningful loss leads to more precise distance measurement and is computationally inexpensive

#### Adversarial training at the sequence level
We introduce two global discriminators to validate the prediction while casting our predictor as a generator

Intuitively, the fidelity discriminator aims to examine whether the generated motion sequence is humanlike and plausible overall, and the continuity discriminator is responsible for checking whether the predicted motion sequence is coherent with the input sequence without a noticeable discontinuity between them.

### Similarities with previous works 



<br>
<br>
-----


## Title
by authors<br>

---- 

### The architecture 

![The architecture]({{site.baseurl}}/assets/images/Ghosh.png )
<div align="center"><i>Figure 1 from the paper.</i></div>
<br>

### Take-away points

### Similarities with previous works 









