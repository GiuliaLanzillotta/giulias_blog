---
layout: post
tags: "AI-topics"
title: "Why I chose continual learning?"
author: "Giulia"
date: 2025-12-27 10:00 +0100
last_updated: 2025-05-08
categories: [statements]
published: false 
excerpt_separator: <!--more-->
sticky: false
hidden: false
---

Coming soon...
<!--more-->
<br>
<br>

I did my phd on the topic of continual learning. The choice to focus on continual learning several years ago was less obvious than it is today. When I started, my research ambitions were no less and no more than understanding the foundations of intelligence. Real, human-like intelligence is what interested me. 

Although I did not resolve right away to focus on continual learning, 
Continual learning has always been at the heart of our conception of intelligence ... as proven by many pop movies

... 

I want to take the reader through a brief historical tour of our conceptualization of AI, to show that "continual learning" has always been in our imaginary of AI since the beginning. 

Show some video clips of movies from different ages, where AI is an autonomous system, and autonomy has always come with the ability to adapt and change in time. 

Academia has the privilege to dream big and shoot far in the future. I wanna build an AI that has 100 years of autonomy (symbolic), like a human. So no centralized updates needed for at least 100 years. 

Why is that a challenge today and what are the real limitations? 


Talk about the history of the field and how the solutions which we have now are a byproduct of incremental progress made in the last .. years. Our applications are more and more pushing the boundaries of this static framework and we'll eventually need to let go of it and explore a fully real-time learning system. 

Once we start thinking in terms of real-time systems, there are several questions which pop up and are scientifically challenging and philosophically interesting. For instance: 
- real-time optimization without interference
- defining an algebra of information in neural networks: data to network state function 
- long-horizons optimization without plasticity
- response times and latency in learning: offline and online actions (do we need a sleep phase?)
- what should be memorized and what should be forgotten : how does our brain choose?
 


### Preface & disclaimers

I'm writing this post with an imaginary fellow AI researcher in mind. That said, all curious minds are welcome to explore—I won’t go too deep into the technical details, and I hope the content will be accessible to anyone with an interest in the topic.

I am a 3rd year phd student, and I spent the first year and a half of my phd exploring many cool topics in the space of deep learning and its intersection with biological intelligence, a field called today neuro-ai. Although my research has since shifted away from neuro-AI, the motivation that initially drew me to it continues to guide my work. At its core, my research is driven by a fascination with intelligence—arguably one of the greatest mysteries facing humanity —and a strong belief that truly intelligent systems have the potential to unlock a new era of prosperity for this earth, one that has so far only inhabited the world of dreams. 

 <!--among which *associative memories* and *self-organizing systems*, backpropagation-free network learning algorithms such as *predictive coding*, intrinsic motivation systems and curiosity in artificial agents through the framework of *active inference*. Eventually, I decided to focus my phd thesis on the problem of *knowledge adaptation and retention in neural networks*, which goes by the name of the "plasticity-stability dilemma" in the biological learning community, or by "continual learning" in the deep learning community. <!--I still like to think however, that one day my path will bring me back to my early topics, and for now I keep following that research from a distance.-->

<br>

### What do I work on? 
#### Many names and many faces.
Today I would say I work on continual learning, but the expression "continual learning" has been overloaded with meaning through the years, and is far too confusing today to be used. Some of you will think of the famous algorithms like EWC, some will confuse it with "meta", "online" or "active" learning, some will think of reinforcement learning and some will just not know what to think (I have experienced all of the listed reactions). So let me introduce the problem to you as I see it.  

As I said above, in general the problem is that of knowledge adaptation or **plasticity** and retention or **stability** --which I prefer to call **memory**-- in neural network models. Plasticity and memory are two facets of the problem of learning, and a narrow focus on one eventually limits the system's intelligence. To some extent, the predominant view of learning in the machine learning community equates learning with plasticity. 
One reason for this is that historically research focused on tasks confined to the realm of a single *stationary* distribution -in simper terms, *environments where things don't change*. In such settings, adapting once at the start is enough to survive. This led to the development of the classical paradigm for artificial learning systems, where learning occurs once on a large data set and then never again. This paradigm is still in use today, despite the increasing difficulty and dynamism of the new tasks assigned to artificial agents.

There is extensive empirical evidence—dating back to the previous century [1,2] and continuing to build today [3,4]—that neural networks struggle to retain knowledge while acquiring new information.  To build some practical intuition, you can think of the case of a language model deployed to assist with coding which has to adapt to a new programming language. When tasked to learn a new language, the system capabilities on other programming languages may be damaged. This failure mode is dramatically referred to as catastrophic forgetting. More generally, we can say that artificial intelligence systems today *lack long term memory*. 

