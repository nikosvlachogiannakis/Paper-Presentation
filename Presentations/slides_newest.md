<!-- night_gold.css Theme -->

# One or two things we know about concept drift — a survey on monitoring in evolving environments. Part A: detecting concept drift

**Fabian Hinder, Valerie Vaquet, Barbara Hammer**

<!-- **Nikos Vlachogiannakis, 2025** -->

---

## Table of Contents

<div style="display: flex; gap: 0em; text-align: left;">

<div style="flex: 1;">

- [Introduction](#/2)
  - [Focus of the paper](#/2/1)
- [Concept Drifts Categories](#/3)
  - [Supervised & Unsupervised Settings](#/3/2)
- [Drift Detection Setup](#/4)
- [General Scheme for Drift Detection](#/5)
  - [Stage 1: Data Aquisition](#/5/1)
  - [Stage 2: Building a Descriptor](#/5/2)
  - [Stage 3: Computing Dissimilarity](#/5/3)
  - [Stage 4: Normalization](#/5/3)
- [Categories of Drift Detectors](#/6)
  - [Two-sample Analysis](#/6/1)
    - [1. Loss-Based Approach](#/6/2)
    - [2. Virtual-Classifier-Based](#/6/3)
    - [3. Statistical-Test-Based](#/6/4)
  - [Meta-Statistics](#/6/5)
    - [1. AdWin](#/6/6)
    - [2. ShapeDD](#/6/7)

</div>

<div style="flex: l;">

  - [Block-Based Methods](#/6/8)
    - [1. Independence-Test-Based](#/6/9)
    - [2. Clustering-Based](#/6/10)
    - [3. Model-Based](#/6/11)
- [Analysis of Strategies](#/7)
  - [Datasets](#/7/1)
  - [Methods](#/7/2)
  - [Evaluation](#/7/3)
- [Results](#/8)
  - [Summary](#/8/5)

</div>

</div>

---

# Introduction

- **<span style="color:skyblue">Concept drift</span>**: when data distribution changes 
over time

- Drift is studied in <span style="color:skyblue">stream setups</span>

- Processing drifting data streams involves 2 tasks:
  - <span style="color:skyblue">Online(or stream) learning</span> for predictive tasks

  - <span style="color:skyblue">Monitoring systems</span> for unexpected behavior

Note: 
- Analyzing concept drift is crucial for automated systems
- Detecting anomalies is essential for identifying faulty productions
- <span style="color:skyblue">stream setups</span>: where changes in the underlying 
data distribution necessitate model adaptation or alerting a human operator 
for corrective action.
- Concept drift appears in time-series data
- Time series: Such drift usually manifests itself as trends, and its absence is known 
as stationarity
- Drift must be taken into account in transfer learning, a deep learning technique in 
which the model is pre-trained on a similar task with a more extensive dataset before 
being fine-tuned on the target task using a limited dataset. 

--

<img src="figures/paper_focus_dark.png" alt="Drift Analysis Categorization" style="width:60%">

Note: This paper focuses on unsupervised drift detection for monitoring situations where
 drift is expected due to sensor usage or environmental changes

---

# Concept Drift

<img src="figures/drift_classes_dark.png" alt="Drift Classification">

--

-  <span style="color:skyblue">Real Drift</span>: Changes in the conditional 
distribution $D_t(Y | X)$

- <span style="color:skyblue">Virtual Drift</span>: Changes within the marginal 
distribution $D_t(X)$ 

<img src="figures/real_virtual_drift_dark.png" alt="Real & Virtual Drift">

Note:

- Concept drift can be subdivided into two types: virtual drift and real drift. 

- Virtual drift can be defined as a change in the unconditional probability distribution 
P(x)

- Real drift can be defined as a change in the conditional probabilities P(y|x)

- They may occur separately or simultaneously and may have different impacts on the 
classifier performance.

--

# Supervised Settings
- Both real and virtual drift might be present
- Focuses on model losses

<br><br>

# Unsupervised Settings
- Only virtual drift has to be considered
- Emphasizes on the data distribution or reconstruction

---

# Drift Detection Setup

- Drift detectors are decision algorithms on data-time-pairs that determine whether 
there is drift

- Accurate drift detection model if:
  - The algorithm always makes the right decision if we provide enough data
  - Can control the chance of false positives independent of the stream.

- Drift detection is typically applied in a streaming setting where data points arrive 
over time

- Samples are analyzed within a time window

Note:  False Positive: Type 1 error. A valid drift detector should allow you to set and 
bound the probability of raising a false alarm — detecting drift when in reality there 
is none —and this bound should not depend on the particular data stream or distribution.

---

# General Scheme for Drift Detection

<div style="display: flex; gap: 0em; text-align: left;">

<div style="flex: 1;">

**<span style="color:skyblue">Four-stages:</span>**
  1. Data acquisition (sliding windows)
  2. Building descriptors
  3. Computing dissimilarity
  4. Normalization & decision

</div>

<div>

</div>
<img src="figures/4_stages_dark.png" alt="4-Stage Scheme" width="350" height="600">
</div>

--

# Stage 1: Data Acquisition

- **Input:** Data Stream  
- **Output:** Windows of data samples

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

<br><br>

### Possible Descriptors

- grid- or tree-based binnings
- neighbor-, model- and kernel-based approaches

Note: The <span style="color:skyblue">goal</span> of the second stage is to provide a 
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

<br><br>

# Stage 4: Normalization

- **Input:** Dissimilarity Score 
- **Output:** Normalized Dissimilarity

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

Usually, the scores suffer from estimation errors. This is counteracted by a 
suitable normalization similar to the p-value in a statistical test. Other examples of 
normalized scales are accuracy or the ROC-AUC.

---

# Categories of Drift Detectors

<img src="figures/drift_det_types_dark.png" alt="Types of Windows">

Note:

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

# Two-Sample Analysis

- Split a sample $S(t)$ into two samples $S_{-}(t)$ and $S_{+}(t)$
- Apply the test to those samples

Note: In severe cases, choosing an unsuited split can make the drift vanish and thus 
undetectable as we consider time averages of the windows. However, there exist 
theoretical works that suggest that the averaging out does not pose a fundamental 
problem

Computing a measure of dissimilarity between the two samples, such as differences in 
their empirical distributions or statistical properties. They help identify whether the 
data distribution has changed over time, signaling drift in data streams

--

## Testing Procedures

<br><br>

### 1. Loss-Based Approach

- Machine learning models evaluate the similarity of new to existing samples
- Unsupervised
- Reference window (Stage 1) stored in a machine learning model which is used as data 
descriptor (Stage 2)
- Dissimilarity is usually given by the model loss

Note: Implementing this strategy: 
- Auto-Encoders(compress & reconstruct the data)
- 1-class SVMs or Isolation Forests (Originating from anomaly detection, they provide an
 anomaly score that estimates how anomalous a data point is)
- Density estimators, which are designed to estimate the likelihood of
observing a sample, can be applied to detect drift
- It is further analyzed using drift detectors which are commonly used in the supervised
 setup and serve as a normalization (stages 3 and 4)

--

### 2. Virtual-Classifier-Based

<img src="figures/virtual_classifier_dark.png" alt="Virtual-classifier">

Note:
- If a classifier performs better than random guessing, then the class distribution must
 be different
- Usage of k-fold evaluation is advised for optimal data usage
- The used model class is crucial in terms of which drift can be detected and how much 
data are necessary
- The chance of choosing an invalid split point is essentially zero
- As a candidate of this class, we consider D3

--

### 3. Statistical-Test-Based

- <span style="color:skyblue">Kolmogorov-Smirnov Test</span>
<img src="figures/Statist_test_dark.png" alt="Statistical-test-based">
- <span style="color:skyblue">Kernel two-sample test</span>
  - Use raw data (Stage 1)
  - Descriptor is given by kernel matrix K (Stage 2)
  - Score is given by the Maximum Mean Discrepancy (Stage 3)
  - Permutation or bootstrap is used for normalization (Stage 4)

Note: 
- Classical statistical tests commonly focus on one-dimensional data
- Kernel two-sample test
  - It is based on the Maximum Mean Discrepancy (MMD)

MMD: It is a statistical distance between two probability distributions, based on 
comparing their means in a high-dimensional feature space defined by a kernel function.

--

# Meta-Statistics
  - Combine values of several estimates to address issues such as the multiple testing 
problem and sub-optimal sensitivity and get better results

Note: 
## Multiple Testing Problem:
When drift detectors test for changes at many time points independently, the chance of 
falsely detecting a drift (false positive) increases, even if no true drift exists. This
 statistical issue—called the multiple testing problem—leads to unreliable results 
unless adjustments (like correction methods or meta-statistics) are applied.

## Sub-optimal Sensitivity:
Sub-optimal sensitivity refers to a detector’s reduced ability to recognize real but 
subtle or complex drifts, especially when it relies on overly simplistic assumptions or 
windowing. As a result, true drifts may go undetected, particularly in high-dimensional 
or correlated data streams.

--

### 1. AdWin (ADaptive WINdowing)
<img src="figures/AdWin_dark.png" alt="AdWin">

Note: Stands for ADaptive WINdowing and is one of the most popular algorithms in 
supervised drift detection. It takes individual scores like model losses or p-values as 
input to estimate the actual change point

--

### 2. ShapeDD (Shape Drift Detector)

Focuses on the discrepancy of two consecutive time windows

<img src="figures/ShapeDD_dark.png" alt="ShapeDD">

--

# Block-Based Methods
  - Do not assume a split of the data into two windows but analyze an entire data 
  segment at once

--

# 1. Independence-Test-Based

- Dynamic Adaptive Window Independence Drift Detection (DAWIDD) is derived from the 
formulation of concept drift as statistical dependence of data X and time T and thus 
resolves drift detection as a test for statistical independence.
- HSIC test is a kernel method similar to MMD and we use it on Stage 2 & 3
- DAWIDD makes the fewest assumptions on the data or the drift
- This allows for detecting more general drifts but comes at the cost of needing more 
data, a usual complexity-convergence trade-off

Note: DAWIDD is again a statistical test, it is also universally valid and surely 
drift-detecting.

--

# 2. Clustering-Based
  - Cluster time points into intervals such that the corresponding data points also form
 clusters, solving an optimization problem for a number of clusters

--

# 3. Model-Based

- Construct new kernels using machine learning models (e.g. Moment Trees)

Note: 
- <span style="color:skyblue">Moment Trees</span>: Random Forests with a modified loss 
function that is designed for conditional density estimation
- This way, we obtain model-based block-based approaches that can be thought of as an 
extension of the virtual-classifier-based two-window approaches to continuous time by 
removing the time discretization

---

# Analysis of Strategies

Note: 
- Provided definitions of drift and drift detection
- Discussed the relevance of unsupervised drift detection in the motoring setting
- Categorized state-of-the-art approaches
- Analyzed them based on a general, four-staged scheme
- Summarize how different methods are implemented
- analyzed the different underlying strategies on simple data sets to showcase the 
effects of various parameters reducing the effect of other dataset-specific parameters

# Part B Paper

This paper is a survey on concept drift in unsupervised data streams, focusing 
specifically on how to locate and explain drift after it is detected. It formally 
defines drift localization and explanation, reviews methods for identifying where and 
how drift occurs in data, and compares strategies experimentally on artificial datasets.
 The authors also discuss challenges in making these explanations understandable for 
human operators and propose guidelines for practical applications.

--

# Datasets

<img src="figures/datasets_dark.png" alt="Datasets">

- Generate data streams consisting of 750 samples
- Drift times randomly picked between t = 100 and t = 650 by changing:
  - Intensity
  - Number of drift events
  - Number of dimensions

Note: 
1. Uniformly sampled from the unit square, drift is introduced
by a shift along the diagonal. Intensity is shift length; noise in
additional dimensions is uniform
2. Data sampled from a Gaussian (normal) distribution with
correlated features, drift flips the sign of the correlation.
Intensity is correlation strength; noise in additional dimensions
is Gaussian
3. Data sampled from two overlapping uniform squares, drift
rotates by 90o . Intensity is inverse to the size of overlap; noise
in additional dimensions is uniform

Illustration of used datasets (default parameters, original size). Concepts are 
color-coded (Before drift: blue/yellow, after drift: green/purple)

- Intensity, default is 0.125.
- Number of drift events, default is 1.
- Number of dimensions by adding non-drifting/noise dimensions, default is 5, that is, 
3 noise dimensions.

--

# Methods

- Split stream into windows of 150 and 250 samples (100 samples are overlapping)

- Two-sample and block-based approaches are applied to each window

- Meta-statistic approaches are applied to the entire stream and the window-wise minimum
 of the score is taken

Note:
- We use D3 (Logistic Regression, Random Forest), KS, MMD, ShapeDD, KCpD and DAWIDD

  - D3 (Logistic Regression, Random Forest): D3 trains a classifier (e.g., logistic 
  regression or random forest) to distinguish between two time windows, using the 
  classifier's performance to infer drift if it exceeds random chance.

  - KS (Kolmogorov–Smirnov): KS compares empirical cumulative distributions feature-wise
   between two windows and signals drift if the maximum difference exceeds a statistical
   threshold.

  - MMD (Maximum Mean Discrepancy): MMD is a kernel-based method that computes the 
  distance between two data distributions in a reproducing kernel Hilbert space to 
  detect drift.

  - ShapeDD: ShapeDD detects drift by analyzing the temporal pattern of a statistical 
  distance (e.g., MMD) across time, identifying characteristic "shapes" that indicate 
  change points.

  - KCpD (Kernel Change-point Detection): KCpD partitions a data stream into segments by
   maximizing within-block kernel similarity and detects drift by identifying optimal 
  segment boundaries.

  - DAWIDD: DAWIDD tests for statistical dependence between data and time using 
  kernel-based independence tests (e.g., HSIC), detecting drift without relying on fixed
   window splits.

- For MMD, ShapeDD, KCpD, and DAWIDD, we used the RBF kernel and 2,500 permutations
- For KCpD, we use an extension and chose the smallest α-value to detect a drift as 
a score. All other methods provide a native score
- By taking these models we cover every major type and sub-type

<!-- -- -->

<!-- ### D3
- If the classifier performs better than random, it suggests drift

### Kolmogorov-Smirnov (KS)
- Compares the empirical distribution functions of two samples (before/after)

### Maximum Mean Discrepancy (MMD)
- Measures the distance between the distributions

### ShapeDD
- Focuses on the “shape” of drift signals to detect meaningful changes robustly

### Kernel Change-point Detection (KCpD)
- Maximizes the mean value of blocks along the diagonal of a kernel matrix

### DAWIDD
- Tests the independence between data and time directly -->

--

# Evaluation

Repeat each setup <span style="color:skyblue">500</span> times. 

The performance is evaluated using the <span style="color:skyblue">ROC-AUC</span>. 


### ROC-AUC

- Provides a scale-invariant upper bound on the performance of every concrete threshold 
- Not affected by class imbalance

Note:
- AUC = 1 if the largest score without drift is smaller than the smallest score with 
drift
- AUC = 0.5 if the alignment is random.
- ROC-AUC: Measures how well the obtained scores separate the drifting and 
non-drifting setups
- <span style="color:red">It is not affected by class imbalance</span> and thus a 
particularly good choice as the number of chunks with and without drift is not the same 
for most setups.

ROC: A graph that plots the True Positive Rate (sensitivity) against the False Positive 
Rate (1 - specificity) at different classification thresholds. 

AUC: The area under the ROC curve, summarizing the model's overall performance in 
distinguishing between classes.

In essence, ROC AUC helps assess how well a model can separate the classes it's trying 
to predict. It's a valuable tool for comparing different models and choosing the one 
that best suits the specific classification task.

---

# Results

--

<img src="figures/performance_dark.png" alt="Drift detection performance for various 
intensities, number of dimensions, and number of drift events" style="max-width: 75%; 
max-height: 75%;">

Note: We evaluate the detectors’ capability to detect very small drifts. From a 
theoretical perspective, we expect that smaller drifts are harder to detect.

# Drift intensity
- Detectors perform better with increasing drift strength
- ShapeDD was notably effective due to its use of MMD, emphasizing the importance of 
meta-heuristics
- The model choice for D3 impacts performance, with simpler models like Logistic 
Regression working well on easy datasets, whereas more complex models are needed for 
intricate data
- KCpD outperforms all methods, especially at higher intensities and its close with 
ShapeDD

### Drift in correlating features

- Drift can affect the correlation or dependency of several features only, in which case 
it cannot be detected in the marginal features.
- We captured this phenomenon in the Gauss and two-overlap datasets.
- KS performance close to random chance and D3 with Random Forests shows similar issues 
that <span style="color:red">cannot</span> be observed for the kernel-based methods.

# Total Number of Dimensions

- All methods suffer heavily from high dimensionality
- Kernel-based methods, this can be explained by the choice of the RBF kernel, also 
explaining why global KCpD performs quite poorly
- D3 with Random Forest, this result is somewhat surprising due to the inherent feature 
selection of tree-based methods
- On Gauss where trees have a harder time exploiting the structure, the method suffers 
the most

# Number of drift events

- Results for different numbers of change points, alternating between two distributions
- All drift detectors suffer in this case, which is particularly interesting for global 
KCpD

### Advice

- Suggest incorporating as much domain knowledge into the choice or construction of the 
descriptor as possible.
- Recommend the usage of meta-statistic or block-based methods.
- Advise only using methods that make heavy use of feature-wise analysis if drift in the
 correlations only is either less relevant or very unlikely. If this is not an option, 
ensemble-based drift detectors that combine feature-wise and non-feature-wise approaches
 may provide an appropriate solution
- Advise making use of block-based drift detectors if several drift events are to be 
expected. In particular, we suggest not to make use of meta-statistic-based methods 
unless they can explicitly deal with the setup

--

<img src="figures/effect_dark.png" alt="Effect of total number of dimensions or choice 
of split point for various drift intensities and drift detectors">

Note: Analyzed the behavior in the case of the uniform dataset with a single drift in 
the middle and 250 samples, either with optimal or with random split points

Effect of total number of dimensions or choice of split point for various drift 
intensities and drift detectors. Graphic shows median (line), 25% − 75%-quantile 
(inner area), min − max-quantile (outer area), and outliers (circles)

# Total Number of dimensions

- For D3 and MMD, the drift becomes harder to detect
- KS suffers from the multi-testing problem, that is, drift-like behavior emerging by 
random chance

# Effect of split point

- Observe a significant increase in performance which is also more reliable in case of a
 correct split point

### Advice

- Choosing appropriate preprocessing techniques or descriptors to select or construct 
suited features
- In case of high dimensionality with high cost in case of false alarms, one should 
refrain from using drift detectors that operate feature-wise

--

<img src="figures/performance_d3_dark.png" alt="Drift detection performance for various 
models used by D3" style="max-width: 75%; max-height: 75%;">

Note: The metric is implicitly given by the model making it interesting to study

Consider D3 with different models:
- Logistic Regression (log.reg.)
- Random Forests (RF)
- Extra Tree Forests (ET)
- k-Nearest Neighbor classifier (k-NN)

**Extra Trees (Extremely Randomized Trees) Forests** is an ensemble learning method 
similar to Random Forests but with more randomness introduced during tree construction. 
Unlike Random Forests, which select the best split among a random subset of features, 
Extra Trees choose split thresholds randomly, making them faster to train and often 
reducing variance at the cost of slightly increased bias.


The performance is strongly impacted by the model and its interplay with the dataset

- k-NN is best on Gauss but worst on uniform
- similar models behave alike, for example Extra Tree Forests and Random Forests
- Thus, models pose a way to integrate prior knowledge into the detection

This result matches the observations of Hinder et al. (2022b) where the authors argued 
that the descriptor (stage 2) is more important than the metric (stage 3) derived from 
it.

--

<img src="figures/performance_loss_dark.png" alt="Drift detection performance of various
 model/loss-based approaches" style="max-width: 100%; max-height: 100%;">

Note: Considered outlier- and density/loss-based approaches
- One-class-SVM (SVM; RBF kernel)
- Local Outlier Factor (LOF; k = 10)
- Isolation Forests (IF)
- Kernel-Density Estimate (KD; RBF kernel)
- Gaussian Mixture Model [GMM; mixture components ≤ 10 cross-validation (CV) or 
Dirichlet prior (Bayes)]

Use either the outlier score or the sample probability as the drift score

Use the first 100 samples for training, and the remainder is used for evaluation

Due to poor performance, we increased the default intensity to 0.5 from 0.125

Found additional empirical evidence which challenge the suitability of loss-based 
approaches for drift detection

Suggest the reader not to make use of loss-based approaches

--

# Summary

1. As much domain knowledge as possible should be incorporated when designing drift 
detection schemes

2. Use meta- or block-based methods

3. Choosing good split points is crucial for obtaining good detection capabilities

4. Feature-wise analysis should only be performed if it is expected that the drift 
does not inflict itself in correlations

5. On high dimensional data, avoid using dimension-wise methodologies

6. Block-based detectors are suitable when multiple drifts are expected

7. Avoid loss-based strategies when the target of the drift detection is monitoring for 
anomalous behavior

Note: 

1. This concerns selecting appropriate preprocessing techniques, constructing and 
engineering suitable features and choosing fitting descriptors in stages 1 and 2 for the
 process
2. -----
3. -----
4. Otherwise, relying on multi-variant techniques seems to be the better solution
5. especially if false alarms are costly in the considered application. It might be 
beneficial to consider feature selection approaches
6. -----
7. -----

Datasets are simple for the sake of controlling a number of parameters

Were able to confirm the theoretical considerations of (paper) in our experiments

---

<!-- # Results & Takeaways

- Drift is formalized as **statistical dependence between data & time**
- Different methods have strengths & weaknesses depending on the scenario
- First comprehensive survey focused on **unsupervised monitoring**
- Practical guidance for monitoring & anomaly detection in real-world systems

Notes:
Highlight the main takeaway: the paper clarifies the landscape of unsupervised drift 
detection for monitoring tasks.  
It shows no single method is best; choice depends on the problem specifics.  
Provides clear guidance for practitioners.

--- -->

# Thank You!
