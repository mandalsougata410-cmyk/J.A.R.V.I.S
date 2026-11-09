# J.A.R.V.I.S. — Feature Specification

**Full name:** Justified Analysis, Reasoning & Visualization of Incident Systems
**Goal:** A working web prototype that collects events from many sources, connects related alerts, finds the most likely root cause, shows the impact, explains its reasoning, suggests fixes, and **predicts the next failure before it happens**.

**Core message of the product:** *"247 separate alerts → 1 incident → 1 root cause → 1 fix."*

---

## 0. Instructions for the AI Builder

- Build a **frontend-only web app** (no backend needed). All logic runs in the browser with simulated data so the demo never fails.
- Suggested stack: **React + Vite + Tailwind CSS**, **React Flow** (dependency graph), **Recharts** (charts), **Framer Motion** (animations).
- Dark, modern "command center" theme. Clean, professional, fast.
- Make it responsive and demo-ready. Every feature below must be visible and clickable.
- Keep code simple and well organized: `/components`, `/engine` (correlation, root cause, prediction), `/data` (simulated logs, scenarios).

---

## 1. Demo Scenario (Built-in)

A **digital payment platform** with these services:

`Web App → API Gateway → Auth Service → Auth DB`
`API Gateway → Payment Service → Payment Gateway → Core DB`
`Auth Service → Core DB`
`Notification Service ← Payment Service`

**Story:** Core DB runs out of connections → Auth Service slows → Login failures → API responses get slow → Payment transactions fail → Notifications queue up.

The system must show these look like separate alerts but have **one root cause: Core DB connection pool exhaustion**.

---

## 2. Feature List

### F1. Multiple Data Sources
- **API Feed:** a connector panel that pulls events from a mock REST endpoint.
- **Log Upload:** drag-and-drop upload for `.json`, `.csv`, `.log`, `.txt` files.
- **Live Simulator:** a Start / Pause / Stop toggle that streams fake events in real time (with speed control).
- **Auto Log Normalizer:** every input is converted into one common schema:
  `{ id, timestamp, service, severity, type, message, user_id, trace_id, metric_value }`
- Show a "Sources" panel with event counts per source and a live ingestion counter.

### F2. Event Correlation
- **Service Dependency Graph:** interactive node graph of all services with arrows showing dependencies. Node color = health (green / yellow / red).
- **Multi-signal correlation score** between events, using:
  - time proximity
  - shared service or dependency link
  - similar error pattern
  - shared trace_id / user_id
- Show the score on every link (example: "92% related").
- **Alert Noise Collapse:** big counter animation like **"247 alerts → 1 incident"**.
- **Log Pattern Clustering:** group similar messages into patterns with counts.
- Grouped incident cards listing all linked events.

### F3. Root Cause Detection
- Ranked **Top 3 probable root causes** with confidence % (example: Core DB — 91%).
- **Graph-based tracing:** walk the dependency graph backward to the first failing service.
- **Earliest-anomaly rule:** the service that became abnormal first gets higher weight.
- **"Why this?" Explainability Panel** (very important):
  - exact logs that support the conclusion
  - rules/signals that fired
  - score breakdown (time, dependency, pattern)
  - this proves the "Justified Analysis & Reasoning" in the name.

### F4. Impact Analysis
- **Blast Radius View:** affected services glow red on the graph, healthy ones stay green.
- **Affected list:** services, applications, and estimated users affected.
- Metrics: failed logins, failed transactions, slow API count, estimated revenue at risk.
- **Severity score** (Low / Medium / High / Critical) with a single combined number.

### F5. Incident Timeline
- Vertical/horizontal timeline showing how the incident **started and spread** across services.
- **Animated Replay:** Play button that replays the incident from first symptom to full spread, with the graph updating in sync.
- **Time Scrubber:** drag a slider to see system state at any minute.
- **Cascade arrows** showing the failure moving from service to service.

### F6. Smart Recommendations
- **Fix playbook:** step-by-step actions for the detected root cause.
  Example: Increase DB connection pool → Restart Auth Service → Retry failed payments → Clear notification queue.
- Each step shows **priority**, **estimated time**, and **risk**.
- **Similar Past Incidents:** "Similar to Incident #12 — fixed by X".
- Copy-to-clipboard for commands/steps.

### F7. Interactive Dashboard
- **Overview page:** live system health map, active incidents, severity filter, event rate chart.
- **Incident list:** sortable by severity/time; click opens the investigation view.
- **Investigation view:** linked events, graph, root cause, reasoning, timeline, recommendations in one place.
- **Incident Report:** auto-generated summary (cause, impact, timeline, fix) with **Export as PDF**.
- **Plain-language summary:** a 3-line explanation anyone can understand.

---

## 3. Innovation Features (Differentiators)