As it turns out from research done in ~ the last 4 years, neural networks do not excel at plasticity either in the long term (CIT). When evaluating solely the ability of the model to adapt to new tasks over and over again, the systems' performance appears to degrade over time. This failure mode of neural networks is known in the literature as *loss of plasticity*. 

For these reasons, continual learning is the real problem of learning—as it fully embraces both plasticity and stability. In fact, continual learning deserves to be called simply “learning,” while what we currently refer to as learning might more accurately be called “once-only learning,” “stationary learning,” or even “static learning.” You get the idea. 


<!--More generally, at the core of the problem is the breaking of an assumption which lies the foundations of all learning algorithms in use today: the assumption that all data is randomly drawn from the same distribution -popularised as *i.i.d. assumption*. Neural network learning algorithms based on this assumption have been remarkably effective on what is called "out of distribution (/domain) generalization", which is a type of performance testing using data outside of the "training distribution". Additionally, the knowledge learned by a neural network can be "transferred" to a different domain with high statistical efficiency, what is known as "few shot learning". However, another testing ground has been largely overlooked throughout the development of network learning algorithms. The networks cannot "remember out of distribution", meaning that they fail to retain knowledge under distribution shifts in the training data. *Historically, research in deep learning has targeted a specific train+test paradigm*, where the model is trained only once (if possible, on a lot of data), and it is deployed with minimal or no finetuning. This paradigm complies with the i.i.d. assumption and naturally places the focus on generalisation and transfer, ignoring knowledge retention.  However, when training on out of distribution data the i.i.d. assumption does no longer hold. --> 


<!--<div align="center" >
<img src="{{site.baseurl}}/assets/images/CLVIEW2.png" width="400"/>
<figcaption>Sketch depiction of the current landscape of learning algorithms for neural networks. Historically, the literature has focused on in- and out- of distribution generalisation and transfer, ignoring knowledge retention.</figcaption>
</div>-->

<!--The way I see it, continual learning hits a fundamental limitation of neural network learning algorithms, and its solution demands foundational research. Like all other such fundamental problems, it allows multiple concurrently valid perspectives, branching out in several subfields of machine learning, such as optimization, statistical temporal models, feature learning, online learning and compression. Further below in this post, I describe some of the questions which I have been thinking about and the corresponding perspectives.-->

<!-- A superficial understanding is that the network overwrites its previous knowledge by modifying network weights which are "important" to the computation of previous tasks. Such intuition is behind many existing continual learning algorithms such as EWC, SI, ... This *perspective* naturally introduces the questions: which weights are important for a task and how much knowledge can be acquired without modifying weights? The latter question  brings us to the literature on "in-context learning". 

As I see it, continual learning is a fundamental problem in neural network learning, and like all other fundamental problems, it affords multiple concurrently valid perspectives, branching out in various existing literatures.  Each perspective comes with its unique conceptual tools and pre-existing knowledge, which allow to address certain questions and not others. It is not clear a-priori which treatment of the problem may help make the most progress. For example, the weight space perspecitve mentioned above leads to questions regarding the geometric properties of neural networks' loss landscape, which are of more general interest to the deep learning community. Other useful points of view on the problem are: feature learning -how do the characteristics of the feature dynamics affect knowledge retention-, optimization -what is the interplay between implicit optimization biases and knowledge in the network-, probabilistic models of the environment and robustness -which distribution shifts are more harmful and how can knowledge of the environment help in knowledge retention. Further down in this post, I describe some of the questions which I have been thinking about and the corresponding perspectives. -->


I haven’t yet mentioned a crucial aspect of the continual learning problem: finite resources. If we had unlimited memory and compute, achieving both plasticity and memory would be relatively straightforward. For instance, with infinite storage, a memory system could archive the agent’s entire experience history and retrieve it as needed. Experience replay, a widely used method, is a scaled-down version of this idea—it stores a random subset of past experiences to approximate this ideal.
Another straightforward but resource-intensive approach would be to train a new model for every skill or whenever the environment changes. Similarly, methods that add new parameters to the model with each experience follow this principle, but they come at the cost of ever-increasing computational demands. These strategies highlight the core challenge: designing adaptive learning algorithms that operate efficiently within fixed resource constraints.
As I noted earlier, the tasks we expect AI systems to tackle today are increasingly dynamic. In response, both tech companies and academic labs are racing to design highly engineered systems that can process new inputs without truly adapting. That’s because we still don’t have genuinely adaptive learning algorithms that operate within fixed resource constraints. Adapting under finite resources—that’s the real challenge.

