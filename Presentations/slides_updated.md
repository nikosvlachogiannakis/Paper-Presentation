<!-- night.css Theme -->

# <span style="color:gold;">One or two things we know about concept drift — a survey on monitoring in evolving environments. Part A: detecting concept drift</span>

**Fabian Hinder, Valerie Vaquet, Barbara Hammer**

---

# <span style="color:gold">Introduction</span>

- **<span style="color:gold">Concept drift</span>**: when data distribution changes over
 time
- Analyzing concept drift is crucial for automated systems

- Detecting anomalies is essential for identifying faulty productions
- Drift is studied in <span style="color:gold">stream setups</span>
- Concept drift appears in time-series data
- Processing drifting data streams involves 2 tasks:
  - <span style="color:gold">Online(or stream) learning</span> for predictive tasks

  - <span style="color:gold">Monitoring systems</span> for unexpected behavior

Note: 
- <span style="color:gold">stream setups</span>: where changes in the underlying 
data distribution necessitate model adaptation or alerting a human operator 
for corrective action.

- Time series: Such drift usually manifests itself as trends, and its absence is known as 
stationarity

- Drift must be taken into account in transfer learning, a deep learning technique in 
which the model is pre-trained on a similar task with a more extensive dataset before 
being fine-tuned on the target task using a limited dataset. 

--

<img src="figures/paper_focus.png" alt="Drift Analysis Categorization" style="width:60%">

Note: This paper focuses on unsupervised drift detection for monitoring situations where
 drift is expected due to sensor usage or environmental changes

---

# <span style="color:gold">Concept Drift</span>

- Drift is classified based on temporal qualities such as abrupt, gradual, incremental,
 and recurring drift
### <span style="color:gold">Supervised Settings</span>
- Both real and virtual drift might be present
- Focuses on model losses

### <span style="color:gold">Unsupervised Settings</span>
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

# <span style="color:gold">Drift Detection Setup</span>

- Drift detectors are decision algorithms on data-time-pairs that determine whether 
there is drift
- A drift detection model is accurate
or valid when:
  - The algorithm will always make the right decision is we just provide enough data
  - We can control the chance of false positives independent of the stream.
- Drift detection is typically applied in a streaming setting where data points arrive 
over time, and a sample is analyzed within a time window

---

# <span style="color:gold">General Scheme for Drift Detection</span>

<div style="display: flex; gap: 0em; text-align: left;">

<div style="flex: 1;">

**<span style="color:gold">Four-stages:</span>**
  1. Data acquisition (sliding windows)
  2. Building descriptors
  3. Computing dissimilarity
  4. Normalization & decision

</div>

<div >

</div>
<img src="figures/4_stages_newest.png" alt="Types of Windows" width="350" height="600">
</div>

--

# <span style="color:gold">Stage 1: Data acquisition</span>

- **Input:** Data Stream  
- **Output:** Windows of data samples

### <span style="color:gold">Types of Windows</span>

<img src="figures/window_types.png" alt="Types of Windows" style="width:65%">

Note: Output: for example, one reference window and one containing the most recent 
samples

There are four main categories which differ in how the reference window is updated

- fixed until an event
-  growing
- sliding along the stream
- implicit as a summary statistic using a model

There also exist approaches using preprocessing such as a deep latent space embedding

--

# <span style="color:gold">Stage 2: Building a Descriptor</span>

- **Input:** Windows of data samples  
- **Output:** Possibly smoothed descriptor of windows

### <span style="color:gold">Possible Descriptors</span>

- grid- or tree-based binnings
- neighbor-, model- and kernel-based approaches

Note: The <span style="color:gold">goal</span> of the second stage is to provide a 
possibly smoothed descriptor of the data distribution in the window obtained in stage 1.

Binnings can be considered as one of the simplest strategies  
The input space is split into bins and the number of samples per bin is counted

Decision trees can be contructed:
- Randomly, according to a fixed splitting rule
- Using a criterion that takes temporal structure into account

neighbor- and kernel-based approaches: the information is encoded via (dis)similarity 
matrices like the adjacency matrix or kernel matrix

