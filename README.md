# TrustNet

> **India's Cross-Bank Fraud Intelligence Layer**

TrustNet is not a better fraud tool. It's a new category — built on data no competitor can access. While traditional fraud detection systems operate within individual banks and miss cross-bank criminal networks, TrustNet maps the entire shadow financial ecosystem using regulatory mandates and licensed data rails.

## 🎯 Live Demo

**https://ecepanch.github.io/Fraud_Detection/** — Interactive "God-View" prototype showing cross-bank mule network detection.

---

## 🚨 The Problem Nobody Has Solved

Modern fraud rings use **3-to-4-hop relay chains** across multiple banks (HDFC → SBI → Axis → Cashout). Each bank sees only **1 hop** and raises no alert. The criminal network is invisible because:

- **Banks operate in silos** — cannot share customer PII due to privacy laws
- **Traditional tools sit inside one bank** — Clari5, BioCatch, TrustArmour all see accounts, not networks
- **Gateway models (like Razorpay Vulcan)** detect checkout behavior, but are blind to post-settlement money movement

**The result:** ₹5,000+ crores lost annually to Digital Arrest scams, mule networks, and synthetic identity farms.

---

## 🔍 What TrustNet Sees

### Individual Bank View (Siloed)
- **HDFC sees:** Account X → Y | ₹50K | 1 hop | No alert
- **SBI sees:** Account Y → Z | ₹48K | 1 hop | No alert  
- **Axis sees:** Account Z → ATM | ₹46K | 1 hop | No alert

### TrustNet God-View (Network)
- **3-hop relay chain** across 3 banks
- **Community C-07** identified (18 members)
- **Coordinator YYYY5678** isolated (never touches victim funds directly)
- **Action:** Trigger U30 soft-decline to break scammer's script

**That's the gap. That's what we solve.**

---

## 🏗️ Architecture: 3 Layers

### Layer 1: Offline Graph Engine (Refreshed 4x Daily)
- **Sources:** DPIP fraud feed + NCRP suspect cache + AA TSP consent metadata
- **Engine:** Neo4j → Louvain community detection → PageRank coordinator identification
- **Output:** Pre-computed account roles (Recruit, Relay, Collector, Cashout, Coordinator), ring memberships, risk scores
- **Refresh:** Every 6 hours

### Layer 2: Real-Time Lookup (<500ms)
- **Trigger:** Bank calls TrustNet API at transaction initiation
- **Action:** Look up destination account in pre-built graph snapshot
- **Output:** `{mule_risk: 0.87, role: Relay, community: C-07, action: SOFT_DECLINE}`
- **Latency:** <500ms (no live graph computation — pure snapshot lookup)

### Layer 3: Auto-Action Policy
- **Score > 75:** U30 technical error screen (breaks scammer script without accusing victim)
- **Score > 85:** Auto-freeze recommendation + STR auto-draft (bank executes)
- **Score > 90:** DPIP alert pushed back to network (all banks warned within 6 hours)

---

## 📦 Two Products. Two Revenue Streams.

### Product 1: Transaction Fraud Shield (For Banks)
**Stops fraud DURING the transaction**

**Detects:**
- Digital arrest / coercion scams
- Mule account transfers
- Balance drain (>90% liquidation)
- Senior citizen targeting
- Structuring (amounts just under reporting thresholds)

**Inputs:** DPIP + NCRP + Graph layer  
**Timeline:** Phase 2 (3-6 months)  
**Integration:** Lightweight alert webhooks (hashed IDs only — zero raw PII)

### Product 2: Lending Fraud Shield (For NBFCs)
**Stops fraud BEFORE disbursal**

**Detects:**
- Loan stacking (6 lenders in 2 hours)
- Synthetic identity farms
- Income-velocity mismatches
- Cross-lender fraud rings

**Inputs:** AA TSP consent velocity metadata ONLY  
**Timeline:** Phase 1 (0-3 months)  
**Dependency:** ZERO — fully independent, no bank integrations needed

---

## 🎬 Scenarios

### Scenario 1: Digital Arrest Stopped
**Context:** 67-year-old victim on live call with scammer, being coached to transfer ₹3.2L to "CBI escrow" account.

| Signal | Detection | TrustNet Action | Time |
|---|---|---|---|
| New beneficiary added | Rule A1 fires (+25 pts) | Flagged | T+0s |
| 4 transfers < ₹50K each | Velocity burst — Rule A2 (+25 pts) | Flagged | T+12s |
| OTP retries with gaps | Behavioral hesitation — F1 (+15 pts) | Flagged | T+18s |
| NCRP: dest account flagged | DPIP match auto-fires | **Auto-Fire** | T+22s |

**Risk Score:** 87  
**Action:** SOFT DECLINE → U30 technical error screen shown  
**Result:** ₹3.2L saved. Scammer script broken. Zero bank agent intervention.

### Scenario 2: Loan Stacking Blocked
**Context:** Borrower applies to 6 NBFCs across 3 Account Aggregators in 2 hours. ₹18L total exposure.

**Without TrustNet:**
- Each lender sees only 1-2 requests
- CIBIL score looks clean
- All 6 loans approved
- Defaults within 90 days → massive NPAs

**With TrustNet (AA TSP signal):**
- Cross-AA consent velocity score: **95**
- Lenders 4, 5, 6 receive `Auto-Decline` recommendation
- **₹12L NPA prevented**
- Decision made in **<2 seconds**

