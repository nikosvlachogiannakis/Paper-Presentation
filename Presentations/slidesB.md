<!-- night_skyblue.css Theme -->

# One or two things we know about concept drift—a survey on monitoring in evolving environments. Part B: locating and explaining concept drift

**Fabian Hinder, Valerie Vaquet, Barbara Hammer**

<!-- **Nikos Vlachogiannakis, 2025** -->

---

## Table of Contents

<div style="display: flex; gap: 0em; text-align: left;">

<div style="flex: 1;">

- [Introduction](#/)
  - [Focus of the paper](#/)
- [Drift Detection Setup](#/)
- [General Scheme for Drift Detection](#/)
  - [Stage 1: Data Aquisition](#/)
  - [Stage 2: Building a Descriptor](#/)
  - [Stage 3: Computing Dissimilarity](#/)
  - [Stage 4: Normalization](#/)


</div>

<div style="flex: l;">

- [Results](#/)
  - [Summary](#/)

</div>

</div>

---

<img src="figures/paper_focus_dark.png" alt="Drift Analysis Categorization" style="width:60%">

Note: This paper focuses on unsupervised drift detection for monitoring situations where
 drift is expected due to sensor usage or environmental changes

--

# Supervised vs Unsupervised Settings

## Supervised

- Relies on analyzing model-losses as a proxy

## Unsupervised

- Consider the distribution directly

Note:

# Supervised

How well a specific model can reconstruct the data or perform a prediction or 
forecasting task

--

<!-- # Part B Paper

This paper is a survey on concept drift in unsupervised data streams, focusing 
specifically on how to locate and explain drift after it is detected. It formally 
defines drift localization and explanation, reviews methods for identifying where and 
how drift occurs in data, and compares strategies experimentally on artificial datasets.
 The authors also discuss challenges in making these explanations understandable for 
human operators and propose guidelines for practical applications. -->

# Introduction

- **<span style="color:skyblue">Concept drift</span>**: A change in the data generating distribution

- Drift is studied in <span style="color:skyblue">stream setups</span>

- Processing drifting data streams involves 2 tasks:
  - <span style="color:skyblue">Online learning</span> to keep a valid model performing
some predictive tasks

  - <span style="color:skyblue">Monitoring systems</span> for unexpected behavior

Note:
- <span style="color:skyblue">Stream setups</span>: where changes in the underlying 
data distribution necessitate model adaptation or alerting a human operator 
for corrective action.
- Stream setups are closely related to concept evolution in <span style="color:skyblue">
continual learning</span> where concepts might appear or vanish
- Data stream might suffer from extreme class imbalances which triggers the problem 
called <span style="color:red">catastrophic forgetting</span>
- Concept drift appears in time-series data
- Time series: Such drift usually manifests itself as trends, and its absence is known 
as stationarity
- Drift must be taken into account in transfer learning, a deep learning technique in 
which the model is pre-trained on a similar task with a more extensive dataset before 
being fine-tuned on the target task using a limited dataset.

- <span style="color:lime">Focus:</span> On monitoring scenario & on methods that help 
human operators understand drift

--

# Monitoring

Addresses the following questions about the drift:

1. Whether(and when) a drift occurs (<span style="color:skyblue">drift detection</span>)

2. Severity of the drift (<span style="color:skyblue">drift quantification</span>)

3. Where does the drift occurs (<span style="color:skyblue">drift localization & 
segmentation</span>)

---

# Concept Drift

<img src="figures/drift_classes_dark.png" alt="Drift Classification">

--

<img src="figures/real_virtual_drift_dark.png" alt="Real & Virtual Drift">

Note:

- Concept drift can be subdivided into two types: virtual drift and real drift. 

- Virtual drift can be defined as a change in the unconditional probability distribution 
P(x)

- Real drift can be defined as a change in the conditional probabilities P(y|x)

- They may occur separately or simultaneously and may have different impacts on the 
classifier performance.

---

# Drift Localization & Segmentation

Determines where in data space the detected drift occurs

- If we know which parts of the data space are affected by drift, we can mark all data 
points as drifting

- If we know for every data point whether or not it is drifting, we can mark the 
corresponding parts of the data space as drifting

---

# General Scheme for Drift Localization

<img src="figures/4_stages_dark.png" alt="4-Stage Scheme" width="350" height="600">

--

# Stage 1: Data Acquisition

- **Input:** Data Stream

- **Output:** Windows of data samples
</br>

### Types of Windows

<img src="figures/window_types_dark.png" alt="Types of Windows">

Note: Output: for example, one reference window and one containing the most recent 
samples

There are four main categories which differ in how the reference window is updated

- fixed until an event
-  growing
- sliding along the stream
- implicit as a summary statistic using a model

There also exist approaches using preprocessing such as a deep latent space embedding.

Deep latent space embedding: Deep neural network to transform your raw data into a 
lower-dimensional, more structured representation (latent space) — and then using that 
representation (or embedding) as the descriptor of the data

--

# Stage 2: Building a Descriptor

- **Input:** Windows of data samples

- **Output:** Possibly smoothed descriptor of windows

Note:

### Possible Descriptors

- grid- or tree-based binnings
- neighbor-, model- and kernel-based approaches

The <span style="color:skyblue">goal</span> of the second stage is to provide a 
possibly smoothed descriptor of the data distribution in the window obtained in stage 1.

Binnings can be considered as one of the simplest strategies  
The input space is split into bins and the number of samples per bin is counted

Decision trees can be contructed:
- Randomly, according to a fixed splitting rule
- Using a criterion that takes temporal structure into account

neighbor- and kernel-based approaches: the information is encoded via (dis)similarity 
matrices like the adjacency matrix or kernel matrix

A kernel matrix (also called a Gram matrix) is a square matrix that contains the 
pairwise similarities between all data points in a dataset, measured by a kernel 
function.

--

# Stage 3: Computing Dissimilarity

- **Input:** Descriptor of windows

- **Output:** Dissimilarity score

Note: Different descriptors can be used to compute the same score, and vice versa

- Total Variation norm: Measures the maximum difference between two probability 
distributions.

- Hellinger distance: A symmetric distance between distributions, good for 
probabilities.

- MMD (Maximum Mean Discrepancy): A kernel-based measure — tests if two samples come 
from the same distribution.

- Jensen–Shannon metric: Symmetric, smoothed version of KL-divergence.

- Kullback–Leibler (KL) divergence: Measures how one probability distribution diverges 
from another — asymmetric.

- Model loss: The performance drop of a model trained on one window and tested on 
another.

- Neighborhood intersection: Compares k-nearest neighbor graphs of two datasets.

- Wasserstein metric: Also called Earth Mover’s Distance — measures the “cost” of 
transforming one distribution into another.

- Mean and moment differences: Compare means, variances, and higher moments of the 
distributions.

--

# Stage 4: Normalization

- **Input:** Dissimilarity Score 

- **Output:** Normalized Dissimilarity

Note:

Usually, the scores suffer from estimation errors. This is counteracted by a 
suitable normalization similar to the p-value in a statistical test. Other examples of 
normalized scales are accuracy or the ROC-AUC.

---

# Results



---

# Thank You!
