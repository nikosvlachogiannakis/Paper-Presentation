# Models For Drift Detection

---

# 1. Meta-statistic based approaches

Does not consider each estimate separately but rather combines the values of several estimates to get better results

<!-- Meta-statistic methods don’t just compare two windows once; instead, they keep a running
 summary of the data as it evolves over time. This could be a statistic that accumulates
 evidence of change or adapts its window size depending on the stability of the stream. 
When the statistic crosses a threshold, it signals drift. These approaches are often 
efficient because they update incrementally rather than storing large chunks of past 
data. -->

--

# Shape Drift Detector

<img src="figures/ShapeDD_no_text.png" alt="ShapeDD Model">

Note: 

## Step 1 – Moving MMD

They compute the Maximum Mean Discrepancy (MMD) statistic across time points in a moving-window fashion.

The dotted line in the middle plot shows the expected shape of this statistic under drift; the blue curve is the actual computed MMD.

The red line marks the true drift point in the data stream.

## Step 2 – Shape Match

The obtained MMD curve is compared against the theoretically expected shape.

ShapeDD looks for locations where the match score changes from positive to negative, i.e., candidate points for drift.

In the figure, dots on the line indicate these candidate points.

## Step 3 – Candidate Points

These are the specific time indices where the MMD curve aligns with the expected drift shape.

The black horizontal line at 0 helps highlight where the sign changes, signaling possible drift boundaries.

## Step 4 – Permutation Test

At each candidate point, a permutation test is run with the MMD statistic to check if the difference between windows is statistically significant.

If the resulting p-value is below the chosen threshold (here p<0.001), ShapeDD declares that drift has been detected.

# Maximum Mean Descrepancy

Measure of the difference betweeen 2 probability distributions

<p>
  \( \text{MMD}(P, Q) = \sup_{f \in H} \left( \mathbb{E}_{X \sim P}[f(X)] - \mathbb{E}_{Y \sim Q}[f(Y)] \right) \)
</p>


--

# 2. Block-based approaches

An entire data segment is analyzed at once.

These methods often rely on statistical independence tests or clustering to check whether the new block of data is distributed differently than the old one.

Note: 

## What they suggest:
The paper recommends **Dynamic Adaptive Window Independence Drift Detection (DAWIDD)** 
and **Kernel Change-point Detection (KCpD)**

--

<!-- # DAWIDD Model -->

<img src="figures/Distribution-Agnostic Window Independence Drift Detection.png" alt="DAWIDD Model">


Note: 

2. The null hypothesis is: the two windows are drawn from the same distribution.
3. If the statistic is large, it suggests drift
4. Thresholf equal to 0.001

DAWIDD makes the fewest assumptions on the data or the drift. This allows for 
detecting more general drifts but comes at the cost of needing more data—a usual 
complexity-convergence trade-off

<!-- Uses independence tests between past and present blocks to formally check for 
distributional changes. It’s statistically valid and reliable, though it needs a 
reasonable amount of data in each block. -->

--

<!-- # KCpD Model -->

<img src="figures/KCpD.png" alt="KCpD Model">

Note:

It’s heuristic but very effective at finding multiple drift events and works well 
with complex, high-dimensional data

# Geometry of the distribution: This refers to the overall structure of how points are arranged in the feature space:

How many clusters there are,

Whether features are correlated or independent,

Whether the data form curved or rotated manifolds,

How dense or sparse certain regions are.

Higher-order dependencies: These are relationships beyond the first and second moments 
(mean, variance). For example:

Correlations between multiple features,

Nonlinear dependencies,

Shapes of clusters that only appear in multidimensional space.

--

### 3. Batch-based methods

<img src="/figures/Framework_batch_based_methods.png" alt="Framework Batch-based methods">

Note: 

Instance selection: label only the most important samples in a new batch after drift is 
suspected, instead of labeling everything

## How they work

- Collect data into batches (windows) → usually a reference batch (past data) and a 
detection batch (new data).

- Compare the distributions of the two batches (using statistics, confidence scores, or 
density).

- If the difference exceeds a threshold → drift is detected.

- Detection happens periodically (after a batch fills).

Most batch detectors are global-drift oriented, so they can miss local/regional changes.

The authors point out NN-DVI as stronger than others, because it explicitly detects 
regional/local drifts, something most batch approaches miss.

--

<!-- # NN-DVI -->

<img src="figures/NNDVI.png" alt="NN-DVI Model">

Note: 

Best at local/regional drift

## 1. Partition the data

Divide the data stream into a reference batch (past) and a detection batch (new).

## 2. Build local partitions

For each instance, use k-nearest neighbors to form small regions (“instance particles”).

## 3. Measure density variation

Compare the densities of these regions in the reference vs. detection batch.

## 4. Statistical testing

Use a statistical test to decide if the difference in densities is significant.

--

# 4. Online-based methods

<img src="/figures/Framework_online_based_methods.png" alt="Framework Online-based methods">

Note: 

## How they work

- Split into 2 windows:
  - Reference window
  - Detection window
- Compare distributions/uncertainty.
- If Difference > Threshold → Drift is Detected

Online methods are faster and better for gradual drifts but computationally heavy

## Best Methods

-  IKS-bdd → efficient, purely unsupervised and computationally light
- SAND & DSDD → promising because they self-annotate, reducing dependency on real labels

and Plover(best in theory, though experimental)

--

# Incremental Kolmogorov–Smirnov Drift Detector 

- Split into windows
  - Reference window (fixed)
  - Detection window (sliding)
- Apply incremental KS test
- Update KS statistic as new data arrives
- Drift Decision

Note:

efficient, purely unsupervised and computationally light

KS Test:  The Kolmogorov–Smirnov statistic quantifies a distance between the empirical distribution function of the sample

# Apply incremental Test

For each attribute, apply an incremental version of the KS test between the two windows.

# Update KS statistic

Without recalculating from scratch

# Drift decision

If the KS test rejects the null hypothesis for an attribute, declare drift.

## Update the model

When drift is signaled, update the classifier using the new data.