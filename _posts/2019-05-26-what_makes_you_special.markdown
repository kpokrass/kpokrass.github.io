---
layout: post
title:      "What Makes You Special? "
date:       2019-05-27 01:54:11 +0000
permalink:  what_makes_you_special
---

## Interpreting the composition of an anomaly using SHAP values




### INTRODUCTION

Anomalies are described as observations that are significantly different from the majority of the data and their detection is an import process for many industries. For example, banks need to have methods for detecting anomalous purchases as these may indicate fraud. However, as the amount of data collected increases, the expenditures of sorting through it for anomalies also increases beyond what a human can manage. Thankfully, there are machine learning methods that can quickly identify suspicious data points and return them for further analysis by a domain expert. However, these methods fail to shed light on WHY a particular observation was returned as anomalous. Without justification/explanation for its identification, the domain expert is responsible for the expensive (in terms of both time and money) task of determining the reason behind the suspicious observation.

A recent paper from Antwarg, Sharpia, and Rokach presented a method to reduce this expense. It uses an autoencoder and SHAP values to give a domain expert information as to what features of the data contributed to an observation being labeled as an anomaly ([Link to paper](https://arxiv.org/pdf/1903.02407.pdf)). 

### AUTOENCODERS

Autoencoders have been around since the 1980’s and they are traditionally used in the form of unsupervised neural networks. In short, an autoencoder takes in a dataset and reduces (encodes) it to a lower dimension. Then, the autoencoder tries to reconstruct (decode) the original input values from the lower dimensional data. The difference between the input value and the reconstructed value for a particular observation in a dataset is known as the reconstruction error. This reconstruction error is what is used to pull anomalies out of the dataset as the higher the error, the greater probability that observation represents a suspicious event. 

### SHAP VALUES

SHAP (**SH**apley **A**dditive ex**P**lanations) values are importance values assigned to each feature that contributed to a model’s predicted value. SHAP values unify work from previous additive feature attribution methods, such as LIME and DeepLIFT, and are based on the concept of Shapley values from game theory. In game theory, a group of players cooperates and benefits from that cooperation. Each player may contribute to the group to varying degrees, and thus has varying levels of importance to the overall cooperation gain the group experiences. For the method described in this paper, each feature is assigned a SHAP value for each anomalous observation that is some fraction of the overall reconstruction error from the autoencoder model.

### METHOD

1. The authors’ method for calculating SHAP values for anomalies detected by an autoencoder are outlined below.

2. Use an autoencoder to detect anomalies within a dataset by assessing the magnitude of the reconstruction error.

3. Reorder the reconstruction error for each feature of an anomaly in descending order and select the top n number of features to analyze (The authors recommend selecting the value of n to include 80% of the reconstruction error.).

4. Calculate SHAP values for each of the top n features using kernel SHAP with the autoencoder built on a sample of 100 instances (“normal’ data points) to develop a local explanation model.

5. Divide the SHAP values into two groups based whether the predicted value was greater than or less than the true input value. The contributing group is comprised of the features that pushed the predicted value AWAY FROM the true input value. The offsetting group is the features that pushed the predicted value TOWARDS the true input value. The absolute value of a feature’s SHAP value corresponds to the how much that feature contributed to the predicted value.

6. Present the anomaly with its corresponding SHAP values to a domain expert to determine if it needs further investigation (true positive) or if it is an “uninteresting” observation (false positive).

### IMPORTANCE

The increase in computing power seen in the last few decades has allowed for the frequent utilization of more complex and computationally expensive models such as neural networks. While this has led to increased performance and predictive capabilities, it has also drastically reduced the interpretability of these models. The methods presented by Antwarg, Sharpia, and Rokach attempts to bring the “under the hood” efforts of a model to the forefront and provide an explanation of why a particular value was predicted. Deployment of their method within industries centered around anomaly detection could result in improvements to current models as insights are gained on the underpinnings of its predictions. Additionally, their method gives supplementary information to the domain expert tasked with analyzing the anomaly to help guide the investigation. This reduces the amount of resources that need to be expended per anomaly and can increase how much a company trusts the predictions of its model.


**The latest documentation for SHAP can be found here:** https://shap.readthedocs.io/en/latest/

**A GitHub repository detailing many of its features can be found here:** https://github.com/slundberg/shap