### P1. Predictive Engine — "Future Incident Forecast" ⭐ (Main Innovation)
- **Early Warning System:** detect weak signals before failure (rising latency, growing error rate, climbing memory/connection usage, retry spikes).
- **"What happens next?" Forecast:** predicted failure chain with probabilities and ETA.
  Example: `Core DB connections 92% → Auth slowdown (~3 min) → Login failures (~5 min) → Payment failures (~10 min)`
- Each prediction shows **probability %**, **time-to-failure**, and **affected services**.
- **Pattern Memory:** if the current log sequence matches the start of a past incident, warn that it is likely to repeat.
- **Trend lines:** metric charts with a dotted projected line showing where the value is heading and when it crosses the danger threshold.
- **Preventive Actions:** suggested fixes for the predicted problem, not only the current one.
- **Forecast confidence meter** and a "Predicted" badge on future nodes in the graph (dashed orange outline).

### P2. What-If Simulator
- User selects any service and clicks **"Simulate failure"** (or "Simulate overload").
- System shows the predicted blast radius and expected user impact **before** anything breaks.

### P3. Incident Injection Panel (for live demo)
- Buttons: **Kill DB**, **Spike CPU**, **Break Payment Gateway**, **Network Delay**, **Memory Leak**.
- Clicking a button triggers a realistic chain of events so judges can watch J.A.R.V.I.S. react live.

### P4. Before / After Comparison View
- Left: "Traditional monitoring" — a messy list of separate alerts.
- Right: "J.A.R.V.I.S." — one grouped incident with root cause.
- Toggle button to switch between the two.

### P5. AI Chat Assistant
- Chat box that answers questions using the incident data:
  "Why did payments fail?", "Which services are affected?", "What will fail next?", "How do I fix it?"
- Answers must reference the actual events and root cause.

---

## 4. Pages / Screens

| Page | Contents |
|------|----------|
| **Dashboard** | Health map, active incidents, event stream, severity filter, alert-collapse counter |
| **Data Sources** | API feed, file upload, live simulator controls, normalized event table |
| **Incident Investigation** | Graph, correlation links, root cause + "Why this?", impact, timeline, recommendations |
| **Prediction Center** | Early warnings, forecast chain, trend charts, what-if simulator |
| **Reports** | Auto-generated incident report with PDF export |
| **Compare View** | Traditional alerts vs J.A.R.V.I.S. |

Persistent: left sidebar navigation, top bar with live status, floating AI chat button.

---

## 5. Engine Logic (Keep Simple, Rule-Based)

1. **Normalize** incoming events into the common schema.
2. **Group** events into time windows (example: 2-minute windows).
3. **Correlate** using the score:
   `score = 0.35 * time_proximity + 0.30 * dependency_link + 0.20 * pattern_similarity + 0.15 * shared_ids`
4. **Cluster** correlated events into one incident when score > 0.6.
5. **Root cause:** for each incident, rank services by
   `earliest anomaly time + number of downstream services affected + severity`.
   The top-ranked upstream service is the likely root cause.
6. **Impact:** traverse the dependency graph downward from the root cause to compute the blast radius.
7. **Prediction:** monitor metric trends (rate of change + threshold); if a metric will cross its threshold within N minutes, raise a forecast and propagate the predicted failure down the dependency graph with decreasing probability.
8. **Recommendations:** map root-cause type to a playbook (lookup table).

---

## 6. UI / UX Requirements

- Dark theme with neon accents: red = critical, orange = predicted, green = healthy, blue = info.
- Smooth animations for alert collapse, graph updates, and timeline replay.
- Every important number is large and readable.
- Tooltips on graph nodes (service, status, metrics).
- Loading skeletons and empty states.
- Keep the first screen impressive: show the **"247 alerts → 1 incident"** animation on load of the demo.

---

## 7. Demo Flow (Must Work End-to-End)

1. Open Dashboard — system is healthy.
2. Click **Start Live Simulator**.
3. Prediction Center shows an **early warning** (DB connections rising).
4. Click **Inject: Kill DB** — alerts flood in.
5. Alerts **collapse into one incident**.
6. Root cause shown: **Core DB (91%)** with the **"Why this?"** panel.
7. Blast radius highlights affected services and users.
8. Play the **timeline replay**.
9. Show **recommended fixes**.
10. Show **forecast of what would fail next** if not fixed.
11. Export the **incident report as PDF**.
12. Toggle **Before / After** view to close the demo.

---

## 8. Priority Order (If Time Is Short)

**Must have:** F2 (graph + alert collapse), F3 (root cause + Why this?), F4 (blast radius), F5 (timeline replay), P1 (prediction), P3 (inject buttons), F7 (report).
**Should have:** F1 (upload + simulator), F6 (playbook), P4 (before/after).
**Nice to have:** P2 (what-if), P5 (chat assistant).

---

## 9. Success Criteria

- Multiple alerts are clearly shown to be connected.
- One root cause is identified with confidence and **evidence**.
- Impact on services and users is visible.
- Incident spread is shown as a timeline with replay.
- Fix suggestions are specific and actionable.
- The system **predicts a future failure** with probability and ETA.
- A clean incident report can be generated.
