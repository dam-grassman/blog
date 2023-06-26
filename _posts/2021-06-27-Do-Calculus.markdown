---
layout: post
title:  Introduction to Causality, Pearl's Framework and Do-Calculus
date:   2021-06-28 15:01:35 +0300
image:  02_causality.png
tags:   Causality
---

# Introduction to Causality

*** 
<blockquote>
    <p>“Data are profoundly dumb [...]. <br>
    That’s all a deep-learning program can do : fit a
function to data.“
    </p>
</blockquote>
*** 

<p style="text-align:right;">-- <strong>Judea Pearl</strong> in ”The Book Of Why”</p>

Causality is definitely one of the hottest topic of the recent years, and rightly so ! It is seen seen as one of the keys to help current AI system reaching a next step towards true intelligence,   as highlighted by Turing-Award winner Yoshua Bengio at Neurips 2020 : “[We] won’t deliver a true AI revolution, until it can go beyond pattern recognition and learn more about cause and effect“.<br>

Let's look at two examples to illustrate why we need causality : (1) an interesting statistical paradox to raise curiosity and (2) the famous brain-teaser Monty Hall problem

## A. Simpson's Paradox

<b><i>Context</i></b> : we want to investigate which of two treatments on tumor is the most effective. To do so, we come accross a study that have been conducted over the years, aiming at comparing the recovery rate between our two treatments of interest : A (Take medication) and B (Have surgery). At first sight, the data collected seems to be quite decent : around 2000 patients (quite as sample!) have been given either treatment A or B  in equal proportion. <br>

<b><i>Question</i></b> : If tomorrow I'm diagnosed a tumor, which one of the two treaments should I take ?<br>

First instinct would be to simply look at the global recovery rate of the two subgroups, the ones who have been given treatment A (medication) against the ones that have received treatment B (surgery). 

<p style="text-align:center;"><img style="max-width: 25%; height: auto"  src="/blog/images/simpson1.png" /></p>

Anyone after looking at the data would have this gut feeling that treatment A seems to be better suited to recover from his tumor. Let's bring the medication !<br>
.. well not so fast !!

The "size of the tumor" feature have been collected in the data, sorting tumor in two subgroups "small" or "big" based on the diameter of the tumor. Let's have a look a it :

<p style="text-align:center;"><img style="max-width: 45%; height: auto" src="/blog/images/simpson2.png" /></p>

Treatment B, i.e having surgery, seems to be better for small tumors .. and better as well for big tumors. How come Treatment B now be better for <b>both groups</b> ? Figures from both tables come from the same survey but now leads to two really distinct conclusion !<br>

<i>Historically</i>, as a matter of life/death decisions when dealing with big tumors, patients would most of the time be given surgery. By nature, bigger tumors leads to a lower recovery rate (due to higher risks of complications) and to a more recurrent use of surgery.  
The size of the tumor acts here as a <b>confounding factor</b>.<br>

<p style="text-align:center;"><img style="max-width: 60%; height: auto" src="/blog/images/simpson3.png" /></p>


We are studying the causal effect between X(treament) → Y (recovery), but a third variable Z which is the size of the tumor affects both the recovery and the treatment given in our observational dataset. One has to be careful when using <b>retrospective studies</b> - in contrast with projective study where links between potential confounding factors and causes can be broken with randomization for instance. 

# Do-Calculus and Pearl's Framework

*** 

<blockquote>
    <p>“Statistics focused exclusively on how to summarize data, not on how to interpret it [...]. Data can tell you that people who took a medicine recovered faster than those who did not, but they can’t tell you why.“
    </p>
</blockquote>
*** 

<p style="text-align:right;">-- <strong>Judea Pearl</strong> in ”The Book Of Why”</p>

