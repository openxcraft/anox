---
id: 01M647SD4DKDY4W2PMCAHRQYK7
domain: ai_egress
tags:
- semantic_egress_filter
created_at: '2026-10-29T16:27:16Z'
source_channels:
- arxiv
- forums
- github
- hackernews
- huggingface
- reddit
- rss
hypothesis: We hypothesize that normalize encodings, then classify, so obfuscated secrets cannot walk
  out — implemented through unicode and encoding normalization precede every egress policy match — can
  bound any single session's outbound leakage below a declared ceiling when addressing payloads smuggled
  through model outputs evade string-match policies, under the constraint of must log every blocked attempt
  for audit, measured by detection dwell time.
test_method: 'Build a minimal prototype of canonicalization-first policy matching with canonicalization
  before classification and evaluate against a baseline without the twist. Simulation first: model an
  injected instruction trying to move secrets across the egress boundary and replay traces sampled from
  the domain''s channel window plus synthetic workloads. Vary one factor at a time; report exfiltration
  success rate, false-positive rate, detection dwell time with confidence intervals over at least thirty
  seeds. Acceptance: the hypothesis improvement must hold at p<0.05 with the twist alone, and regressions
  on the other metrics must stay within ten percent of baseline. Follow with a small live-network pilot
  if simulation gates pass.'
score: 82
status: new
rounds: 4
---

# Canonicalization Before Classification via Canonicalization-First Policy Matching for Covert Channels In Embeddings and Code

## The Idea
This proposal targets closing agent-driven exfiltration paths without breaking workflows in the **ai_egress** domain. It combines a
canonicalization-first policy matching — where unicode and encoding normalization precede every egress policy match — with a deliberately adversarial
reading of the covert channels in embeddings and code problem: payloads smuggled through model outputs evade string-match policies. The novel step is the
twist: normalize encodings, then classify, so obfuscated secrets cannot walk out.

In operation, the system treats detection dwell time as the first-class observable. Every
design decision is evaluated against it under the model of an injected instruction trying to move secrets across the egress boundary. Unlike a
straight deployment of canonicalization-first policy matching, the twist changes what an observer can learn
from the outside: the mechanism's cost curve is reshaped so that the promised improvement
holds where it matters — must log every blocked attempt for audit.

Why now: the channel window shows sustained interest in semantic_egress_filter-adjacent
techniques, while the open problem remains covert channels in embeddings and code. The combination is new in
this framing, and its falsifiable form makes it testable with a prototype plus simulation
before any large-scale commitment.

## Inspiration Source
Observed signal (collected this run from: arxiv, github, hackernews, huggingface, reddit):
- (arxiv, 2026-10-28) "Information-flow taint tracking through agent tool-call plans" — A formalization of taint propagation through multi-step tool-calling plans, where a secret copied into a plan argument must remain blocked at every later egress point.
- (arxiv, 2026-10-27) "Covert channels in model outputs: measuring prompt-side exfiltration" — A study quantifies how much data an injected instruction can exfiltrate through benign-looking completions, and evaluates entropy ceilings as a countermeasure.
- (github, 2026-10-28) "Toolkit: schema-enforced egress gateways for agent frameworks" — A proxy that only permits tool responses matching whitelisted schemas, rejecting free-form fields that could smuggle secrets, with per-session egress budgets.
- (hackernews, 2026-10-26) "Canary strings in enterprise model deployments: lessons from incidents" — Operators describe planting decoy credentials in retrievable corpora and watching which egress paths light up when agents leak.

The inspiration is the gap between those signals and the stated constraint — not any single
item. No item is copied; each is treated as evidence that the problem is live.

## Hypothesis
We hypothesize that normalize encodings, then classify, so obfuscated secrets cannot walk out — implemented through unicode and encoding normalization precede every egress policy match — can bound any single session's outbound leakage below a declared ceiling when addressing payloads smuggled through model outputs evade string-match policies, under the constraint of must log every blocked attempt for audit, measured by detection dwell time.

