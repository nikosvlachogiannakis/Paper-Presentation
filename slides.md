# One or two things we know about concept drift

**Fabian Hinder, Valerie Vaquet, Barbara Hammer**
*Frontiers in AI, June 2024*

---

# Problem & Motivation

- **Concept drift**: when data distribution changes over time
- Detecting and analyzing concept drift is crucial for automated systems in critical
 infrastructure, manufacturing, and quality control
- Detecting anomalies and changes in sensors is essential for identifying faulty 
productions, replacing sensors, or modifying system processing
- Concept drift extends beyond data streams, appearing in time-series data and federated
 learning techniques
- Processing drifting data streams involves 2 tasks:
  - Online(or stream) learning for predictive tasks
  - Monitoring systems for unexpected behavior
- This paper focuses on unsupervised drift detection for monitoring situations where 
drift is expected due to sensor usage or environmental changes

---

# Consept Drift in Supervised and Unsupervised Setups

- Drift is classified based on temporal qualities such as abrupt, gradual, incremental,
 and recurring drift
### Supervised Settings
- Both real and virtual drift might be present
- Focuses on model losses

### Unsupervised Settings
- Only virtual drift has to be considered
- Emphasizes on the data distribution or reconstruction


<!-- - Reviews & categorizes unsupervised drift detection methods
- Proposes a general four-stage detection scheme
- Compares methods on synthetic data
- Provides practical guidelines for practitioners -->

<!-- Notes: 
Summarize the contributions:
formal definitions, taxonomy of methods, a general detection framework, experiments, and
 guidelines for selecting methods.-->

---

# Drift Detection Setup and Challenges

- Drift detectors are decision algorithms on data-time-pairs that determine whether 
there is drift
- A drift detection model is accurate
or valid when:
  - The algorithm will always make the right decision is we just provide enough data
  - We can control the chance of false positives independent of the stream.
- Drift detection is typically applied in a streaming setting where data points arrive 
over time, and a sample is analyzed within a time window

---

# Methods & Framework

- **Four-stage detection scheme:**
  1. Data acquisition (sliding windows)
  2. Building descriptors
  3. Computing dissimilarity
  4. Normalization & decision

- **Categories of Drift Detectors:**
  - Two-sample tests
    - Computing a measure of dissimilarity between the two samples, such as differences 
    in their empirical distributions or statistical properties. They help identify 
    whether the data distribution has changed over time, signaling drift in data streams
  - Meta-statistics
    - Combine values of several estimates to address issues such as the multiple testing problem and sub-optimal sensitivity
  - Block-based methods
    - Do not assume a split of the data into two windows but analyze an entire data 
    segment at once
  - Clustering-based methods
    - Cluster time points into intervals such that the corresponding data points also form clusters, solving an optimization problem for a number of clusters

<!-- Notes:
The authors propose a general scheme with 4 stages: windowing, describing distributions,
 computing differences, and deciding if drift occurred.  
They also categorize existing methods into 4 types based on their strategy. -->

---

# Analysis of Strategies

- It investigates the role of drift strength, the influence of drift in correlating 
features, data dimensionality, and the number of drift events
- The experiments use three 2-dimensional synthetic datasets with differently structured
 abrupt drift: uniform shift, Gaussian correlation flip, and uniform squares rotation
- ROC-AUC is used to evaluate the performance, measuring how well the obtained scores 
 separate drifting and non-drifting setups

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

### 🦧 That is all 🦧



