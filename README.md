# J.A.R.V.I.S.
### Justified Analysis, Reasoning & Visualization of Incident Systems

> *"Other tools show you 247 alerts. J.A.R.V.I.S. shows you the 1 cause, the 1 fix, and the next failure before it happens."*

[![Live Demo](https://img.shields.io/badge/Demo-Live%20on%20Vercel-00F2FE?style=for-the-badge&logo=vercel)](https://jarvis-eta-khaki.vercel.app)
[![Hackathon](https://img.shields.io/badge/Hackathon-NITSHACKS%209.0-blueviolet?style=for-the-badge)](https://github.com/mandalsougata410-cmyk/J.A.R.V.I.S)
[![Team](https://img.shields.io/badge/Team-BITCODE-orange?style=for-the-badge)](https://github.com/mandalsougata410-cmyk)

---

## 🌐 Live Prototype
Explore the interactive command center:  
👉 **[https://jarvis-eta-khaki.vercel.app](https://jarvis-eta-khaki.vercel.app)**

---

## 🎯 The Problem
In modern distributed microservice architectures, when a single upstream component degrades, it triggers a cascade of alerts across downstream services. 
* **Alert Fatigue:** On-call engineers are inundated with hundreds of noisy, disconnected notifications.
* **Delayed MTTR:** Crucial minutes are lost guessing the real root cause among dozens of red dashboards.
* **Blind Cascades:** Teams react to symptoms that have already failed instead of preventing the next failure in the dependency chain.

---

## 💡 The Solution
**J.A.R.V.I.S.** is an AI-powered, HUD-inspired incident command system designed to collapse alert storms, provide transparent, explainable reasoning for failures, and forecast cascades before they impact users.

### Built-in Cascade Simulation
**Scenario:** Digital Payment Infrastructure
```
Web App ──► API Gateway ──► Auth Service ──► Auth DB
                 │                │
                 ▼                ▼
         Payment Service ──► Core DB (Connection Pool Exhaustion)
                 │
                 ▼
         Payment Gateway
                 │
                 ▼
        Notification Service
```
**Progression:**
1. `Core DB` exhausts connection pool (Root Cause).
2. `Auth Service` latencies spike → login failures cascade to users.
3. `API Gateway` throttles transactions.
4. `Payment Service` drops requests → downstream `Notification Service` queue backlogs.
5. **J.A.R.V.I.S. Result:** 247 disparate alerts collapsed into **1 Incident**, isolating **Core DB** with **91%+ confidence** and an actionable remediation playbook.

---

## ⚡ Core Features & Differentiators

### 1. In-Memory Temporal Knowledge Graph (No DB Bottleneck)
* Eliminates SQL/NoSQL storage latency during active incidents.
* Services, events, and metrics are modeled dynamically as nodes and weighted causal edges in an append-only in-memory event stream.
* Enables zero-overhead **time-travel replay** and state reconstruction.

### 2. Multi-Signal Alert Noise Collapse
* **247 alerts → 1 actionable incident.**
* Multi-factor correlation engine evaluating temporal proximity, graph topology traversal, error pattern clustering, and shared trace identifiers.

### 3. Predictive Failure Cone (Future Incident Forecast)
* Early-warning system scanning subtle leading indicators (climbing connection saturation, latency trends, retry spikes).
* Projects failure propagation with estimated time-to-impact (ETA countdown) before downstream systems breach SLA thresholds.

### 4. Counterfactual "Rewind & Fix" Simulator
* Interactive digital twin allowing SREs to rewind the incident and test proposed remediation playbooks in a safe sandbox prior to production execution.

### 5. Explainable Reasoning & Evidence Cards
* Complete transparency ("Why this cause?").
* Highlights exact log fingerprints, rule triggers, confidence score breakdowns, and explicit falsification criteria ("What would change this conclusion?").

### 6. Incident DNA Fingerprinting
* Synthesizes incident attributes into a visual signature pattern to identify recurrences and retrieve historical resolutions.

---

## 🚀 Getting Started

### Run Locally

1. **Clone the repository:**
   ```bash
   git clone https://github.com/mandalsougata410-cmyk/J.A.R.V.I.S.git
   cd J.A.R.V.I.S
   ```

2. **Launch with any local web server:**
   ```bash
   # Using npx serve
   npx serve .

   # Or using Python
   python -m http.server 3000
   ```

3. Open `http://localhost:3000` in your web browser.

---

## 📁 Repository Structure
```
├── assets/
│   ├── index-BqStd347.js     # Bundled application logic & visualization engine
│   └── index-CqwIGiFX.css    # Command center HUD styling & theme
├── docs/
│   ├── JARVIS_Features.md    # Detailed feature specification & user stories
│   └── JARVIS_Innovation_and_Winning_Touches.md # Data architecture & innovations
├── index.html                # Main application entry point
├── package.json              # Project metadata and run scripts
├── vercel.json               # Vercel deployment routing configuration
└── README.md                 # Project documentation
```

---

## 🏆 Hackathon Details
* **Event:** NITSHACKS 9.0
* **Team:** BITCODE
* **Author:** [Sougata Mandal](https://github.com/mandalsougata410-cmyk)
* **Live Deployment:** [https://jarvis-eta-khaki.vercel.app](https://jarvis-eta-khaki.vercel.app)
