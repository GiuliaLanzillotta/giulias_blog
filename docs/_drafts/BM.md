---
layout: post
title: "Boltzmann machines"
subtitle: "The beauty and complexity of the simple"
date: 2021-03-22 10:00 +0100
categories: [AI, technical]
published: false 
---


<div align="center">


<div style='position:relative; padding-bottom:calc(56.25% + 44px)'><iframe src='https://gfycat.com/ifr/SlightContentGermanshorthairedpointer' frameborder='0' scrolling='no' width='100%' height='100%' style='position:absolute;top:0;left:0;' allowfullscreen></iframe></div>
<br>

<div align="justify">



<h3>References</h3>
[1] <a href="https://arxiv.org/abs/2007.12815">From Boltzmann Machines to Neural Networks and Back Again</a> <br>
[2] Lecture notes from the <a href="http://www.da.inf.ethz.ch/teaching/2020/DeepLearning/"> Deep Learning course</a> held at ETH <br>
[3] <a href="https://hashingit.com/elements/research-resources/1948-intelligent-machinery.pdf">Intelligent machinery, Turing, 1948</a> <br>
[4] <a href="https://www.pnas.org/content/79/8/2554">Neural networks and physical systems with emergent collective computational abilities, Hopfield, 1982</a>

</div>

<!-->
Boltzmann machine course notes
-----
1) models binary data 
In a causal model 2 steps to generate data:
- pick hidden states from prior
- pick visible states from the conditional
ALTERNATIVE generative models: energy based models
- everything is defined in terms of the energies of JOINT CONFIGURATIONS of the visible and hidden units p(v,h) = exp(-E(h,v))/Z 
-E(v,h) = sum individual states + interactions
Sampling from a model: Markov Chain monte carlo -> random init + picking units at random and allowing them to stochastically update their states based on energy gaps 
If we cannot compute the normalisation term we can use sampling to approximate the probabilities (keep sampling until stationary distribution is achieved)
Same for posterior probabilities (keep visible units clamped to the data vector)
Surprising fact: all the units are connected but local learning rule is effective
Learning rule (using gradient dp(v)/dw) = <s_i*s_j>v - <s_i*s_j>


Notes on self-organising maps
https://medium.com/@abhinavr8/self-organizing-maps-ff5853a118d4 - medium 
https://en.wikipedia.org/wiki/Self-organizing_map - wikipedia
Self-organizing maps differ from other artificial neural networks as they apply competitive learning as opposed to error-correction learning (such as backpropagation with gradient descent), and in the sense that they use a neighborhood function to preserve the topological properties of the input space.
<!--->

