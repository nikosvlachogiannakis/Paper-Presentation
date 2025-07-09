# One or two things we know about concept drift

**Fabian Hinder, Valerie Vaquet, Barbara Hammer**  
*Frontiers in AI, June 2024*

Notes:
Welcome the audience.  
This presentation summarizes a 2024 paper about detecting concept drift in evolving environments.  
Introduce the authors and mention it was published in Frontiers in AI.

---

# Problem & Motivation

- Automated systems operate in **changing environments**
- **Concept drift**: when data distribution changes over time
- Prior work focuses on **supervised detection**
- But monitoring often needs **unsupervised detection**
- This paper surveys **unsupervised methods** for monitoring

Notes:
Explain that in real-world systems, data distributions change — this is called concept drift.  
Most studies focus on supervised detection (where labels exist), but monitoring tasks (like anomaly detection) often lack labels.  
This paper addresses that gap by surveying unsupervised drift detection.

---

# What the Paper Does

- Formalizes & defines concept drift mathematically
- Reviews & categorizes unsupervised drift detection methods
- Proposes a general four-stage detection scheme
- Compares methods on synthetic data
- Provides practical guidelines for practitioners

Notes:
Summarize the contributions:
formal definitions, taxonomy of methods, a general detection framework, experiments, and guidelines for selecting methods.

---

# Methods & Framework

- **Four-stage detection scheme:**
  1. Data acquisition (windowing)
  2. Building descriptors
  3. Computing dissimilarity
  4. Normalization & decision
- **Method categories:**
  - Two-sample tests
  - Meta-statistics
  - Block-based methods
  - Clustering-based methods

Notes:
The authors propose a general scheme with 4 stages: windowing, describing distributions, computing differences, and deciding if drift occurred.  
They also categorize existing methods into 4 types based on their strategy.

---

# Results & Takeaways

- Drift is formalized as **statistical dependence between data & time**
- Different methods have strengths & weaknesses depending on the scenario
- First comprehensive survey focused on **unsupervised monitoring**
- Practical guidance for monitoring & anomaly detection in real-world systems

Notes:
Highlight the main takeaway: the paper clarifies the landscape of unsupervised drift detection for monitoring tasks.  
It shows no single method is best; choice depends on the problem specifics.  
Provides clear guidance for practitioners.

---

### 🦧 That is all 🦧