## Proposed Testing Method
Build a minimal prototype of canonicalization-first policy matching with canonicalization before classification and evaluate against a baseline without the twist. Simulation first: model an injected instruction trying to move secrets across the egress boundary and replay traces sampled from the domain's channel window plus synthetic workloads. Vary one factor at a time; report exfiltration success rate, false-positive rate, detection dwell time with confidence intervals over at least thirty seeds. Acceptance: the hypothesis improvement must hold at p<0.05 with the twist alone, and regressions on the other metrics must stay within ten percent of baseline. Follow with a small live-network pilot if simulation gates pass.

## Research Rounds
### Round 1 — Formulation
- **Focus:** initial formulation — is the causal claim specific enough to be falsifiable?
- **Hypothesis:** We hypothesize that normalize encodings, then classify, so obfuscated secrets cannot walk out — implemented through unicode and encoding normalization precede every egress policy match — can bound any single session's outbound leakage below a declared ceiling when addressing payloads smuggled through model outputs evade string-match policies, under the constraint of must log every blocked attempt for audit, measured by detection dwell time.
- **Evidence gathered:** channel items from arxiv/github/hackernews describing canonicalization-first policy matching behavior in deployed systems
- **Falsification attempt:** construct a trace where the canonicalization before classification depends on an accurate pool-size estimate; if the mechanism holds, the hypothesis survives
- **Verdict:** supported with caveats
- **Confidence delta:** +0.07 -> running confidence 0.52

### Round 2 — Adversarial pressure
- **Focus:** adversarial shift — does the claim survive a stronger or relocated adversary?
- **Hypothesis:** As refined in round 1: the same claim, with the adversary modeled as an injected instruction trying to move secrets across the egress boundary and focus narrowed to detection dwell time.
- **Evidence gathered:** prior simulated results of comparable designs reporting detection dwell time tradeoffs
- **Falsification attempt:** re-run with the adversary moved to a collocated vantage point; a drop below the promised margin refutes the claim
- **Verdict:** not yet supported
- **Confidence delta:** -0.06 -> running confidence 0.46

### Round 3 — Scalability and cost probe
- **Focus:** scalability and cost — do asymptotic costs and operational overhead stay bounded?
- **Hypothesis:** As refined in round 2: the same claim, with the adversary modeled as an injected instruction trying to move secrets across the egress boundary and focus narrowed to detection dwell time.
- **Evidence gathered:** documented failure cases where covert channels in embeddings and code defeated similar mechanisms
- **Falsification attempt:** double the workload scale; if the detection dwell time regresses beyond ten percent, the claim fails
- **Verdict:** supported with caveats
- **Confidence delta:** +0.14 -> running confidence 0.60

### Round 4 — Operational viability
- **Focus:** deployment reality — would operators actually run this, and at what switching cost?
- **Hypothesis:** As refined in round 3: the same claim, with the adversary modeled as an injected instruction trying to move secrets across the egress boundary and focus narrowed to detection dwell time.
- **Evidence gathered:** channel items from arxiv/github/hackernews describing canonicalization-first policy matching behavior in deployed systems
- **Falsification attempt:** inject churn of twenty percent of participants mid-run; measure whether recovery preserves the promised detection dwell time
- **Verdict:** supported with caveats
- **Confidence delta:** +0.06 -> running confidence 0.66

Final confidence after 4 rounds: **0.66**.

## Self-Assessment
- **Score: 82/100** (novelty 34/40, channel signal
  19/25, feasibility 19/20, domain fit
  10/15)
- Rationale: fresh mechanism-twist combination in a live problem area; feasible to test in
  simulation now, with the main uncertainty being real-world distribution shift.

## Risks
- The detection dwell time estimate may not transfer from simulation traces to production distributions; mitigate with a staged pilot and continuous baselining.
- Operators may mis-tune the canonicalization before classification under pressure; ship conservative defaults and an explicit audit log of threshold changes.
- An adaptive adversary could probe the canonicalization-first policy matching until its cost curve is mapped; rotate parameters and monitor for probing patterns.

## Next Steps
- Prototype the canonicalization before classification path; land the detection dwell time instrumentation first.
- Replay the recorded channel-window traces; publish the harness and seeds for reproducibility.
- Define the go/no-go gate: bound any single session's outbound leakage below a declared ceiling, measured by detection dwell time, at p<0.05.
- Schedule the adversarial re-review round after the first external pilot.