<!--There are many "hacky" ways to retain knowledge, and all the methods used in practice today belong to this class. For example, one can retrain the model on the entire history, inlcuding the new data [cit] or portions of it [cit]. Or equivalently, one can add new "parts" to the network and train only the new part on the new data [5, cit].[a footnote for in-context learning] Although they work in practice, these solutions have a computational or storage cost which grows at the same rate as the rate of production of new data. As of today, computation is not a bottleneck for the big tech competitors, and these solutions are ugly but affordable. However, a linear computational complexity is not feasible in the long run. Here we might disagree, depending on your vision regarding the future of artificial intelligence. In the next section I will try to convince you that resource-constrained knowledge adaptation and retention will not only be profitable but necessary at some point in the future. -->


<br>

<!--### Why do I work on it? 
#### Let's start from the top. AGI. 

Today we hear a lot of "artificial general intelligence" or AGI. Only a couple of years ago, when I started my phd, AGI was still an undignified topic in the research community. I was one of the few in my lab actively interested in AGI back then. Despite the fact that the topic has quickly risen to the spotlight, its treatment is still not properly scientific. What does AGI mean? The answer varies from person to person, and it is more or less elaborate, and more or less detailed. -->

### Investors pitch for continual learning
<!--When I imagine the future of AI, whether it is a car driving system, a robotic assistant, a personalised health and nutrition coach, I envision an intelligence that *adapts quickly* to any change in the environment in order to be serviceable for long periods of time. The lower the adaptation cost, the longer the AI can be used. The best continual learning algorithms today yield adaptation costs growing linearly with the number of total bits processed *O(N)* [citations/footnote]. This means that given a finite maximum computational budget *B* per update, the model will be able to process *O(B)* bits in total. Consider instead a more efficient learning algorithm with adaptation cost growing sub-linearly in the number of bits, e.g. *O(ln N)*. A model with such algorithm can process *O(exp B)* bits in total. This exponential gain can make a great difference in practice. -->

As I mentioned above, a partial interpretation of the concept of learning can limit the system's intelligence in fundamental ways. By that I mean that the system will be able to solve some problems, but not others. Arguably, the old stationary learning formula, which overlooks memory, has been succesful in a wide arena of problems. So the question is: *what is the limit of this approach?*

As noted earlier, current state-of-the-art adaptation strategies that incorporate memory have computational or storage costs that scale with the number of new experiences. This means that the set of solvable problems is bounded by how quickly information accumulates compared to the growth of available compute or storage. Today, most AI competition happens within this constrained arena—where systems are designed to handle growth in a relatively contained way.There are two ways to expand the set of problems a system can handle:

1. Increase available compute. Many companies are pursuing this route—competing for existing resources or investing in the development of new computing infrastructure. [Insert link to relevant article]()
2. Lower the cost of adaptation and memory. By making it cheaper to remember and learn from new information, a system can process more experiences within the same computational budget—unlocking access to more complex problems. A striking example of the power of this path was the release of the DeepSeek model, which triggered significant market and political reactions globally. [Insert link to article]()
This latter path —*i.e., continual learning*— calls for a foundational rethink of what learning is. As noted above, it requires an algorithmic design that integrates both plasticity and memory in equal measure. Developing such systems is a difficult and slow process, often reliant on a healthy bit of randomness.

It’s understandable, then, why many large tech companies focus on competing in the space of immediately solvable problems. The pressure to deliver market-ready solutions—and to adopt AI systems rapidly is high. Yet, whoever succeeds first in building a true continual learning system will gain a significant strategic advantage. *So, my dear investor: the risk is high, but the potential returns are astronomical.*  

A final note on AI and sustainability: continual learning isn’t just good for business—it also offers a more environmentally sustainable approach to AI. By optimizing memory and reducing wasteful retraining, we can build smarter systems that are lighter on the planet.



<!--The predominant approach to AGI today seems to be that of *general* foundation models which are subsequently *specialized* to a specific problem. Those who bet on this solution, disregarding the continual learning problem, must believe one of two things: (1) that the amount of things to learn is finite and can be exhausted or (2) that the more general the (pre-)training distribution is, the lower the adaptation cost to new information. 
Regarding the first point, consider the simple example of an AI assistant reading and explaining scientific papers. Despite its unpredictability, progress in science is steady and scientific knowledge grows regularly. Since each new discovery builds on previous ones, the model has to process and incorporate every new paper. In other words, the amount of things to learn will continue growing as long as science is unfinished (and that is probably much longer than the lifespan of any of us).
Regarding the second point, there is no evidence at the moment of foundation models being better at knowledge retention. On the contrary, foundation models seem to suffer from catastrophic forgetting similarly to smaller or specialized models. [citation] However, as far as I know, a rigorous study of the effect of general pretraining on knowledge retention has not been conducted yet. 