--

# <span style="color:gold">Stage 3: Computing dissimilarity</span>

- **Input:** Descriptor of windows  
- **Output:** Dissimilarity score

# <span style="color:gold">Stage 4: Normalization</span>

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

# <span style="color:gold">Categories of Drift Detectors</span>

<img src="figures/drift_det_types.png" alt="Types of Windows" style="width:100%">

--

# <span style="color:gold">Two-sample Analysis</span>

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

## <span style="color:gold">Testing Procedures</span>

### <span style="color:gold">1. Loss-based Approach</span>

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

--

### <span style="color:gold">2. Virtual-classifier-based</span>

<img src="figures/virtual_classifier.png" alt="Virtual-classifier-based" width="1300" height="360">

<!-- - Store all samples explicitly in two windows (stage 1)
- Define labels according to reference or current sample: $y = -1, \text{ if } x \in S_{-}(t)$  
$y = 1, \text{ if } x \in S_{+}(t)$
- Use that to train a model (stage 2)
- The test score then serves as a drift score (stage 3) which is commonly a normalized 
score (stage 4) -->

Note:
- If a classifier performs better than random guessing, then the class distribution must
 be different
- Usage of k-fold evaluation is advised for optimal data usage
- The used model class is crucial in terms of which drift can be detected and how much 
data are necessary
- The chance of choosing an invalid split point is essentially zero
- As a candidate of this class, we consider D3

--

#### <span style="color:gold">3. Statistical-test-based</span>

- <span style="color:gold">Kolmogorov-Smirnov Test</span>
<img src="figures/Statist_test.png" alt="Statistical-test-based" width="1300" height="360">
- <span style="color:gold">Kernel two-sample test</span>
  - Use raw data (Stage 1)
  - Descriptor is given by kernel matrix K (Stage 2)
  - Score is given by the Maximum Mean Discrepancy (Stage 3)
  - Permutation or bootstrap is used for normalization (Stage 4)

Note: 
- Classical statistical tests commonly focus on one-dimensional data
- Kernel two-sample test
  - It is based on the Maximum Mean Discrepancy (MMD)

--

# <span style="color:gold">Meta-statistics</span>
  - Combine values of several estimates to address issues such as the multiple testing 
  problem and sub-optimal sensitivity and get better results

--

# <span style="color:gold">Meta-Statistic-Based Drift Detectors</span>

- <span style="color:gold">1. AdWin (ADaptive WINdowing)</span>
<img src="figures/AdWin.png" alt="AdWin" width="1300" height="390">

Note: Stands for ADaptive WINdowing and is one of the most popular algorithms in 
supervised drift detection. It takes individual scores like model losses or p-values as 
input to estimate the actual change point

--

- <span style="color:gold">2. ShapeDD (Shape Drift Detector)</span>

Focuses on the discrepancy of two consecutive time windows

<img src="figures/ShapeDD.png" alt="ShapeDD" width="1300" height="390">


--

# <span style="color:gold">Block-based methods</span>
  - Do not assume a split of the data into two windows but analyze an entire data 
  segment at once

--

# <span style="color:gold">1. Independence-test-based</span>

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

# <span style="color:gold">2. Clustering-based</span>
  - Cluster time points into intervals such that the corresponding data points also form
   clusters, solving an optimization problem for a number of clusters

<!-- Notes:
The authors propose a general scheme with 4 stages: windowing, describing distributions,
 computing differences, and deciding if drift occurred.  
They also categorize existing methods into 4 types based on their strategy. -->

--

# <span style="color:gold">3. Model-based</span>

- Construct new kernels using machine learning models
- Moment Trees are used to construct such kernels

Note: 
- <span style="color:gold">Moment Trees</span>: Random Forests with a modified loss 
function that is designed for conditional density estimation
- This way, we obtain model-based block-based approaches that can be thought of as an 
extension of the virtual-classifier-based two-window approaches to continuous time by 
removing the time discretization

---

# <span style="color:gold">Analysis of Strategies</span>

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

# <span style="color:gold">Thank You!</span>



