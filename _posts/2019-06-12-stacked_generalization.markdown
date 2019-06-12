---
layout: post
title:      "Stacked Generalization"
date:       2019-06-12 10:09:52 -0400
permalink:  stacked_generalization
---


## Introduction

One of the most influential papers in the field of data science was published in 1992 by David H. Wolpert concerning the novel technique of “stacked generalization.”  Informally known as “model stacking,” this was proposed as a strategy to “reduce the generalization error rate of a generalizer.” To understand this in layman’s terms, we need to define what a generalizer is and what the generalization error rate is. First, a generalizer is a mathematical function that describes a population from a subsample of that population. Essentially, it uses the subsample to describe (i.e. generalize) the characteristics of the total population. For any subsample that is smaller than the total population, it is reasonable to assume that there will be some part of the total population that the generalizer does not describe well. This inability of the generalizer to describe some portion of the total population is what Wolpert is referring to with the term “generalization error rate.” Thus, his stacked generalization technique can be used to increase how well the mathematical function describes the total population.

## How Stacked Generalization Works

![Basic stacked generalization schema](https://raw.githubusercontent.com/kpokrass/dsc-3-final-project-online-ds-ft-021119/master/stacked_schema.png)

The basic schema of stacked generalization can be thought of as multiple levels of generalizers. Essentially, the data (a learning set of dimension (j,k)) is fed to n number of first-level generalizers. These first-level generalizers each make a prediction on the data to produce second-level data of dimension (j,n). This second-level data is then presented to the second-level generalizer, which produces the final predictions for the dataset. Of course, this schema could also be extended to more than two levels of generalizers if required by the particular problem being solved.

> "Stacked generalization isn’t just a way of determining which [first-level] generalizer works best (as in cross-validation), nor even which linear combination of them works best; rather stacked generalization is a means of non-linearly combining generalizers to make a new generalizer, to try to optimally integrate what each of the original generalizers has to say about the learning set." - David H. Wolpert

There are two main reasons why stacked generalization improves upon the performance of a single generalizer. First, the composition of the first-level generalizers allows for greater coverage/description of the total population. For example, let’s say our first-level has three generalizers, A, B, and C. Generalizer A describes 70% of the total population, whereas generalizers B and C describe 10% and 5%, respectively, of the total population. Because generalizer A does a good job of describing the population, generalizers B and C would be useless, or at best redundant, unless they described a portion of the population that was not covered well by generalizer A. By including generalizers B and C roughly 85% of the total population is described. Thus, combining the three first-level generalizers allows for a better generalization of the total population over any single generalizer alone.

Second, utilizing a second-level generalizer allows for a more sophisticated combination of the outputs from the first-level generalizers. Two common, unsophisticated methods for combining the outputs of the first-level generalizers are “winner-takes-all” and a simple average. The “winner-takes-all” approach would just base the final output on the first-level generalizer output that has the highest correlation with the correct value. In our example, this would be no different than simply using the output from generalizer A as the final output. The simple average method is just as it sounds in that the outputs of the three first-level generalizers are averaged together to produce the final output. Both of these methods fail to take into account the strengths and weaknesses of each first-level generalizer. By using a more sophisticated combination method (i.e. a second-level generalizer), the fringe cases covered by generalizers B and C can be combined with the majority of the population covered by generalizer A in a way that increases overall performance.

## Why It Matters

The theories and methods presented in Wolpert’s “Stacked Generalization” paper have been shown to significantly improve the performance of many standard model schemas. Model stacking has demonstrated versatility and consistently strong performance as it has been implemented in the winning strategies of multiple data science competitions. Furthermore, model stacking is relatively easy to integrate into already existing models as the outputs of the existing models would simply need to be configured as inputs for a second model. While this does come with a higher computational expense, the relative ease of implementation and the potential increase in performance outweigh the computational costs in most instances.


## References
Wolpert, David H. (1992). Stacked Generalization, *Neural Networks*, **5**(2), 241-259.
[Link to original paper](http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=6CB61B8B36A1873CF487D97E36F25BBB?doi=10.1.1.56.1533&rep=rep1&type=pdf)


