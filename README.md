# causal-ai-dag

**Causal AI with DAGs — discovery, intervention, and counterfactual stress testing (PCMCI, do-calculus).**
*Synthetic / dummy data only.*

> **Part of a larger architecture →** This causal-DAG engine is the reasoning layer of **Crystalline AI** — a reliable-AI system design where the architecture itself is a graph (a DAG of specialized agents), Operations Research sets the optimization goal, a measured loop improves the system, and a human makes every consequential decision. **Full design: https://github.com/daviddata-cloud/Crystalline-AI**

---

## What this repo is — in one idea

**Causal AI uses a Directed Acyclic Graph (DAG) to show direction** — not just that two things are related, but *which way the influence flows*, how source variables connect through a relationship network to an outcome, and what happens if you intervene. That direction is what gives the analysis **accuracy**: it separates a real cause from a coincidence, and it makes every assumption visible for human review.

This repo is the **causal reasoning layer** behind the broader [Crystalline AI](https://github.com/daviddata-cloud/Crystalline-AI) architecture.

*All examples use synthetic or dummy data — to demonstrate the method, not to forecast real markets or decide any real case. The system flags candidates for human review; it does not decide outcomes.*

---

## The core problem this solves

**Problem:** Most AI predicts — given the past, what is likely next. But prediction only shows *correlation*: two signals move together. It cannot tell you whether one *caused* the other, or whether a hidden third factor drove both. In regulated, policy, and risk work, acting on correlation can justify the wrong decision.

**Solution:** A causal DAG makes **direction and structure explicit** — which source variables influence which outcomes, through what relationship paths, after accounting for confounders. The `do()` operator then tests "what if we intervene here?"

**Impact:** Analysts can tell a real signal from a coincidence, see how a shock or rule would propagate, and challenge the assumptions *before* a decision — which is exactly what accuracy means in high-stakes work.

---

## What it does — Problem / Solution / Impact

### 1. Causal discovery — find the direction from data

<!-- IMAGE: PCMCI discovered causal graph -->
<img width="904" height="697" alt="image" src="https://github.com/user-attachments/assets/c1abaa58-ae3f-4da7-8290-619892ad074d" />


*Figure 1. Discovered causal graph (synthetic data). PCMCI recovers directed, lagged relationships directly from the time series. Where an edge can't be oriented from data alone, the ambiguity stays visible for human review.*

- **Problem:** In complex systems, variables influence each other over time, but correlation doesn't show *direction*, *lag*, or the *relationship path*.
- **Solution:** PCMCI (Tigramite) learns the causal structure directly from time-series data. Time is unrolled (cause at *t−1* → effect at *t*), so the network of source-to-outcome relationships stays directed and acyclic.
- **Impact:** The system shows which variables lead and which follow — a data-driven map of the relationship network, not a hand-drawn guess.

### 2. Structural estimation — quantify the relationships

- **Problem:** Knowing the structure isn't enough; you need the strength of each link.
- **Solution:** Each variable is regressed on its discovered parents, so the edge weights come from the data.
- **Impact:** A transparent model that can be inspected, challenged, and improved — accuracy you can audit.

### 3. Intervention with `do()` — test cause, not coincidence

<!-- IMAGE: causal DAG what-if -->
![Causal DAG what-if simulation](causal_dag_whatif.png)
*Figure 2. "What-if" intervention (synthetic data). Forcing `do(node = value)` propagates the effect through the directed network, producing a collateral-impact report — showing exactly which downstream nodes are affected and which are not.*

- **Problem:** Stakeholders ask "what happens if we change this policy?" — a question prediction can't answer.
- **Solution:** A `do()` intervention forces a variable and propagates the effect along the DAG's directed edges to every descendant.
- **Impact:** Turns a passive model into a counterfactual simulator — you see the ripple of a change before it happens.

### 4. Counterfactual stress testing — find the breaking point

- **Problem:** Teams need to know how much stress a system can take before failure cascades.
- **Solution:** Dose-response testing scales an intervention `do(X=x)` from low to high severity and measures the downstream ripple through the network.
- **Impact:** Reviewers can set mathematically grounded safety thresholds before a process reaches a critical point.

---

## The two demonstrations

Both run the **same causal engine** on synthetic data.

<!-- IMAGE: pandemic what-if result chart -->
<img width="864" height="696" alt="image" src="https://github.com/user-attachments/assets/3002d6d1-e7cc-459d-b3a8-d94c99b533fe" />

*Figure 3. Counterfactual what-if (synthetic data). As modeled pandemic severity rises, equity falls and credit spreads widen together — the result of propagating a `do()` shock through the discovered causal network.*

- **Macro — pandemic stress test.** **Problem:** "If another COVID-style shock hit, what happens to markets?" **Solution:** discover the causal structure, inject the shock, propagate it (severity → real economy and market fear → liquidity and credit spreads → equity). **Impact:** a clear counterfactual showing how the shock moves through the relationship network.
- **Micro — market manipulation.** **Problem:** did a spoofing-type signal *cause* a price move, or just ride a market-wide swing? **Solution:** a `do()` intervention removes the confounding factor. **Impact:** isolates true cause from coincidence — the accuracy a regulator needs.

---

## Run it

```bash
pip install -r requirements.txt

# Macro: pandemic stress test
python pandemic_pipeline.py pandemic_markets.csv

# Micro: market-manipulation discovery + intervention
python pcmci_discovery.py market_timeseries.csv

# Diagrams
python make_domains_chart.py
```
*(If `pandemic_markets.csv` doesn't exist yet, run `python make_pandemic_data.py` first.)*

---

## Extension: Graph Detection + Operations Research (reviewer triage)

- **Problem:** Reviewers can't check everything, and risk often hides in *relationships* — not in one row.
- **Solution:** Graph detection rules flag relationship-based issues (peer deviation, shared-auditor cluster, cross-filing mismatch); then Operations Research (0/1 knapsack) picks which flagged items a human reviews first, maximizing risk caught within a limited time budget.
- **Impact:** The graph understands relationships, OR optimizes scarce expert time, and the human decides — limited attention goes to the highest-risk cases, fully auditable.

<!-- IMAGE: graph detection -->
![Graph detection — flagged relationships](or_graph_detection.png)

*Figure 4. Relationship graph for detection (synthetic data). Rules flag a peer outlier (red), a shared-auditor cluster (amber), and a cross-filing mismatch (red dashed) — relationship problems a flat table would miss.*

<!-- IMAGE: OR triage bar chart -->
![Operations Research review triage](or_triage.png)
*Figure 5. Optimizing limited review time (synthetic data). A 0/1-knapsack optimization selects the flagged items that maximize total risk caught within a fixed review budget. Green = review now; gray = deferred. A human makes the final call.*

```bash
pip install networkx matplotlib
python or_with_charts.py
```

---

## Enterprise risk & executive dashboards

The causal engine feeds executive-facing visualizations that translate the directed-graph math into business intelligence. *(All figures illustrative on synthetic data.)*

### Dose-Response Sensitivity
- **Problem:** Leaders need to know how vulnerability grows as a root cause worsens.
- **Solution:** Simulate `do(X=x)` from 1% to 100% severity and measure the downstream ripple via descendant propagation.
- **Impact:** Set mathematically grounded safety thresholds before a breaking point.

<!-- IMAGE: dose-response -->
<img width="1021" height="553" alt="image" src="https://github.com/user-attachments/assets/4d137425-7f5f-46a9-a6a3-5b9eea8744c6" />

*Figure 6. Dose-Response Sensitivity (synthetic data). Systemic impact as a root-cause node's severity scales from 1% to 100%.*

### Vulnerability Tornado Chart
- **Problem:** Which few processes, if fixed, give the most systemic protection?
- **Solution:** Rank nodes by **betweenness centrality** across the DAG — the primary bridges for risk to travel along the network.
- **Impact:** Direct limited audit budget to the highest-leverage bottlenecks.

<!-- IMAGE: tornado -->
<img width="820" height="874" alt="image" src="https://github.com/user-attachments/assets/bcfd3b95-0f16-41dd-9663-303f72c5cb6b" />


*Figure 7. Vulnerability Tornado (synthetic data). Nodes ranked by systemic bridging risk (betweenness centrality).*

### Intervention ROI Waterfall
- **Problem:** Stakeholders need to see the payoff of fixing specific things.
- **Solution:** Sequential counterfactual subtraction — Baseline − E[Y | do(Fix₁)] − E[Y | do(Fix₂)].
- **Impact:** Translates causal analysis into budget and policy language — how much risk each fix removes.

<!-- IMAGE: waterfall -->
![Intervention ROI Waterfall](roi_waterfall.png)
*Figure 8. Intervention ROI Waterfall (synthetic data). Illustrative systemic risk reduction from fixing the top bottlenecks in sequence.*

### Cross-Domain Contagion Heatmap
- **Problem:** Failure in one department can silently threaten another.
- **Solution:** Aggregate directed edge weights across node clusters (Markov blankets) to show inter-domain transmission.
- **Impact:** Reveals hidden cross-functional exposure (e.g., IT/Cyber → Lab Operations).

<!-- IMAGE: heatmap -->
<img width="755" height="652" alt="image" src="https://github.com/user-attachments/assets/f89916b8-d5b0-4f49-a82e-3464431a0b45" />

*Figure 9. Cross-Domain Contagion Heatmap (synthetic data). Probability of failure spreading from one domain to another.*

---

## The same engine, many domains

Causal propagation on a directed graph recurs across fields: contagion in **public health**, intervention effects in **tax and fiscal policy**, fund-flow in **elections and campaign finance**, and shock propagation in **physics**. Different domains, identical mathematical structure — which is why the same engine transfers across regulated and risk-analysis problems.

---

## Important caveats

- **All data is synthetic, by design** — generated with a known causal structure so the method can be validated against ground truth. The purpose is to demonstrate the method, not to forecast.
- **The pandemic series are trending,** so magnitudes are illustrative — the recovered *structure and direction* are the solid claim. A production version would difference the series and estimate from real data.
- **Educational proof of concept, not a deployed tool.** Not investment, enforcement, medical, or policy advice.
- Causal discovery rests on assumptions (e.g., no unmeasured confounders) that don't hold automatically in real data. Where the method can't decide, the uncertainty stays visible for human review.

---

## Methods / references

- **PCMCI** — Runge et al., causal discovery for time series (Tigramite).
- **do() operator and DAG-based causal inference** — Pearl.
- **NOTEARS, DoWhy** — related structure-learning and effect-estimation tooling.

## License
MIT

** The next step (a coordinated agent colony).** Inspired by how ant colonies 
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

