## Overview: from causal AI to a coordinated agent colony
> **Part of a larger architecture →** This causal-DAG engine is the reasoning 
> layer of **Crystalline AI** — a reliable-AI system design where the architecture 
> itself is a graph (a DAG of specialized agents), Operations Research sets the 
> optimization goal, a measured loop improves the system, and a human makes every 
> consequential decision. 
> **See the full design: https://github.com/daviddata-cloud/Crystalline-AI

========================================================================================================================================================
# Causal AI with DAGs — Discovery, Intervention, and Counterfactual Stress Testing

This repository demonstrates a small, reproducible causal-AI engine using synthetic data. It focuses on a question ordinary predictive AI cannot answer well:

> What caused the outcome, and what happens if we intervene?

The repo is the causal reasoning layer behind the broader Crystalline AI architecture. Crystalline AI handles multi-agent workflow design, graph-based triage, Operations Research prioritization, and executive dashboards. This repo focuses on the causal engine: DAG discovery, effect estimation, `do()` interventions, counterfactual propagation, and sensitivity analysis.

**Important scope:** All examples use synthetic or dummy data. The purpose is to demonstrate method, assumptions, and reproducibility — not to forecast real markets, make investment claims, or decide any real case.

---

## What This Repo Demonstrates

### 1. Causal Discovery

The pipeline learns directed, lagged relationships from synthetic time-series data using PCMCI-style causal discovery. Instead of assuming a graph manually, the system recovers candidate causal structure from the data and leaves uncertainty visible for expert review.

### 2. Structural Estimation

After discovery, each variable is modeled using its discovered parents. This produces a transparent structural model that can be inspected, challenged, and improved.

### 3. Intervention with `do()`

The engine applies synthetic interventions such as `do(X = x)` and propagates effects through the fitted graph. This turns the model into a counterfactual simulator rather than a passive prediction tool.

### 4. Sensitivity Analysis

The repo includes dose-response style stress tests that vary intervention severity from low to high and measure how downstream outcomes change. This is useful for risk, policy, oversight, and operational resilience questions.

---

## Why This Matters

Correlation can show that two signals move together. Causal AI asks a harder question: whether changing one variable would change another.

That distinction matters in regulated and policy environments. Analysts often need to know whether a signal is meaningful, whether a rule or shock may propagate, and where assumptions or confounders must be reviewed by human experts.

---

## Dynamic Dashboard Extension

The causal engine can feed executive-facing visualizations:

### Dose-Response Sensitivity Chart

Shows how downstream vulnerability changes as intervention severity increases. This helps reviewers identify potential safety thresholds and nonlinear failure points.

### Vulnerability Tornado Chart

Ranks nodes by systemic importance using graph metrics such as betweenness centrality. This helps prioritize which processes or variables should be reviewed first.

### Intervention ROI Waterfall

Shows stepwise reduction in systemic risk after selected interventions. This translates causal analysis into budget and policy language.

### Cross-Domain Contagion Heatmap

Aggregates causal/risk transmission across workflow domains to reveal hidden cross-functional exposure.

---

## Responsible Use

This repo is a proof-of-concept reference pattern. It is not legal, investment, enforcement, medical, or policy advice. Causal discovery depends on assumptions such as variable selection, time lag design, missing-data handling, and possible unmeasured confounding. Where the method cannot decide, the uncertainty should remain visible for human review.




## Details===================================================================================================================================================================================

**1. Causal AI (the foundation).** Classic causal inference — Directed Acyclic 
Graphs (DAGs), the do() operator, counterfactuals — answers the question 
prediction can't: not just *what* will happen, but *what causes it* and *what 
happens if we intervene*. This is well-established (Pearl, PCMCI, DoWhy) and 
it is the reasoning that policy, risk, and oversight actually need.

**2. Today's AI (one large model).** Most current systems lean on a single 
large LLM — powerful, but a "lone wolf": it does everything, is expensive to 
run for every task, and is hard to audit step by step.

