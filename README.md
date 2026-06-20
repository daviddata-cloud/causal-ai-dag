# causal-ai-dag
Causal AI with DAGs — discovery, intervention, and counterfactual what-if simulation (PCMCI, do-calculus)

# Causal AI: discovery, intervention, and counterfactual what-ifs

> AI can predict almost anything now. Trusting it is the hard part.
> This repo is a small, end-to-end **causal AI** pipeline — using Directed
> Acyclic Graphs (DAGs) — that answers the question prediction can't: not
> just *what* will happen, but *what causes it*, and *what happens if we
> intervene*.

![One causal simulation engine, many domains](images/causal_engine_domains.png)

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

![Pandemic stress test](images/pandemic_whatif_chart.png)

**Discovered causal graph** — PCMCI recovers the structure directly from data:

![Discovered causal graph](images/pcmci_clean_graph.png)

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
