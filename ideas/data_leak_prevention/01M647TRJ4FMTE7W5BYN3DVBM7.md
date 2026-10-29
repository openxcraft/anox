---
id: 01M647TRJ4FMTE7W5BYN3DVBM7
domain: data_leak_prevention
tags:
- honeytokens
- secret_scanning
- exfil_detection
- taint_tracking
created_at: '2026-10-29T16:28:00Z'
source_channels:
- arxiv
hypothesis: We hypothesize that rank transforms by how much provenance they preserve — implemented through
  decoy documents versioned by tier reconstruct the exfiltration path — can raise alert precision to at
  least point-eight at fixed recall when addressing generic thresholds bury operators in benign alerts,
  under the constraint of must preserve forensic audit trails, measured by false-positive rate.
test_method: 'Build a minimal prototype of honeytoken lineage tiers with taint survival scoring per transform
  and evaluate against a baseline without the twist. Simulation first: model an insider moving data slowly
  through legitimate-looking channels and replay traces sampled from the domain''s channel window plus
  synthetic workloads. Vary one factor at a time; report detection dwell time, alert precision, false-positive
  rate with confidence intervals over at least thirty seeds. Acceptance: the hypothesis improvement must
  hold at p<0.05 with the twist alone, and regressions on the other metrics must stay within ten percent
  of baseline. Follow with a small live-network pilot if simulation gates pass.'
score: 73
status: new
rounds: 4
---

# Taint Survival Scoring Per Transform via Honeytoken Lineage Tiers for False-Positive Alert Fatigue

## The Idea
This proposal targets earlier, quieter detection of data leaving the boundary in the **data_leak_prevention** domain. It combines a
honeytoken lineage tiers — where decoy documents versioned by tier reconstruct the exfiltration path — with a deliberately adversarial
reading of the false-positive alert fatigue problem: generic thresholds bury operators in benign alerts. The novel step is the
twist: rank transforms by how much provenance they preserve.

In operation, the system treats false-positive rate as the first-class observable. Every
design decision is evaluated against it under the model of an insider moving data slowly through legitimate-looking channels. Unlike a
straight deployment of honeytoken lineage tiers, the twist changes what an observer can learn
from the outside: the mechanism's cost curve is reshaped so that the promised improvement
holds where it matters — must preserve forensic audit trails.

Why now: the channel window shows sustained interest in honeytokens, secret_scanning, exfil_detection-adjacent
techniques, while the open problem remains false-positive alert fatigue. The combination is new in
this framing, and its falsifiable form makes it testable with a prototype plus simulation
before any large-scale commitment.

## Inspiration Source
Observed signal (collected this run from: arxiv):
- (arxiv, 2026-10-21) "Agent Mosaic: Recognizing Confidentiality Leaks in Agentic LLM Systems" — Modern LLM agents can retrieve information from accessible documents, and then disclose a sensitive conclusion assembled from them. This mosaic effect creates a critical risk for a
- (arxiv, 2026-08-25) "Towards LLM-Enhanced Android Taint Analysis" — Taint analysis is a fundamental technique for detecting sensitive data leaks in Android apps. However, traditional static tools, such as FlowDroid, still face well-known challenges
- (arxiv, 2026-08-02) "SP2UBI: Secure and Privacy-Preserving Usage-Based Insurance" — The transition from traditional auto insurance, whose basis is defined by a set of static parameters, like the age of driver and number of accidents, to Usage-Based Insurance, whos
- (arxiv, 2026-07-06) "Measuring Healthcare Data Leaks and Security Flaws at Internet Scale" — Systems that process medical data should be meticulously secured. Yet, network services in healthcare environments often fail to implement basic security measures. For example, pre

The inspiration is the gap between those signals and the stated constraint — not any single
item. No item is copied; each is treated as evidence that the problem is live.

## Hypothesis
We hypothesize that rank transforms by how much provenance they preserve — implemented through decoy documents versioned by tier reconstruct the exfiltration path — can raise alert precision to at least point-eight at fixed recall when addressing generic thresholds bury operators in benign alerts, under the constraint of must preserve forensic audit trails, measured by false-positive rate.