**3. The next step (a coordinated agent colony).** Inspired by how ant colonies 
and wolf packs organize *specialized roles* for efficiency, this approach uses 
a **team** of focused agents — each a small, fine-tuned model doing one job — 
coordinated by a management agent, with a human at the final gate. The graph 
*describes* relationships, Operations Research *optimizes* how scarce expert 
time is spent, and causal AI *tests* whether a relationship is credible. 
Together, small specialized agents are more efficient and more auditable than 
one large black box.

<img width="603" height="772" alt="image" src="https://github.com/user-attachments/assets/efe0269e-7b38-40c0-bc3f-482b0bde1027" />

Each agent is small and specialized; the structured hand-off between them stays 
auditable (sources, assumptions, confidence, limitations). Graph = understanding, 
Operations Research = efficiency, causal AI = credibility, human = accountability.

*All demos use synthetic/dummy data only — they illustrate the method, not any 
real matter. The system flags candidates for human review; it does not decide 
outcomes.*

[make_pandemic_whatif_dag.py](https://github.com/user-attachments/files/29167170/make_pandemic_whatif_dag.py)# causal-ai-dag
Causal AI with DAGs — discovery, intervention, and counterfactual what-if simulation (PCMCI, do-calculus). Note: dummy/public/synthetic data only in this project.

# Causal AI: discovery, intervention, and counterfactual what-ifs

> AI can predict almost anything now. Trusting it is the hard part.
> This repo is a small, end-to-end **causal AI** pipeline — using Directed
> Acyclic Graphs (DAGs) — that answers the question prediction can't: not
> just *what* will happen, but *what causes it*, and *what happens if we
> intervene*.

<img width="1635" height="1184" alt="causal_engine_domains" src="https://github.com/user-attachments/assets/50bbf841-2ce4-4646-8574-7776b7ecdb68" />


## What this is

Two reproducible demonstrations, both running the same causal engine on
**synthetic data**:

**Macro — a pandemic stress test.** "If another COVID-style shock hit, what
happens to the markets?" The pipeline discovers the causal structure from a
time series, then injects the shock and propagates it through the discovered
graph: pandemic severity drives the real economy and market fear, flowing
through liquidity and credit spreads into equity prices. The output is a
counterfactual what-if — as severity rises, equity falls and credit spreads
widen, together.

**Micro — market manipulation.** A pipeline that isolates whether a
spoofing-type trading signal actually *caused* a price move, or merely rode a
market-wide swing, by running a `do()` intervention that removes the
confounding factor.

## The method

Both demos use the same three steps:

1. **Discovery** — the causal structure is learned directly from time-series
   data with PCMCI (Tigramite), not assumed. Time is unrolled (cause at *t−1*
   → effect at *t*), so feedback is handled and the graph stays acyclic.
2. **Estimation** — each variable is regressed on its discovered parents, so
   the coefficients come from the data.
3. **Intervention** — a `do()` operation forces a variable and propagates the
   effect through the fitted structural model, yielding the counterfactual.

This is **causal AI**, not generative AI: established machine-learning methods
(PCMCI, NOTEARS, DoWhy) that reason about cause and intervention rather than
predicting the next token.

## Why causal, not just predictive

Correlation can't answer interventional questions. A model can show that a
signal and an outcome move together without knowing whether one *caused* the
other, or whether both were driven by a hidden third factor. DAGs and the
`do()` operator are built exactly for this — and it's the reasoning that
policy, enforcement, and risk analysis actually need: stress-testing a shock,
or a rule, *before* it happens.

## The same engine, many domains

The structure here — causal propagation on a directed graph — recurs across
fields: contagion in **public health**, intervention effects in **tax and
fiscal policy**, fund-flow in **elections and campaign finance**, and shock
propagation in **physics**. Different domains, identical mathematical
structure. See the overview diagram above.

## Results

**Pandemic what-if** — equity falls and credit spreads widen as severity rises:
<img width="584" height="448" alt="image" src="https://github.com/user-attachments/assets/98ca6ae8-0e40-4bbd-9041-c798777f9cd2" />

<img width="1483" height="881" alt="pandemic_whatif_chart" src="https://github.com/user-attachments/assets/800f9b1e-502b-4b83-a170-c560c0ca636a" />

**Discovered causal graph** — PCMCI recovers the structure directly from data:
<img width="961" height="687" alt="image" src="https://github.com/user-attachments/assets/cd8cadcb-3ff2-4ca5-8845-f049a002adc3" />

<img width="1380" height="1062" alt="pcmci_clean_graph" src="https://github.com/user-attachments/assets/ddbcf9f9-78e1-4562-bace-8dd96b3d886b" />


<img width="1935" height="1259" alt="pandemic_whatif_dag" src="https://github.com/user-attachments/assets/0eb1e5a5-b6fd-49dd-989b-067d2eb4fe4a" />

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/87dc2b6f-4c9d-4630-b21e-c87327035419" />

## Run it

```bash
pip install -r requirements.txt

# Macro: pandemic stress test (generates data, discovers, runs what-if, charts)
python pandemic_pipeline.py pandemic_markets.csv

# Micro: market-manipulation discovery + intervention
python pcmci_discovery.py market_timeseries.csv

# Diagrams
python make_domains_chart.py
```

(If `pandemic_markets.csv` doesn't exist yet, run `python make_pandemic_data.py`
first.)

## Important caveats

- **All data is synthetic, by design.** It is *not* real market or health
  data. It is generated with a known causal structure so the method can be
  validated against a ground truth. The purpose is to demonstrate the
  **method**, not to make any claim about real markets or to forecast.
- **The pandemic series are trending**, so the estimated magnitudes are
  illustrative — the recovered *structure and direction* are the solid claim.
  A production version would difference the series and estimate from real data.
- **This is an educational proof of concept, not a deployed tool.** Not
  investment advice, not enforcement guidance, not production software.
- Causal discovery rests on assumptions (e.g. no unmeasured confounders) that
  do not hold automatically in real data. Where the algorithm cannot orient an
  edge from data alone, that ambiguity is left visible rather than hidden.


## Extension: Graph Detection + Operations Research (reviewer triage)

Beyond causal discovery, the same analytical thinking extends to **prioritizing limited expert review time**. A small companion demo (`or_with_charts.py`, synthetic data) shows the full chain:

1. **Graph detection rules** flag relationship-based issues a flat table can't easily catch:
   - **Peer deviation** — a company's value is far from its industry-peer cluster
   - **Shared-auditor cluster** — many companies share one auditor (worth watching)
   - **Cross-filing mismatch** — the same value reported differently across connected filings

2. **Operations Research (0/1 knapsack)** then picks *which* flagged items a human should review first, maximizing total risk caught within a limited review budget (analyst-hours).

3. A **human reviews** the flagged candidates and makes the final decision.

**Why it matters:** Reviewers can't check everything. The graph *understands* relationships; Operations Research *optimizes* how scarce expert time is spent; the human decides. For regulatory and risk work — including structured disclosure and market oversight — this directs limited expert attention to the highest-risk cases, with every step auditable.

The two figures below show the full chain on synthetic data: graph-based rules find relationship issues (Figure 1), and Operations Research optimizes which flagged items a human reviews first within a limited time budget (Figure 2).
<img width="938" height="700" alt="image" src="https://github.com/user-attachments/assets/6101eda7-8895-460a-ac7d-12560ef16352" />
Figure 1. Relationship graph for detection. Companies (blue) are linked to their auditors. Graph-based rules flag candidates for review: CIRRUS (red) is a peer outlier — its margin deviates sharply from its industry peers; AudX (amber) is a shared-auditor cluster — it audits three companies; the red dashed line marks a cross-filing mismatch between DELTA and ECHO, where the same item is reported as two different values. Synthetic data only.



<img width="1126" height="704" alt="image" src="https://github.com/user-attachments/assets/ba133039-11be-4ebe-b73c-00c5f4026d0a" />
Figure 2. Optimizing limited review time. Given each flag's estimated risk and review cost (analyst-hours) and a fixed review budget (7 hours), an Operations Research optimization (0/1 knapsack) selects the subset that maximizes total risk caught. Green bars are selected for immediate review (7 hours used, risk 10.9 caught); the gray bar is deferred. A human reviews the selected candidates and makes the final decision. Synthetic data only.


**Run it:**

## 📊 Enterprise Risk & Executive Dashboards

While core Causal AI focuses on topological discovery (PCMCI) and mathematical interventions (do-calculus), translating these findings into actionable business intelligence is critical. We have developed a suite of dynamic, real-time executive dashboards that run directly on top of the generated DAG. 

These visualizations transform raw mathematical probabilities into systemic risk forecasts and ROI calculations for leadership teams.

### 1. Dose-Response Sensitivity Analysis
**What it is:** A continuous counterfactual stress test. It visualizes the total systemic collapse (blue bars) and target outcome failure probability (red line) as the severity of a root-cause node increases.
*   **The Causal Math:** Instead of a binary `do(X=0)` or `do(X=1)`, this simulates a continuous spectrum of interventions `do(X=x)` where `x` scales from 1% to 100% severity, measuring the downstream ripple effect via descendant propagation.
<img width="953" height="617" alt="image" src="https://github.com/user-attachments/assets/0a897064-f1d8-4430-a04f-4b60e09a0e32" />

*   **Business Value:** Allows auditors and risk managers to set strict, mathematically proven "safety thresholds" before a process reaches a critical breaking point.

### 1. Vulnerability Tornado Chart (Critical Bottlenecks)
**What it is:** A ranked horizontal bar chart identifying the exact nodes that act as the biggest systemic liabilities.
*   **The Causal Math:** Calculated using **Betweenness Centrality** across the causal DAG. It identifies the specific variables that serve as the primary bridges for risk contagion.
*   <img width="885" height="529" alt="image" src="https://github.com/user-attachments/assets/3b53db55-5a12-4d40-b983-f259506c346b" />

*   **Business Value:** Answers the question: *"If we only have budget to audit or upgrade 5 processes this year, which exact 5 provide the highest systemic protection?"*

### 2. Intervention ROI Waterfall
**What it is:** A step-by-step financial/risk visualization showing how systemic risk decreases as specific interventions are applied.
*   **The Causal Math:** Visualizes sequential counterfactual subtraction: `Baseline Risk - E[Y | do(Fix_1)] - E[Y | do(Fix_2)]`. 
<img width="783" height="494" alt="image" src="https://github.com/user-attachments/assets/750b6dfa-88fa-4579-8b71-421ddfe22ddb" />

*   **Business Value:** Proves the **Return on Investment (ROI)** of policy changes. It demonstrates to stakeholders exactly how much systemic risk is mitigated by funding specific upgrades.

### 3. Cross-Domain Contagion Heatmap
**What it is:** A macro-level matrix showing the probability of failure spreading from one department (e.g., IT Security) to another (e.g., Lab Operations).
*   **The Causal Math:** Aggregates individual edge weights and probabilities across grouped node clusters (Markov Blankets) to show inter-departmental edge density.
*   <img width="622" height="608" alt="image" src="https://github.com/user-attachments/assets/7d5d75d0-1bb6-45c2-afd5-ae57f3fd2bac" />

*   **Business Value:** Identifies hidden blind spots where a failure in an administrative silo silently threatens operational execution in an entirely different domain.

## Methods / references

- PCMCI — Runge et al., causal discovery for time series (Tigramite).
- The `do()` operator and DAG-based causal inference — Pearl.
- NOTEARS, DoWhy — related structure-learning and effect-estimation tooling.

## License

MIT
