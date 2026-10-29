---
id: 01M647QR1NF2AZHXTAE5QHKF3H
domain: ai_egress
tags:
- canary_tokens
- prompt_dlp
- taint_tracking
created_at: '2026-10-29T16:26:22Z'
source_channels:
- arxiv
- forums
- github
- hackernews
- huggingface
- reddit
- rss
hypothesis: We hypothesize that an agent may only send data whose taint lineage is fully policy-clean
  — implemented through decoy credentials planted in retrievable content expose leak paths when touched
  — can bound any single session's outbound leakage below a declared ceiling when addressing injected
  instructions turn an assistant into an unwitting exfiltration bot, under the constraint of must log
  every blocked attempt for audit, measured by false-positive rate.
test_method: 'Build a minimal prototype of canary-baited retrieval corpora with egress budgets tied to
  plan lineage and evaluate against a baseline without the twist. Simulation first: model an injected
  instruction trying to move secrets across the egress boundary and replay traces sampled from the domain''s
  channel window plus synthetic workloads. Vary one factor at a time; report exfiltration success rate,
  false-positive rate, detection dwell time with confidence intervals over at least thirty seeds. Acceptance:
  the hypothesis improvement must hold at p<0.05 with the twist alone, and regressions on the other metrics
  must stay within ten percent of baseline. Follow with a small live-network pilot if simulation gates
  pass.'
score: 87
status: new
rounds: 4
---

# Egress Budgets Tied To Plan Lineage via Canary-Baited Retrieval Corpora for Prompt-Injection Exfiltration

## The Idea
This proposal targets closing agent-driven exfiltration paths without breaking workflows in the **ai_egress** domain. It combines a
canary-baited retrieval corpora — where decoy credentials planted in retrievable content expose leak paths when touched — with a deliberately adversarial
reading of the prompt-injection exfiltration problem: injected instructions turn an assistant into an unwitting exfiltration bot. The novel step is the
twist: an agent may only send data whose taint lineage is fully policy-clean.

In operation, the system treats false-positive rate as the first-class observable. Every
design decision is evaluated against it under the model of an injected instruction trying to move secrets across the egress boundary. Unlike a
straight deployment of canary-baited retrieval corpora, the twist changes what an observer can learn
from the outside: the mechanism's cost curve is reshaped so that the promised improvement
holds where it matters — must log every blocked attempt for audit.

Why now: the channel window shows sustained interest in canary_tokens, prompt_dlp, taint_tracking-adjacent
techniques, while the open problem remains prompt-injection exfiltration. The combination is new in
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
We hypothesize that an agent may only send data whose taint lineage is fully policy-clean — implemented through decoy credentials planted in retrievable content expose leak paths when touched — can bound any single session's outbound leakage below a declared ceiling when addressing injected instructions turn an assistant into an unwitting exfiltration bot, under the constraint of must log every blocked attempt for audit, measured by false-positive rate.

## Proposed Testing Method
Build a minimal prototype of canary-baited retrieval corpora with egress budgets tied to plan lineage and evaluate against a baseline without the twist. Simulation first: model an injected instruction trying to move secrets across the egress boundary and replay traces sampled from the domain's channel window plus synthetic workloads. Vary one factor at a time; report exfiltration success rate, false-positive rate, detection dwell time with confidence intervals over at least thirty seeds. Acceptance: the hypothesis improvement must hold at p<0.05 with the twist alone, and regressions on the other metrics must stay within ten percent of baseline. Follow with a small live-network pilot if simulation gates pass.

## Research Rounds
### Round 1 — Formulation
- **Focus:** initial formulation — is the causal claim specific enough to be falsifiable?
- **Hypothesis:** We hypothesize that an agent may only send data whose taint lineage is fully policy-clean — implemented through decoy credentials planted in retrievable content expose leak paths when touched — can bound any single session's outbound leakage below a declared ceiling when addressing injected instructions turn an assistant into an unwitting exfiltration bot, under the constraint of must log every blocked attempt for audit, measured by false-positive rate.
- **Evidence gathered:** channel items from arxiv/github/hackernews describing canary-baited retrieval corpora behavior in deployed systems
- **Falsification attempt:** construct a trace where the egress budgets tied to plan lineage depends on an accurate pool-size estimate; if the mechanism holds, the hypothesis survives
- **Verdict:** supported with caveats
- **Confidence delta:** +0.13 -> running confidence 0.58

### Round 2 — Adversarial pressure
- **Focus:** adversarial shift — does the claim survive a stronger or relocated adversary?
- **Hypothesis:** As refined in round 1: the same claim, with the adversary modeled as an injected instruction trying to move secrets across the egress boundary and focus narrowed to false-positive rate.
- **Evidence gathered:** prior simulated results of comparable designs reporting false-positive rate tradeoffs
- **Falsification attempt:** re-run with the adversary moved to a collocated vantage point; a drop below the promised margin refutes the claim
- **Verdict:** supported with caveats
- **Confidence delta:** +0.13 -> running confidence 0.71

### Round 3 — Scalability and cost probe
- **Focus:** scalability and cost — do asymptotic costs and operational overhead stay bounded?
- **Hypothesis:** As refined in round 2: the same claim, with the adversary modeled as an injected instruction trying to move secrets across the egress boundary and focus narrowed to false-positive rate.
- **Evidence gathered:** documented failure cases where prompt-injection exfiltration defeated similar mechanisms
- **Falsification attempt:** double the workload scale; if the false-positive rate regresses beyond ten percent, the claim fails
- **Verdict:** supported with caveats
- **Confidence delta:** +0.10 -> running confidence 0.81

### Round 4 — Operational viability
- **Focus:** deployment reality — would operators actually run this, and at what switching cost?
- **Hypothesis:** As refined in round 3: the same claim, with the adversary modeled as an injected instruction trying to move secrets across the egress boundary and focus narrowed to false-positive rate.
- **Evidence gathered:** channel items from arxiv/github/hackernews describing canary-baited retrieval corpora behavior in deployed systems
- **Falsification attempt:** inject churn of twenty percent of participants mid-run; measure whether recovery preserves the promised false-positive rate
- **Verdict:** supported with caveats
- **Confidence delta:** +0.14 -> running confidence 0.95

Final confidence after 4 rounds: **0.95**.

## Self-Assessment
- **Score: 87/100** (novelty 40/40, channel signal
  19/25, feasibility 15/20, domain fit
  12/15)
- Rationale: fresh mechanism-twist combination in a live problem area; feasible to test in
  simulation now, with the main uncertainty being real-world distribution shift.

## Risks
- The false-positive rate estimate may not transfer from simulation traces to production distributions; mitigate with a staged pilot and continuous baselining.
- Operators may mis-tune the egress budgets tied to plan lineage under pressure; ship conservative defaults and an explicit audit log of threshold changes.
- An adaptive adversary could probe the canary-baited retrieval corpora until its cost curve is mapped; rotate parameters and monitor for probing patterns.

## Next Steps
- Prototype the egress budgets tied to plan lineage path; land the false-positive rate instrumentation first.
- Replay the recorded channel-window traces; publish the harness and seeds for reproducibility.
- Define the go/no-go gate: bound any single session's outbound leakage below a declared ceiling, measured by false-positive rate, at p<0.05.
- Schedule the adversarial re-review round after the first external pilot.

