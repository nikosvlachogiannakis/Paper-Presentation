# Models For Drift Detection

---

# Part A

--

# 1. Meta-statistic based approaches

## How it works:
Meta-statistic methods don’t just compare two windows once; instead, they keep a running summary of the data as it evolves over time. This could be a statistic that accumulates evidence of change or adapts its window size depending on the stability of the stream. When the statistic crosses a threshold, it signals drift. These approaches are often efficient because they update incrementally rather than storing large chunks of past data.

## What they suggest:
The paper highlights **ShapeDD** as a strong representative.

### How ShapeDD works: 
It monitors the shape of the data distribution by detecting changes in its structure (like correlation or dependency changes), rather than just shifts in means. It can pinpoint change points precisely, making it well-suited for tasks where accurate detection timing is important.

--

# 2. Block-based approaches

## How it works:
Block-based methods divide the stream into segments (“blocks”) and then compare these blocks to detect differences. Instead of tracking every incoming sample, they look at larger portions of data at once, which makes them robust to noise and particularly good at handling multiple drifts over time. These methods often rely on statistical independence tests or clustering to check whether the new block of data is distributed differently than the old one.

## What they suggest:
The paper recommends **DAWIDD** and **KCpD**.

### DAWIDD (Distribution-Agnostic Window Independence Drift Detection):
Uses independence tests between past and present blocks to formally check for distributional changes. It’s statistically valid and reliable, though it needs a reasonable amount of data in each block.

### KCpD (Kernel Change Point Detection):
Uses clustering and kernel methods to detect change points in blocks of data. It’s heuristic but very effective at finding multiple drift events and works well with complex, high-dimensional data.


---

# Paper

--

# 1. Batch-based methods

## How they work

- Collect data into batches (windows) → usually a reference batch (past data) and a detection batch (new data).

- Compare the distributions of the two batches (using statistics, confidence scores, or density).

- If the difference exceeds a threshold → drift detected.

- Detection happens periodically (after a batch fills).

Most batch detectors are global-drift oriented, so they can miss local/regional changes.

The authors point out NN-DVI as stronger than others, because it explicitly detects regional/local drifts, something most batch approaches miss.
They also note SQSI-IS as an improvement over SQSI because it reduces labeling needs, making it more efficient.

--

# 2. Online-based methods


## How they work

- Process the stream instance by instance.
- Maintain two windows:
  - Reference window (past data; fixed or sliding).
  - Detection window (most recent data; sliding).
- Continuously compare distributions/statistics/uncertainty.
- If difference > threshold → drift signaled immediately.

Online methods are faster and better for subtle/gradual drifts but heavier computationally.

## Best Methods

  -  IKS-bdd → efficient, purely unsupervised, and computationally light (incremental KS test) (best efficiency + unsupervised)
  - SAND & DSDD → promising because they self-annotate, reducing dependency on real labels (best adaptability, self-annotation)

and Plover(best in theory, though experimental)