With this premise, it is clear that most of the competition in AI products today happens in a space of problems well within the bounds of the maximal computational budget *B*. The lower budget problems are obviously the first in line to be solved. Predictably, in a few (or many) years the competition field will have expanded to those problems which go beyond the maximal computation budget, calling for more resource-efficient adaptation. 
In other words, a solution to continual learning is a crucial component on the path to AGI, allowing the agent to last longer in the environment within the same maximal computation.  
-->

<br>

### Let's talk research 

hand-wavy arguments, but 

In general, I hope I have convinced you that...
Continual learning should be ragarded as another goal of learning such as generalization or transfer.


Finally, I thought I'm sharing here a approximate overview of my thoughts, these include thoughts developed in past and current project and thoughts which are waiting to be developed in future projects. If you would like to discuss any of these or are inspired by them in your research, feel free to contact me - I leave my contacting details in the next paragraph.

Share slides here

1. **What is a distribution shift?** 

    Historically, continual learning has been associated with the idea of a "*task*", which would itself be conceptualised as a semantically closed set of items, such as related concepts or behaviours. When expanding this restricted setting to arbitrary streams of data the notion of *relatedness* or similarity becomes harder to fathom. Theoretical studies of catastrophic forgetting have made attempts at including some version of task relatedness in their models. Invariably, the definition of similarity ends up depending on the specific theoretical setup. In other words, there is no unified understanding of the effect of similarity on the learning dynamics, because there is not unified definition of similarity.  As an example, in a popular (toy) benchmark known as "Rotated MNIST", the input images are rotated by a fixed *r in [0,90]* degrees rotation to create a distribution shift. The amount of rotation *r* in this case is a natural measure of the amount of distribution shift. However, this measure is useless outside of this constructed setting. Think, for a counter-example, of the training of large language models. Given the exceptionally large number of samples processed by the model (often in the order of the whole internet) it is easy to argue that *there are distribution shifts in the input distribution*. However, *how intense* and *where* these distribution shifts may be are much harder questions. Until we have a working definition of distribution shift we cannot begin to understand its effects on learning.

2. **Sequential multitask learning**  

    One of the many valid ways to conceptualise continual learning is as "sequential multitask learning". Multitask learning is the well-defined problem of learning mutliple tasks -again, what is a task?- with a single model. In the classical continual learning setting too, the learner is presented with multiple tasks, and it is expected to learn to perform well *all the tasks*. The characterising difference between the two settings is that in continual learning the tasks are only available sequentially. In other words, continual learning can be thought of as a multitask learning problem where in each moment only a subset of the data is available -typically the data of one task. Following this description, we can begin to formalise the problem as a function approximation problem, where the function to be approximated is the multitask objective.
    one to approach the problem as the problem of approximating the "*multitask objective L(D1, ..., D10)*" using some limited memory of the past *M(D1, ..., D9)* and the available data *D10*. Within this perspective existing continual learning algorithms can be understood as taking different approximations, ... which provides valuable lenses to understand the workings of the algorithms. In past work I have taken this perspective to study the limitation of one assumption ... 

3. **Learning what is useful, knowing what is useful**

    Connected to the previous perspective, but critical. is the multitask objective always good? we studied from the view of online performance that ... the question of knowledge retention becomes a two parts question: which knowledge to retain and how? 

4. **Optimization in non i.i.d. settings** 

5. **Feature learning** 

#### get in touch

There is a lot of research still to do in this field. I personally have many ideas which I will share at a high level as interests here. If you find any of this interesting contact me by email. I often am too busy with a million projects, but I am always happy to at least advise...

I am happy to chat with anyone

<br>
<br>

--- 

Want to cite this blog?

### References

[1] Michael McCloskey and Neal J. Cohen. Catastrophic interference in connectionist
networks: The sequential learning problem. volume 24 of Psychology of Learning
and Motivation, pages 109–165. Academic Press, 1989.

[2] Robert M. French. Catastrophic forgetting in connectionist networks. Trends in
Cognitive Sciences, 3(4):128–135, 1999.

[3] Yun Luo, Zhen Yang, Fandong Meng, Yafu Li, Jie Zhou, and Yue Zhang. An
empirical study of catastrophic forgetting in large language models during continual
fine-tuning. arXiv preprint arXiv:2308.08747, 2023.

[4] Tongtong Wu, Linhao Luo, Yuan-Fang Li, Shirui Pan, Thuy-Trang Vu, and Gholam-
reza Haffari. Continual learning for large language models: A survey. arXiv preprint
arXiv:2402.01364, 2024.

[5] Hu, Edward J., et al. "Lora: Low-rank adaptation of large language models." arXiv preprint arXiv:2106.09685 (2021). https://arxiv.org/pdf/2106.09685