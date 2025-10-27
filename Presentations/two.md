# Unsupervised Concept Drift Detection with a Discriminative Classifier

Gözüaçık, Ö., Büyükçakır, A., Bonab, H., Can, F. (2019)

Proceedings of the 28th ACM International Conference on Information and 
Knowledge Management

Note: Presents an unspervised method called D3 which uses a discriminative 
cassifier with a sliding window to detect concept drift by monitoring changes in
the feture space.

It outperforms the baselines, yielding models with higher performances on both
real-world and synthetic datasets

---

# Introduction

- Traditional classification tasks assume that data is static which indicate 
  both train and test sets are from teh same distribution

- In a stream setting, D3 is the first method that utilizes a discriminative 
  classifier for **concept drift detection tasks**

Note:

This assumption is often violated as real-world applications are dynamic and 
evolve over time. This is known as **concept drift**

**Data Stream**: Consits of data that arrive over time

### Condtributions of this paper

1. Introduce a simple yet effective method for unsupervised concept drift 
  detection

2. Empirically test the propose method on 8 datasets against widely used concept
  drift detection methods

3. Evaluate the performance of D3 and demonstrate that it outperforms the 
  baselines

---

# Problem Definition

- Concept Drift: Determines whether the joint distribution differs between $t_0$
and $t_1$

$$p_{t_0}(X,y) \neq p_{t_1}(X,y)$$

---

# Related Work

$\newline$
**Rigorous Concept Drift**: Concept drifts happening on both $p(X)$ and $p(y|X)$

$\newline$
### Approaches for detecting **Rigorous Concept Drift**:
   
   1. Based on **Novelty Detection**(clustering or outlier detection)

   2. Based on **Multivariate Distribution**: Compare batches of data using 
   statistical distances(KL Divergence, Hellinger Distance, etc.)

--

# Best Methods

- Page-Hinckley Test
  - Tracks the cumulative difference between the observed values  and their mean
  of the chosen metric

- Drift Detection Method (DDM)
  - Monitors the probability of error rate and its standard deviation

-  Early Drift Detection Method (EDDM)
    - Tracks the distance in between two back-to-back-errors

- Adaptive Windowing (ADWIN)
  - Uses a sliding window, whose size is adaptive. There are 2 sub-windows 
  storing the performance metric of old and new data

Note:

- Page-Hinckley Test
  - It detects a change when the difference is more than a given threshold

- Drift Detection Method (DDM)
  - It signals a drift if the error rate within a period increases substantially

-  Early Drift Detection Method (EDDM)
  - It assumes this distance to be larger if there is no drift and vice versa
   otherwise

- Adaptive Windowing (ADWIN)
  - A drift is detected if the mean of these sub-windows differ by more than a 
  certain threshold

---

# Proposed Approach: D3

- **Unsupervised**

- **Sliding Window**

- Use of a **discriminative classifier** (logistic regression)

- Detects drift when the classifier **performance** (AUC) exceeds a threshold

## Goal

To observe whether 2 sets differ continuously, not to estimate their distributions

Note:

- **Discriminative classifier**: Can be used with any online algorithm without 
  a built in drift detector

- **Sliding Window**: When the whole window is empty, we wait until it gets full.
  The oldest members of size w are labeled as old, and given a value 0. The 
  remaining are labeled new, and given a value 1.

- **Discriminative classifier**: Logistic regression model is trained to 
  distinguish old and new with s as labels

- **AUC**: Expresses to what degree a model can distinguish 2 classes.
  
  **Perfect Model**: AUC = 1  
  **Poor Model**: AUC $\approx 0.5$

### Old data are always larger than New data

--

<!-- D3 Algorithm -->
<img src="figures/D3_workflow.png" alt="D3 Workflow">


Note: 

### Old data are always larger than New data

### w: the size of the old data

### ρ is the percentage of new data with respect to old

### Size for the new data: wρ

Due to different sizes, it is **important** to use a metric that works well in the presence of **class imbalances**, that is why we use AUC.

---

# Evaluation

- D3 is tested on 8 real-world and synthetic datasets

- Hoeffding Tree is used due to its effectiveness

- Comparison with:
  - ADWIN
  - DDM
  - EDDM

--

# Hoeffding Trees (Very Fast Decision Trees)

- **Purpose:** Incremental decision trees for **data streams** — process one 
  instance at a time without storing past data.  
- **Key Idea:** Use the **Hoeffding Bound** to decide when a node should split, 
  ensuring statistically confident decisions with limited samples.  
- **Formula:**  
  $$\epsilon = \sqrt{\frac{R^2 \ln(1/\delta)}{2n}}$$
  where $\epsilon$ bounds the error in selecting the best attribute.
- **Advantages:** Fast, memory-efficient, scalable, and always ready to predict.  
- **Limitation:** Assumes stable data distribution — needs drift detectors 
  (like **D3**) to handle evolving streams.  
- **In D3:** Used as the **base classifier** for online learning and drift 
  evaluation.

---

# Results

$\newline$
## Changes in $P(X)$ (Virtual Drift)

- D3 acts **quicker** by signaling a drift

$\newline$
## Changes in $P(y|X)$ (Real Drift)

- D3 **cannot** detect drift

$\newline$
#### D3's performance on real-world datasets is better compared to synthetics

Note: In synthetic datasets, real and virtual concept drifts can be more 
dominant

--

# Friedman Test with Nemenyi post-hoc analysis

- Checks the statistical significance of the used methods

<!-- D3_critical_distance -->
<img src="figures/D3_critical_distance.png" alt="Critical Distances">

Note: The critical distance for Nemenyi Significance is calculated by plugging 
in values specific for our setting from the Critical Values Table for Two Tailed
 Nemenyi Test

---

# Thank You!