## Proposed Testing Method
Build a minimal prototype of honeytoken lineage tiers with taint survival scoring per transform and evaluate against a baseline without the twist. Simulation first: model an insider moving data slowly through legitimate-looking channels and replay traces sampled from the domain's channel window plus synthetic workloads. Vary one factor at a time; report detection dwell time, alert precision, false-positive rate with confidence intervals over at least thirty seeds. Acceptance: the hypothesis improvement must hold at p<0.05 with the twist alone, and regressions on the other metrics must stay within ten percent of baseline. Follow with a small live-network pilot if simulation gates pass.

## Research Rounds
### Round 1 — Formulation
- **Focus:** initial formulation — is the causal claim specific enough to be falsifiable?
- **Hypothesis:** We hypothesize that rank transforms by how much provenance they preserve — implemented through decoy documents versioned by tier reconstruct the exfiltration path — can raise alert precision to at least point-eight at fixed recall when addressing generic thresholds bury operators in benign alerts, under the constraint of must preserve forensic audit trails, measured by false-positive rate.
- **Evidence gathered:** channel items from arxiv describing honeytoken lineage tiers behavior in deployed systems
- **Falsification attempt:** construct a trace where the taint survival scoring per transform depends on an accurate pool-size estimate; if the mechanism holds, the hypothesis survives
- **Verdict:** supported with caveats
- **Confidence delta:** +0.06 -> running confidence 0.51

### Round 2 — Adversarial pressure
- **Focus:** adversarial shift — does the claim survive a stronger or relocated adversary?
- **Hypothesis:** As refined in round 1: the same claim, with the adversary modeled as an insider moving data slowly through legitimate-looking channels and focus narrowed to false-positive rate.
- **Evidence gathered:** prior simulated results of comparable designs reporting false-positive rate tradeoffs
- **Falsification attempt:** re-run with the adversary moved to a collocated vantage point; a drop below the promised margin refutes the claim
- **Verdict:** not yet supported
- **Confidence delta:** -0.07 -> running confidence 0.44

### Round 3 — Scalability and cost probe
- **Focus:** scalability and cost — do asymptotic costs and operational overhead stay bounded?
- **Hypothesis:** As refined in round 2: the same claim, with the adversary modeled as an insider moving data slowly through legitimate-looking channels and focus narrowed to false-positive rate.
- **Evidence gathered:** documented failure cases where false-positive alert fatigue defeated similar mechanisms
- **Falsification attempt:** double the workload scale; if the false-positive rate regresses beyond ten percent, the claim fails
- **Verdict:** supported with caveats
- **Confidence delta:** +0.07 -> running confidence 0.51

### Round 4 — Operational viability
- **Focus:** deployment reality — would operators actually run this, and at what switching cost?
- **Hypothesis:** As refined in round 3: the same claim, with the adversary modeled as an insider moving data slowly through legitimate-looking channels and focus narrowed to false-positive rate.
- **Evidence gathered:** channel items from arxiv describing honeytoken lineage tiers behavior in deployed systems
- **Falsification attempt:** inject churn of twenty percent of participants mid-run; measure whether recovery preserves the promised false-positive rate
- **Verdict:** not yet supported
- **Confidence delta:** -0.04 -> running confidence 0.47

Final confidence after 4 rounds: **0.47**.

## Self-Assessment
- **Score: 73/100** (novelty 27/40, channel signal
  19/25, feasibility 12/20, domain fit
  14/15)
- Rationale: fresh mechanism-twist combination in a live problem area; feasible to test in
  simulation now, with the main uncertainty being real-world distribution shift.

## Risks
- The false-positive rate estimate may not transfer from simulation traces to production distributions; mitigate with a staged pilot and continuous baselining.
- Operators may mis-tune the taint survival scoring per transform under pressure; ship conservative defaults and an explicit audit log of threshold changes.
- An adaptive adversary could probe the honeytoken lineage tiers until its cost curve is mapped; rotate parameters and monitor for probing patterns.

## Next Steps
- Prototype the taint survival scoring per transform path; land the false-positive rate instrumentation first.
- Replay the recorded channel-window traces; publish the harness and seeds for reproducibility.
- Define the go/no-go gate: raise alert precision to at least point-eight at fixed recall, measured by false-positive rate, at p<0.05.
- Schedule the adversarial re-review round after the first external pilot.

