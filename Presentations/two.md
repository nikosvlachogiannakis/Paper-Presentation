# Unsupervised Concept Drift Detection with a Discriminative Classifier

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

# Problemn Definition

- Concept Drift: Determines whether the joint distribution differs between $t_0$
and $t_1$

$$p_{t_0}(X,y) \neq p_{t_1}(X,y)$$

---

# Related Work

**Rigorous Concept Drift**: Concept drifts happening on both $p(X)$ and $p(y|X)$

### Approaches for detecting **Rigorous Concept Drift**:
   
   1. Based on **Novelty Detection**(clustering or outlier detection)

   2. Based on **Multivariate Distribution**: Compare batches of data using 
   statistical distances(KL Divergence, Hellinger Distance, etc.)

--

# Best Methods

- Page-Hinckley Test

- Drift Detecion Model(DDM)

-  EDDM

- ADWIN

<!-- ..... -->

---

# Proposed Approach: D3



---

# Evaluation



---

# Results