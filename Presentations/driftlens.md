# DriftLens
## Real-time unsupervised Concept Drift detection by evaluating embedding distributions

---

# Concept Drift

The phenomenon in which the underlying data distributions and statistical 
properties of a target data domain change over time.

$$P_{[0,t]} (X, y) \neq P_{t+w} (X, y)$$

where:

- [0, t]: time period
- t + w: time window

**Due to the lack of actual labels, the only possible drift that can be considered is the change in the prior probability of the features P(X)**

Note:

A change in the joint distribution between a time period $[0, t]$ and a time 
window $t + w$

This work focuses on detecting drift in scenarios where no ground truth labels 
are available for new data. Therefore, drift must be detected in an 
unsupervised manner. Due to the lack of actual labels, the only possible 
drift that can be considered is the change in the prior probability of the 
features P (X)

# Application Scenario

- Unavailability of ground truth labels for new incoming
data

- High data complexity and dimensionality

---

# Framework

<img src="figures/driftlens/framework.png">

Note:

1. Build baseline

Extract embeddings → reduce with PCA → fit Gaussian distributions (per-batch + per-label).

2. Set thresholds

Sample windows from clean data → compute Frechét distances → choose max non-outlier distances as thresholds.

3. Monitor stream

For each new window: extract embeddings → apply PCA → fit Gaussians.

4. Compute drift score

Calculate Frechét distance between window Gaussians and baseline (per-batch + per-label).

5. Detect drift

If distance > threshold → drift flagged (and which labels are affected).

**Per-Batch Distance:** Detects if the entire window is affected by drift

**Per-Label Distance:** Characterizes drift and identifies the labels most affected

--

# Offline Phase (Executed only once)

1. **Baseline Modeling:** Estimates the probability distribution of a reference 
(**historical**) dataset representing the concepts learned by the model during 
training (**baseline**)

<img src="figures/driftlens/data_modeling.png" style="width:80%;">

<span style="color: red;">Assumption:</span> Embeddings follow multivariate normal distribution

Note:

#### Baseline Estimation

1. Input historical data $X_b$ with $m_b$ samples.

2. Pass it through the deep learning model $\rightarrow$ get embedding vectors $E_b$
 and predicted labels $\hat{y}_b$

3. Apply PCA (Principal Component Analysis):

  - Once on the whole batch of embeddings

  - Separately for each label group

4. Reduce the embeddings $\rightarrow$ get smaller matrices $E_b^{'}$ (per-batch) and $E_{b,l}^{'}$ (per-label)

5. Estimate multivariate normal distributions:

  - Per-batch distribution: mean vector $\mu_b$ and covariance matrix $\Sigma_b$

  - Per-label distributions: mean vectors $\mu_{b,l}$ and covariance matrices $\Sigma_{b,l}$

--

2. **Threshold Estimation**

Estimate distance thresholds to distinguish between normal and drift distances, 
which will be used in the online phase

- Sample in random $n$ windows from the dataset
  - For each window performs data modeling and computes the per-batch and 
  per-label **Frechét Drift Distances** from the baseline distribution

$$F_{DD}(b, w) = \||\mu_b - \mu_w\||_2^2 + \operatorname{Tr}(\Sigma_b + \Sigma_w - 2 \Sigma_b \Sigma_w)$$

- Sort the distances in **descending order**

- The first element contains the **maximum distance** that a window considered without drift can achieve

#### Handle outliers

- Use $T_a \in [0,1]$

- Removes $T_a$\% left tail of the sorted distancies to remove outliers

Note:

- Estimates the maximum distance a window without drift can reach

- n distances are computed

**Higher $T_a$** → Lower Threshold & Higher Sensitivity to possible drift and 
false alarms

--

# Online Phase

**3. Window Modeling**

- Extract the embedding $E_w = \phi$ and predicted labels

- Perform Embedding dimensionality reduction (PCA) and get $E_w^{'}$ and $E_{w,l}^{'}$

- Estimate the per-batch and per-label multivariate normal distributions
(i.e. find mean vectors and covariance matrices)

Note:

Continuously detects  drift in the monitored model when applied to a datastream

**Splits data in fixed size windows**

DriftLens performs an embedding dimensionality reduction applying a principal 
component analysis (PCA)

- PCA was fitted in the **offline phase** and are reused in this step

--

**4. Distribution Distances Computation**

Calculate the per-batch and per-label **Frechét Drift Distance (FDD)** distances between the window and the baseline distribution

**5. Distribution Distances Evaluation**

If the distances exceed the threshod, **DRIFT is predicted**

Note:

4. Given a multivariate reference normal distribution $b$ (e.g., the baseline 
computed in the offline phase) characterized by a mean vector $\mu_b$ and a 
covariance matrix $\Sigma_b$ , and the multivariate normal distribution of a new
 data window $w$, characterized by $\mu_w$ and $\Sigma_w$

**Per-Batch Distance:** Detects if the entire window is affected by drift

**Per-Label Distance:** Characterizes drift and identifies the labels most affected

--

# Drift Monitor Example

<img src="figures/driftlens/drift_monitor_example.png">

Note:

**Per-Batch Distance:** Detects if the entire window is affected by drift

**Per-Label Distance:** Characterizes drift and identifies the labels most affected

---

# Evaluation Metrics

--

### Drift Detection Accuracy

The ground truth label is assigned as:
- **Yes (1)** → if *any* sample in the window comes from drifted data  
- **No (0)** → if the window is completely clean (0% drift)

DriftLens also outputs a prediction:
- **Yes (drift detected)**  
- **No (no drift detected)**

Accuracy measures how often DriftLens matches the ground truth.

For each drift level **0%, 5%, 10%, 15%, 20%**:
1. They create **100 windows** containing exactly that percentage of drift.  
2. DriftLens predicts “drift” or “no drift” for each window.  
3. $Accuracy = \frac{\text{number of correct predictions}}{100}$
4. They repeat the process **5 times** and take the **average accuracy**.

<img src="figures/driftlens/Adrift.png" style="width:50%;">

Note:

This results in accuracy values:
- **A₀%** (no-drift accuracy → false alarm control)  
- **A₅%, A₁₀%, A₁₅%, A₂₀%** (drift detection accuracy at different severities)

--

### HDD — Harmonic Drift Detection Score

- Gives one number that summarizes performance

- Forces detector to be good at BOTH:

  - avoiding false alarms

  - detecting real drift

- It is used to identify the best drift detector overall

<img src="figures/driftlens/hdd.png" style="width:40%;">


Note:

DriftLens is tested on synthetic drift, meaning the authors create the drift 
themselves during evaluation.

---

# Thank You!