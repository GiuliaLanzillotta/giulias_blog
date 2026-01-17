---
layout: post
tags: "AI-topics"
title: "Why I chose Continual Learning?"
author: "Giulia"
date: 2025-12-27 10:00 +0100
last_updated: 2025-05-08
categories: [statements]
published: true 
excerpt_separator: <!--more-->
sticky: false
hidden: false
---

I started writing this post exactly a year ago, on the flight back from NeurIPS. At the conference, I found myself in a perpetual cycle of presenting and defending the necessity of continual learning, articulating its nuances so frequently that my responses crystallized into a practiced defense. I intended to commit those thoughts to paper immediately, but as is often the case, the friction of life and a persistent academic perfectionism delayed the process.
In the time since, those memorized arguments have dissolved into something else, and this is a new, final version of that post, written by a slightly older version of myself. 
<!--more-->

<br>

{: .drop-cap }
**My PhD is about continual learning, but I got there via a somewhat un-scenic route.** The choice to study this in 2022 was much less obvious than it is today, in 2025. *I started with the modestly sized goal of understanding the foundations of intelligence itself*. This involved a lot of time spent trying to pin down what we were actually talking about, in company of [Legg & Hutter (2007)](https://arxiv.org/pdf/0712.3329), [Chollet (2019)](https://arxiv.org/pdf/1911.01547) and [Gallese and Lakoff (2007)](https://www.tandfonline.com/doi/abs/10.1080/02643290442000310). While everyone else was busy with DALL-E and the magic of Transformers, I was reading about *Active Inference* ([Friston 2005](https://pubmed.ncbi.nlm.nih.gov/15937014/)), *Associative Memories* ([Hopfield 1982](https://www.pnas.org/doi/10.1073/pnas.79.8.2554)), *Adaptive Resonance Theory* ([Grossberg 2012](https://pubmed.ncbi.nlm.nih.gov/23149242/)) and *Cell Assemblies* ([Buzsàki 2010](https://pmc.ncbi.nlm.nih.gov/articles/PMC3005627/)). At the time talking about AGI back still made people slightly uncomfortable in polite academic circles. I suppose one has to be exceptionally stubborn to ignore a mainstream that was suddenly everywhere in the news, I'll admit that much. 
<!-- At the same time, I was studying Evolutionary theory (), Neuroscience (), and Dynamical systems theory ().  -->

Eventually, I hit on an intuition that felt both simple and revelatory: *memory and learning are just two sides of the same coin*. If we wanted algorithms that could actually match human performance, we had to ask how memory is actually implemented. As I came to learn, this question can be the basis for an entire career in neuroscience or cognitive science, and indeed, many have spent their lives on it. Yet, in computer science, we still tend to treat learning and memory as separate silos. We build models that learn on a massive, static pile of data, and then we freeze them. When given something new, deep learning models suffer from a form of amnesia known as "catastrophic forgetting"—which is less like forgetting a name and more like learning a new language and suddenly forgetting how to walk. Because the pathology is baked into the way weights are updated, fixing it requires a fundamental change to the learning process itself. This realization is what drove me to study continual learning with a specific focus on adaptive optimization.


<br>

{: .drop-cap }
**What is continual learning?**

Think of it this way: Suppose you want to build a model of a house to help a robot navigate.
Under current "Standard" Machine Learning, the robot needs scenes from every room—the kitchen, the bedroom, the attic—fed to it in a shuffled, random order, over and over. If you add a new sunroom, you can’t just "show" it the sunroom. You have to take the robot back through the entire house, plus the new room, and retrain it from scratch.
A Continual Learning algorithm, by contrast, learns more like a human. It processes the kitchen, then the bedroom, then the attic. When the sunroom is added, it simply walks in, learns the new space, and integrates it without forgetting where the kitchen is. 

Formally, CL is the study of algorithms that can handle a *non-stationary* sequence of data while maintaining high performance throughout. It is the quest for **adaptivity** —the ability to change  every time the world does without an engineer needing to intervene. This mission involves balancing several competing dimensions: not just memory, but also *plasticity* (the ability to keep learning) and *forward transfer* (using old knowledge to learn new things faster). Although the debate on the desiderata of continual learning has yet to be settled (e.g., see [Hadsell et al., 2020](https://www.cell.com/trends/cognitive-sciences/fulltext/S1364-66132030219-9)), the concept of adaptivity is at the core of it. 

<br>


{: .drop-cap }
**Adaptivity is, I think, the quiet center of how we see intelligence.** Whether you are reading a dense scientific paper or a speculative blog post, there is a shared intuition: if it cannot adapt, it isn't truly intelligent. The exhaustive *Collection of Definitions of Intelligence* ([Legg and Hutter, 2007](https://arxiv.org/pdf/0706.3639)) makes this point fairly plain: adaptivity—or equivalently, the ability to learn from and function within a changing environment—is present in nearly all of the seventy different definitions they surveyed.
But rather than diving deeper into the philosophical debate surrounding these definitions, I’d like to add to the discussion definitions from a different source: cinema.
<!-- 
[citation of some movie critic here that says that Movies are a good representation of the culture of a society and of the topics that live in the public discourse of the time.] So I think they can be just as telling, regarding how AI was ever portrayed to be on TV. -->

As the film theorist Siegfried Kracauer famously argued, films are a mirror of the "psychological disposition" of a nation. They show us not just what we can build, but what we expect from the things we build.

Take, for example, the T-800 from Terminator 2: Judgment Day. Beyond being [the most iconic AI to ever wear a leather jacket](https://www.youtube.com/watch?v=XPtVZ69lomk)), the T-800 (played by Arnold Schwarzenegger) provides a perfect accidental lesson in Continual Learning. In a famous scene cut from the original theatrical release, the young John Connor asks the robot a fundamental question about its nature:

<div class="video-container" align="center">
<!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/fYTOK89w8h0?si=ejV5oHcogMFeglZ_&amp;start=35" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe> -->
<iframe width="600" height="315" src="https://www.youtube.com/embed/LGC8nY-s7jc?si=FBBu9q2AUGG0dQ8h" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>
<br>

After discovering that the Terminator's battery lasts for 120 years, young John Connor asks: "*Can you learn stuff that you haven't been programmed with, so you can be, you know, more human?*" The T-800’s reply is a minor masterpiece of accidental technical accuracy: "*My CPU is a neural net processor, a learning computer. But Skynet presets the switch to read-only when we are sent out alone*". 

It turns out the unknowing writers, James Cameron and William Wisher, managed to perfectly describe the awkward gap between what the public expects from AI and how we actually deploy it. We want systems that learn and grow "more human" through experience, yet we—the somewhat over-cautious designers—usually flip the switch to "read-only" the moment the model leaves the lab. We’ve spent decades building machines that are essentially frozen in time.

Beyond the Terminator, most AI characters in fiction are portrayed as fully autonomous and capable of acquiring information rapidly. This is true in older films—think of C-3PO and R2-D2 in Star Wars, HAL 9000 in 2001: A Space Odyssey, or the centuries-long evolution of Andrew Martin in Isaac Asimov’s Bicentennial Man—as well as more recent examples like Samantha in Her or WALL-E. 

A charmingly literal scene that shows this ability to learn is that of Johnny 5 in the 1986 film [Short Circuit](https://www.imdb.com/title/tt0091949/?ref_=nv_sr_srsg_0_tt_8_nm_0_in_0_q_short%2520circuit): 

<div class="video-container" align="center">
<iframe width="600" height="315" src="https://www.youtube.com/embed/WnTKllDbu5o?si=6UoXjS-C0fUtxhOs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>
<br>

What we’re seeing here is Continual Learning in its purest, most chaotic form. Johnny 5 doesn't learn about the world in a basement before being shipped out; he learns after deployment. He is a fully functional agent who treats every new interaction as "Input!"—integrating new information without needing a factory reset or a specialized fine-tuning team.

According to a survey on fictional robots ([Saffari et al., 2021](https://www.researchgate.net/publication/351782098_Does_cinema_form_the_future_of_robotics_a_survey_on_fictional_robots_in_sci-fi_movies)), there are hundreds of movies featuring autonomous machines. In almost every single one, the ability to learn is simply "given." The public discourse has mostly moved on to worrying about more dramatic things, like *consciousness*, *existential threats*, or whether a machine can feel love. These are all perfectly valid questions for a late-night debate, I suppose. But from where I sit in the lab, they feel a bit like worrying about the wallpaper before you’ve checked if the house has a foundation. In the movies, adaptivity is a given. In reality, it’s a stubborn bottleneck that we’re still trying to solve.

<br>

{: .drop-cap }
**So how far are we from *fully ~~intelligent~~ autonomous systems*?**

Some limited forms of adaptation are already possible. For most of what we actually need to do today, the solutions we have are honestly quite impressive. We use dynamic memory banks like RAG to give models a "library," agentic tools to compress context, and techniques like experience replay or LoRA to nudge models into new behaviors. For a short-term deployment, these are robust solutions. They work.

However, they all share a fundamental, somewhat nagging limitation: they aren't particularly efficient. In our current paradigm, every time the system's memory grows, the computational "tax" required to manage it grows right along with it. This makes it difficult to actually let these algorithms loose in the real world for long periods without a centralized, expensive update. For many of the tasks we’re solving right now, this is a perfectly acceptable compromise. But if we’re looking toward a future where systems need to be truly autonomous, this high-maintenance relationship becomes a bit of a dealbreaker.

I’ve always thought the best part of being in academia is the license to be slightly impractical. While industry is busy solving the problems that need to be fixed by Tuesday, we get to think about the things that people haven't even realized are problems yet. It’s a luxury to be able to dream.
In that spirit, my research aims for something much more ambitious than the "limited autonomy" we see in current models. What I’m interested in is something like a **human-lifetime autonomy** -I call it the *one hundred years mission*. I imagine a future where artificial agents can act and learn independently for a century without needing a factory reset, a centralized patch, or a sudden "senior moment" where they forget everything they learned in the first fifty years.

I hold the conviction that this symbolic 100-year goal is not the kind of problem we can just throw more GPUs at because it is a foundational problem. To get there, we need to find a way for *memory to scale sublinearly with compute*. We need to understand the theoretical lower bounds of the memory a system can hold onto its history without its energy requirements or processing time exploding. In short, we need theory. I have a detailed research plan for how we might start chipping away at this. If you’re a fellow researcher and want to talk about the math of long-term autonomy, feel free to drop me an email. I’m always looking for people who are just as stubborn about the future as I am.
<!-- here make a link to the FAQ about continual learning page -->


<br>
<!-- All of what I said does raise a fairly awkward question: if autonomy is so central to intelligence, why didn’t we just build systems this way from the start? It feels a bit like we spent seventy years ignoring the obvious and  taking a very scenic, very expensive detour through the land of static datasets.
The answer is a messy mix of mid-century philosophy, early engineering pragmatism that eventually hardened into dogma, and some deep-seated historical biases about what "thinking" actually looks like. But that’s a story for another post. 
--> 

*And that's all for today, until next time!*
<!-- https://en.wikipedia.org/wiki/History_of_artificial_intelligence -->
