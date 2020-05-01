---
layout: post
title:  "Human motion prediction"
date:   2020-05-01 10:51:43 +0100
categories: topic review
---
Hello everyone, welcome to my first topic review!
First things first: why am I doing a topic review on *Human motion prediction*? The reason is that I am starting a new project on the topic and I'm anyway going to read a few papers on the subject, and -as I've recently discovered during one of my experiments- sharing your work usually has the effect of pushing your quality bar higher. <br>
Now let me cut the premises here and dive into Human Motion Prediction. 

A quick note before continuing: this is not intended to be a complete tour on the history of the topic, but instead a look at some of the latest and most succesful approaches. 
<br> 
<br>

## On human motion prediction using recurrent neural networks
by Julieta Martinez, Michael J. Black, and Javier Romero. <br>

---- 

I enjoyed reading this paper: the authors proposed many (though not too many) interesting ideas, that are clearly justified by a critical analysis of those that were State-of-the-art models in 2017. Too easily the scientific discourse can spiral down a dead-end, dragged by its own weight. Papers that can stir the discourse attention into a new direction, like this one, are necessary to its very progress.

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


And the experiments' results show that this simplicity-biased mindset has repaid their trust, obtaining new state-of-the-art results. 

This simplicity-first approach also brought them to take into consideration an easy-to-implement but until then ignored baseline, which is still today hard to beat: the **zero velocity** baseline. Behind this fancy name is simply the concept of predicting zero movement, aka predicting stillness. To the surprise of the authors this baseline beated all state-of-the-art model back then: 
>We have found a very simple baseline that outperforms sophisticated state-of-the-art methods based on deep RNNs for short-term motion prediction.


#### Sampling-based loss 

One of the problems that this model aims at solving is the typical incapacity of RNN models to recover from their own mistakes, which leads to a substantial discrepancy between training and testing performances, and hence to low robustness. <br>
To address the issue they feed to the decoder its own samples, instead of the ground truth, at each training time-step.


#### Residual architecture 

This idea is the one that has brought the highest improvements on the results of their experiments, and it tackles the huge discontinuity between the conditioning sequence and prediction (aka between the first predicted frame and the last observed input). 

The idea is simple and effective: instead of modelling all possible conditioning poses so that the first frame prediction is continuous, directly model the velocity of the model. <br>
And to model velocity they propose a residual architecture, obtained by simply adding a residual connection between the input and the output of each RNN cell. 