Judea Pearl introduced the concept of <i>Ladder of Causation</i>, as a conceptual and abstract way to sort the different levels at which us, as human, reason. It captures the differents and gradual notions of causality, and accounts for the next challenges machine learning will have to face :
<ol>
  <li> <i>Association</i> - the first level consists in <i>seeing</i>, or <i>observing</i>, which cover most of the robot and animal's  learning. It enables to answer questions like "What if I see ..?", "How are the variables related" or "How would seeing X change my belief in Y"? It corresponds to the estimation of <math>p(Y|X)</math>. </li>
  <li> <i>Intervention</i> - the next level enables you to act within your environment and witness the results of your own actions. It allows you to answer question like "What if I do ..?", "How can I make .. happen?" or "What would Y become if I do X"?. It is really crucial to grasp the difference between the first and second level because here is introduced the notion of causality between what I see and what I do. We aim at estimating <math>p(Y|do(X))</math>. </li>
  <li> <i>Counterfactual</i> - the last, but no least, level of the ladder introduces imagination. What makes human reasoning so singular is its ability to reason counterfactually, that is to say, answer to question like "What if i acted differenty?" or "Why did it happen?". This retrospection enables humans to get a much better understanding of their environment and increases significantly their capacity to learn, and to learn quickly.</li>
</ol>

*** 

### The ladder of Causation with an Example

***

<p style="text-align:left;">
    Level 1 - Association 
    <span style="float:right;">
        <i>What does</i> a symptom <i>tell me</i> about a disease ?
    </span>
</p>
<p style="text-align:left;">
    Level 2 - Intervention 
    <span style="float:right;">
        <i>What if I take</i> an aspirin, will my headache be cured ?
    </span>
</p>
<p style="text-align:left;">
    Level 1 - Counterfactual 
    <span style="float:right;">
        <i>Was it</i> my aspirin that stopped my headache?
        <br>
        <i>What woud have happen if</i> I took something else ?
    </span>
</p>
***

## Do-Calculus

<p>So as to fill the gap between the first two levels, Pearls introduced the framework of <i>do-calculus</i>. It plays a major role in relating both observational distributions p(Y|X) and interventional distributions p(Y|do(X)). Basically, we are interested in measuring changes in distribution of some random variables {X, Y, Z, ..} when one performs an intervention <i>do(x)</i>, which forces the random variable X to take the value x, regardless of its causes (or causal ancestors). To do so, one uses a set of rules which allows for measuring and indentifying interventional queries, when specific structural conditions are met in the causal DAG (directed acyclic graph). The main goal is to make interventional queries <i>identifiable</i>, i.e that can be expressed in terms of observational distributions, without the <i>do</i> operator.</p>

Consider a set of random variables {X, Y, Z, W} along with their associated DAG G : 
<ol>
  <li><strong>Rule 1 - Insertion/delation of observations</strong> : if Y and Z are <i>d-separated</i> by <math>X U W</math> in <math>G*</math> - from G by removing arrows pointing into X -, then :  <br>

  <br>
  <p style="text-align:center;"><math>P(Y | do(x), Z, W) = p(Y | do(x), W)</math></p>
</li>
  <li><strong>Rule 2 - Exchange action/observation</strong> : if Y and Z are <i>d-separated</i> by <math>X U W </math> in G<sup>+</sup>- from G by removing arrows pointing into X and arrows pointing out of Z-, then : <br>

  <br>
  <p style="text-align:center;"><math>P(Y | do(x), do(Z), W) = p(Y | do(x), Z,W)</math></p>
  </li>
  <li><strong>Rule 3 - Insertion/Delation of actions</strong> : if Y and Z are <i>d-separated</i> by <math>X \U W</math> in G<sup>++</sup> - from G by (1) removing arrows pointing into X (G*) then (2) removing arrows pointing to Z that are not ancestors of any variable W in G*-, then :<br>

  <br>
  <p style="text-align:center;"><math>P(Y | do(x), do(Z), W) = p(Y | do(x), W)</math></p></li>
</ol>

***

<br>
<br>
