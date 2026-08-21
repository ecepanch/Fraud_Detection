# MULEWORLD

> **A world model of the adversary.**

MULEWORLD is an interactive simulation of India's shadow financial economy. It is not a fraud-detection dashboard — it is a sandbox where you pull defense levers and watch the adversary **remember, adapt, and evolve** in response.

Traditional bank fraud tools only see 1-hop transactions. Modern mule rings use 3-to-4-hop relay chains across multiple banks. Because banks cannot share PII, each sees a fragment and misses the network entirely. MULEWORLD shows what no single bank can see — and what happens when you try to stop it.

**Live demo:** https://ecepanch.github.io/Fraud_Detection/ — no install, runs entirely in the browser, works offline.

## World Model Rubric

| Property | How MULEWORLD demonstrates it |
|---|---|
| **Memory** | Rings remember frozen accounts. Burned mules are retired and never reused — the recruitment log and Burned Accounts ledger both reflect this. |
| **Interaction** | Four defense levers plus two scenario injectors. Every intervention changes adversary behaviour within ten simulated days. |
| **Consistency** | Deterministic seeded RNG. Reset produces an identical world. Patterns drift coherently over 90 days rather than randomly. |
| **Intervention** | The Policy Sandbox runs counterfactual 90-day A/B tests — same world, different defenses, diverging cumulative loss curves. |

## How to Run

1. Open the live demo, or open `index.html` in any browser.
2. Click **START SIMULATION**, then **4x**.
3. Watch for the **RING DETECTED** alert (fires around day 4).
4. Drag **Bank webhook coverage** down to 30% — within ten days the log shows rings routing through non-participating banks.
5. Tick **X-RAY** to reveal those dark hops in red.
6. Click **Run A/B (90 days)** for the counterfactual comparison.

## Architecture

```
Agent World (70 agents, 10 banks, 3 rings)
        │  tick() = one simulated day
        ▼
Transaction Generator ── victims → collector → relays → cashout → coordinator
        │                plus background family/merchant noise
        ▼
Detection Engine ── label propagation over VISIBLE transactions only
        │           (bank coverage lever controls what the operator can see)
        ▼
Adaptation Engine ── structuring · synthetic KYC · dark hops · cash cashout
        │            recruits replacements, never reuses burned accounts
        ▼
Policy Sandbox ── silent 90-day replay under two lever sets, loss curves + % prevented
```

### The four defense levers

| Lever | What it models | How the adversary answers |
|---|---|---|
| **Freeze threshold** | Risk score at which a bank freezes an account | Sustained freezes trigger structuring — amounts split below reporting thresholds |
| **U30 Disruption** | Soft-decline screens shown to victims mid-transfer | Ring pivots to synthetic-KYC recruitment |
| **DPIP Feed** | Regulatory feed supplying confirmed-mule seeds | Ring adds hops and shifts toward cash cashout |
| **Bank webhook coverage** | Fraction of banks sharing telemetry | Below 50%, rings route through non-participating banks — invisible to the operator, visible under X-RAY |

## Design Decisions

**Label propagation rather than Louvain.** Runs in-browser at interactive speed with no dependency. Detection has to fire live while someone is watching, not batch-compute correctly.

**Seeded RNG.** A world model claim only holds if the world is reproducible. Reset returns the identical world, which is also what makes the A/B counterfactual meaningful — both policy runs face the same adversary.

**Visibility is simulated, not assumed.** The central problem isn't that fraud is hard to detect; it's that no single bank sees the whole chain. The coverage lever makes that blind spot a first-class object in the model.

## Known Limitations

- Adaptation is a rule tree, not a learned policy. The adversary responds to pressure but does not optimise against it.
- Rings degrade rather than fully regenerate: when a collector or cashout is frozen, replacement is partial, so a heavily-pressured ring can go dormant instead of restructuring. Role-aware recruitment is the first fix on the list.
- Detection uses a rolling 7-day window; longer-horizon patterns are out of scope.
- Transaction amounts and ring topology are synthetic and illustrative, not calibrated against real NPCI or bank data.

## Tech Stack

Vanilla JavaScript, vis-network, Chart.js, Tailwind (CDN). No build step, no backend, no API keys. Single file.

## Why This Exists — Product Context

MULEWORLD is the adversarial-emulation module of **TrustNet**, a cross-bank fraud intelligence layer for the Indian financial ecosystem. TrustNet's premise is that mule rings are invisible to any single bank, and that a graph built across institutions — using DPIP fraud feeds, NCRP suspect data, and Account Aggregator consent-velocity metadata — can surface rings no individual bank can see.

**None of that infrastructure exists yet.** TrustNet is a design, not a deployed system; the planned stack (Neo4j, Redis, Kafka, FastAPI) is a roadmap and none of it is in this repo. What is real is this simulation — which is how you test a fraud policy before inflicting it on live customers.

## What I Would Build Next

- Role-aware recruitment so rings restructure rather than go dormant
- Replace the rule tree with a reinforcement-learning adversary trained against the operator's policy
- Multi-operator mode: two people pulling levers against the same world
- Export replay logs as training data for a defense policy

## Built By

**Venkatesh Panchariya** — solo builder, Inception II World Model Hackathon, August 2026. Track: Robotics & Simulation.

**Disclosure:** The base simulation engine was prototyped before the event. The Policy Sandbox, Burned Accounts ledger, and Adversary's Mind narration were built during Inception II.
