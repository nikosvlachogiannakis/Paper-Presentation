```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Concept Drift Survey Presentation</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js/dist/reveal.css">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js/dist/theme/white.css">
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <section>
        <h2>One or two things we know about concept drift</h2>
        <p>Fabian Hinder, Valerie Vaquet, Barbara Hammer</p>
        <p><em>Frontiers in AI, June 2024</em></p>
      </section>

      <section>
        <h2>Problem & Motivation</h2>
        <ul>
          <li>Automated systems operate in <strong>changing environments</strong>.</li>
          <li><strong>Concept drift</strong>: when data distribution changes over time.</li>
          <li>Prior work focuses on <strong>supervised detection</strong>.</li>
          <li>But monitoring often needs <strong>unsupervised detection</strong>.</li>
          <li>This paper surveys <strong>unsupervised methods</strong> for monitoring.</li>
        </ul>
      </section>

      <section>
        <h2>What the Paper Does</h2>
        <ul>
          <li>Formalizes & defines concept drift mathematically.</li>
          <li>Reviews & categorizes unsupervised drift detection methods.</li>
          <li>Proposes a general four-stage detection scheme.</li>
          <li>Compares methods on synthetic data.</li>
          <li>Provides practical guidelines.</li>
        </ul>
      </section>

      <section>
        <h2>Methods & Framework</h2>
        <ul>
          <li><strong>Four-stage detection scheme:</strong></li>
          <ol>
            <li>Data acquisition (windowing)</li>
            <li>Building descriptors</li>
            <li>Computing dissimilarity</li>
            <li>Normalization & decision</li>
          </ol>
          <li>Method categories:</li>
          <ul>
            <li>Two-sample tests</li>
            <li>Meta-statistics</li>
            <li>Block-based methods</li>
            <li>Clustering-based methods</li>
          </ul>
        </ul>
      </section>

      <section>
        <h2>Results & Takeaways</h2>
        <ul>
          <li>Formalizes drift as statistical dependence of data & time.</li>
          <li>Shows strengths & weaknesses of different methods.</li>
          <li>First survey focused on monitoring (unsupervised).</li>
          <li>Guidelines for real-world monitoring & anomaly detection.</li>
        </ul>
      </section>
    </div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/reveal.js/dist/reveal.js"></script>
  <script>
    Reveal.initialize();
  </script>
</body>
</html>

```

---

### 🦧 That is all 🦧