**This signal exists NOWHERE else.** Only an AA TSP sees all 6 consent events simultaneously.

---

## 🗺️ Build Roadmap

### Phase 1: Zero Dependency (Months 0-3)
✅ DPIP fraud feed integration  
✅ NCRP suspect cache (batch refresh daily)  
✅ Neo4j graph productionized from existing PoC  
✅ AA TSP consent velocity engine  
✅ Lending Fraud Shield API live  

**Outcome:** Demo-ready for 2 pilot NBFCs

### Phase 2: The Bank Layer (Months 3-6)
✅ Alert webhooks (push model, hashed IDs only — no raw PII)  
✅ Real-time <500ms transaction lookup  
✅ U30 disruption module + STR auto-draft engine  

**Outcome:** Transaction Fraud Shield live for pilot banks

### Phase 3: Platform Scale (Months 6-12)
✅ Full federated cross-bank graph (existing banks)  
✅ DPIP auto-action (auto-freeze, auto-STR at network level)  
✅ IP filing: consent velocity + graph mule classification  

**Outcome:** Network effect begins. Every bank onboarded makes the graph smarter for everyone.

---

## 🛡️ Privacy & Compliance

- **Zero raw PII stored centrally** — all account numbers, PANs, and Aadhaar tokenized at bank edge using HMAC-SHA256
- **No consented financial content read** — AA TSP processes consent *metadata* only (timestamps, FIU counts, status)
- **Banks remain data controllers** — TrustNet is a processor; banks decide whether to freeze or file STR
- **DPDP Act 2023 compliant** — purpose limitation, minimal data retention, auditable decisions

---

## 📊 Competitive Advantage

| Capability | BioCatch | Feedzai | SBI MuleHunter | **TrustNet** |
|---|---|---|---|---|
| Cross-bank graph ring detection | ❌ | ❌ | ❌ | ✅ |
| AA TSP consent velocity | ❌ | ❌ | ❌ | ✅ |
| DPIP/I4C inline enrichment | ❌ | ❌ | ⚠️ Batch only | ✅ |
| 5-role mule classification | ❌ | ❌ | ❌ | ✅ |
| Privacy-preserving (no PII) | ❌ | ❌ | ❌ | ✅ |
| India RBI/NPCI/I4C native | ❌ | ❌ | ✅ | ✅ |
| Deployment speed | 6-9 months | 9-12 months | N/A | **2-4 weeks** |

**This is not a feature gap.** BioCatch and Feedzai **cannot** get AA TSP access — it's a regulatory licensing gap. That's the moat.

---

## 🧠 Tech Stack

- **Graph Database:** Neo4j (community detection, PageRank, relationship mapping)
- **Real-Time Cache:** Redis (snapshot serving, <500ms lookups)
- **Event Streaming:** Kafka (consent events, bank webhooks, regulatory feeds)
- **Stream Processing:** Apache Flink (velocity computation, session rules)
- **API Layer:** FastAPI (decision APIs for NBFCs and banks)
- **Orchestration:** Apache Airflow (scheduled graph refresh, NCRP cache)
- **Infrastructure:** Kubernetes, India-region cloud (data residency compliance)

---

## 🎨 Design Decisions

**Why Louvain rather than GNNs?**  
Graph Neural Networks require years of labeled training data. Louvain community detection is deterministic, explainable, and works immediately on confirmed NCRP/DPIP seeds. Banks can audit every decision.

**Why pre-compute offline rather than live graph traversal?**  
Live graph queries over millions of nodes cannot hit <500ms. Pre-computing snapshots every 6 hours and serving via Redis guarantees latency SLAs while still catching mule rings within hours of formation.

**Why U30 soft-decline rather than hard block?**  
Hard blocks create legal liability and customer complaints. U30 screens show "technical error, try tomorrow" — breaking the scammer's coercion script without accusing the victim or exposing the bank to defamation claims.

---

## ⚠️ Known Limitations

- **Graph refresh is 6 hours** — not real-time. A mule ring formed at T+0 is detected by T+6h at latest.
- **Household false positives** — families sharing devices and moving money daily can trigger velocity rules. Countered with beneficiary vintage filters and bank-supplied "linked relationship" flags.
- **AA TSP legal clarity pending** — consent velocity analytics must be confirmed as within AA framework purpose limitations. Legal opinion commissioned.
- **DPIP partnership in progress** — RBIH pilot (2025) may require formal partnership with rbih.org.in for inline feed access.

---

## 🔮 What We're Building Next

- **Graph Neural Networks** — once we have 2+ years of historical graph snapshots, train GNNs to predict which clean accounts will become mules
- **Reinforcement learning adversary** — simulate mule rings adapting to defenses, train optimal intervention policies
- **Multi-operator mode** — federated learning across banks without sharing raw data
- **PSP enrichment API** — sell account-identity risk scores to gateways like Razorpay, PhonePe, Google Pay (the "Vulcan bridge")

---

## 👤 Built By

**Venkatesh Panchariya** — Building India's cross-bank fraud intelligence infrastructure.

**Contact:** [Your Email]  
**GitHub:** https://github.com/ecepanch  
**Live Demo:** https://ecepanch.github.io/Fraud_Detection/

---

## 📄 License

Proprietary. All rights reserved. TrustNet is a product of [Your Company/Name].

---

**Disclosure:** Base simulation prototyped pre-event. Policy Sandbox, Burned Accounts ledger, and Adversary's Mind narration built during Inception II World Model Hackathon (Aug 22-23, 2026).
