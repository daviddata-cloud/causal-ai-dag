[make_pandemic_whatif_dag.py](https://github.com/user-attachments/files/29167170/make_pandemic_whatif_dag.py)# causal-ai-dag
Causal AI with DAGs — discovery, intervention, and counterfactual what-if simulation (PCMCI, do-calculus)

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

<img width="1483" height="881" alt="pandemic_whatif_chart" src="https://github.com/user-attachments/assets/800f9b1e-502b-4b83-a170-c560c0ca636a" />

**Discovered causal graph** — PCMCI recovers the structure directly from data:

<img width="1380" height="1062" alt="pcmci_clean_graph" src="https://github.com/user-attachments/assets/ddbcf9f9-78e1-4562-bace-8dd96b3d886b" />

[Uploading make_pande"""
make_pandemic_whatif_dag.py -> pandemic_whatif_dag.png
Run:  python make_pandemic_whatif_dag.py
"""
import matplotlib.pyplot as plt
from matplotlib.patches import FancyBboxPatch, FancyArrowPatch
from matplotlib.lines import Line2D

fig, ax = plt.subplots(figsize=(13, 8.5))
ax.set_xlim(0, 13); ax.set_ylim(0, 9); ax.axis("off")

C = {"shock": "#A32D2D", "real": "#BA7517", "fin": "#1D9E75", "outcome": "#378ADD"}

N = {
    "pand":  (6.5, 8.0, "Pandemic shock", "do( ) inject here", "shock"),
    "mob":   (3.0, 6.2, "Mobility drop", "lockdowns", "real"),
    "fear":  (10.0, 6.2, "Market fear", "VIX spike", "fin"),
    "econ":  (3.0, 4.2, "Economic activity", "contraction", "real"),
    "liq":   (10.0, 4.2, "Liquidity", "funding stress", "fin"),
    "earn":  (3.0, 2.2, "Corporate earnings", "fall", "real"),
    "credit":(6.5, 2.2, "Credit spreads", "widen", "fin"),
    "equity":(10.0, 2.2, "Equity prices", "OUTCOME", "outcome"),
}
W, Hh = 2.5, 1.0
def node(k):
    x,y,t,s,r = N[k]
    ax.add_patch(FancyBboxPatch((x-W/2,y-Hh/2),W,Hh,
        boxstyle="round,pad=0.04,rounding_size=0.12",
        facecolor=C[r], edgecolor="black", linewidth=0.9, zorder=3))
    ax.text(x,y+0.13,t,ha="center",va="center",fontsize=10.5,
            fontweight="bold",color="white",zorder=4)
    ax.text(x,y-0.22,s,ha="center",va="center",fontsize=8.5,
            color="white",zorder=4)

E = [("pand","mob"),("pand","fear"),("mob","econ"),("econ","earn"),
     ("econ","credit"),("fear","liq"),("liq","credit"),
     ("earn","equity"),("credit","equity"),("fear","equity")]
for src,dst in E:
    x1,y1 = N[src][0], N[src][1]
    x2,y2 = N[dst][0], N[dst][1]
    ax.add_patch(FancyArrowPatch((x1,y1),(x2,y2),
        arrowstyle="-|>", mutation_scale=15, color=C[N[src][4]],
        linewidth=2.4, alpha=0.65, shrinkA=30, shrinkB=30,
        connectionstyle="arc3,rad=0.05", zorder=1))

for k in N: node(k)

ax.text(6.5, 0.55,
    "What-if (from data):  severity 0.5 → equity −0.87,  severity 1.0 → equity −1.73\n"
    "as severity rises, equity falls and credit spreads widen, together",
    ha="center", va="center", fontsize=10,
    bbox=dict(boxstyle="round,pad=0.5", fc="#EEEDFE", ec="#7F77DD", lw=1))

legend = [
    Line2D([0],[0],marker="s",color="w",markerfacecolor=C["shock"],markersize=13,label="injected shock"),
    Line2D([0],[0],marker="s",color="w",markerfacecolor=C["real"],markersize=13,label="real economy"),
    Line2D([0],[0],marker="s",color="w",markerfacecolor=C["fin"],markersize=13,label="financial system"),
    Line2D([0],[0],marker="s",color="w",markerfacecolor=C["outcome"],markersize=13,label="market outcome"),
]
ax.legend(handles=legend, loc="upper left", frameon=False,
          fontsize=9.5, ncol=4, bbox_to_anchor=(0.05, 1.0))

ax.set_title("Counterfactual stress test: if another pandemic hits, how does it reach the markets?",
             fontsize=13, pad=26)
plt.tight_layout()
plt.savefig("pandemic_whatif_dag.png", dpi=150, bbox_inches="tight")
print("Saved pandemic_whatif_dag.png")mic_whatif_dag.py…]()

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

## Methods / references

- PCMCI — Runge et al., causal discovery for time series (Tigramite).
- The `do()` operator and DAG-based causal inference — Pearl.
- NOTEARS, DoWhy — related structure-learning and effect-estimation tooling.

## License

MIT
