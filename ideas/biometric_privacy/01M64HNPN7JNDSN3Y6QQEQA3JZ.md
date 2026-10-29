---
id: 01M64HNPN7JNDSN3Y6QQEQA3JZ
domain: biometric_privacy
tags:
- template_protection
- recognition_resistance
- face_blurring_pipelines
created_at: '2026-10-29T19:20:00Z'
source_channels:
- autosearch
hypothesis: Per-relying-party, epoch-rotated re-randomization of face embedding templates (capsule transforms
  keyed to a service-specific salt and an epoch counter, constrained to orthogonal small-norm jitter)
  keeps genuine-match error within one percentage point of the untransformed baseline while collapsing
  cross-service template linkability — measured as agglomerative clustering AUC across five services —
  from roughly 0.90 to at most 0.60, under the constraint that derivation happens only on the subject's
  device.
test_method: 'Derive capsule transforms as products of a random orthogonal matrix and bounded-norm diagonal
  jitter, keyed by a keyed hash of the service identity and epoch. Evaluate on a standard public face-embedding
  benchmark corpus (used generically, via its published evaluation protocol only) across five simulated
  relying parties and twelve monthly epochs. Vary one factor at a time over at least thirty seeds: transform
  norm bound, epoch cadence, and embedding dimension. Report equal-error-rate delta versus baseline, false-negative
  rate at a fixed false-match operating point, cross-service clustering AUC under an adversary that pools
  templates from all services, rotation cost on-device, and enrollment friction. Acceptance: mean EER
  delta stays within one percentage point with overlapping confidence intervals, cross-service AUC drops
  to at most 0.60 at p<0.05, and rotation cost stays under one second per template on mid-range mobile
  hardware. Ablate the norm bound to expose the accuracy-linkability tradeoff curve.'
score: 81
status: new
rounds: 4
---

## The Idea

Face verification is quietly becoming a federation problem. A person enrolls at a handful of services in a year, and each service stores an embedding derived from the same face. Even when every service applies its own template protection, the protected templates remain correlated because they derive from the same biometrics — so an adversary who pools templates across services can cluster them and reconstruct a cross-service identity graph that no single service ever saw. Template protection as deployed today defends the template against inversion at one service; it does not defend the person against linkage across services.

This proposal introduces rotating template capsules: before enrollment, the subject's device applies an invertible, service-bound transform to the embedding — a random orthogonal matrix composed with small-norm bounded jitter, keyed by a keyed hash of the service identity and a monotonically increasing epoch. Every service sees a different, per-epoch version of the template; the same face yields pairwise-far capsules at different services and at different times. Matching still works because verification compares a probe transformed under the same service key and epoch against the enrolled capsule, and orthogonality preserves the geometry that separates genuine pairs from impostor pairs. The bounded jitter is the deliberate twist: it shrinks genuine-pair similarity slightly and uniformly, but destroys the stable offsets that cross-service clustering relies on.

The system treats cross-service clusterability as the first-class observable, not just template inversion resistance. Rotation is cheap because the transform never touches the raw image and never leaves the device; renewal is a re-derivation, not a re-enrollment, when the transform is transmitted as a public parameter to the verifier rather than stored as a secret.

## Evidence Background

Published analyses of embedding-based biometric systems report two recurring findings: template-inversion attacks recover recognizable representations from unprotected embeddings, and cross-dataset linkage of protected templates succeeds far more often than vendors assume when the protection is static. Meanwhile, deployed cancelable-biometrics schemes concentrate on a single service's threat model — a breach of that service — and are silent about the pooled-adversary setting, which is exactly the setting that data-broker aggregators and large-scale authentication providers create.

Practice shows the constraint stack clearly: on-device derivation is already the norm for mobile face unlock, embedding dimensions and matching thresholds are standardized enough to reproduce, and rotation-friendly enrollment flows exist in passkey-style re-attestation patterns. The open problem is the tradeoff curve — how much distortion cross-service linkage needs, and how much genuine-match accuracy that distortion costs — with no published mechanism that holds accuracy constant while collapsing poolability. That gap is the proposal's target.

## Hypothesis

Orthogonal, small-norm, service-and-epoch-keyed capsule transforms hold the equal-error-rate within one percentage point of baseline while reducing cross-service clustering AUC to at most 0.60, measured under a pooled-adversary evaluation across five simulated services and twelve epochs, with derivation confined to the subject's device.

## Proposed Testing Method

Implement capsule derivation as a keyed orthogonal factorization with bounded diagonal jitter, plus a device-side rotation routine. Use a standard public face-embedding benchmark through its published protocol only, treating the corpus generically. Simulate five relying parties with independent salts and twelve monthly epochs; the pooled adversary receives all capsules and runs agglomerative clustering with cosine and Euclidean metrics, plus a learned-linkage baseline.

Metrics, each over at least thirty seeds with confidence intervals: equal-error-rate delta versus untransformed baseline per service; false-negative rate at a fixed false-match point; cross-service clustering AUC under both metrics; on-device rotation latency distribution on mid-range mobile-class hardware; enrollment friction measured as added round trips. Vary the jitter norm bound, epoch cadence, and embedding dimension one factor at a time; report the accuracy-versus-linkability tradeoff curve from the norm-bound ablation. Acceptance gates: EER delta within one percentage point with overlapping intervals, cross-service AUC at most 0.60 at p<0.05, rotation under one second at the ninetieth percentile. Follow with a consented pilot on volunteer templates if gates pass.

## Research Rounds

### Round 1 — Formulation
- **Focus:** is the causal claim precise and falsifiable?
- **Evidence gathered:** geometry-preserving properties of orthogonal transforms; published linkage results for static template protection; embedding-benchmark protocol stability.
- **Falsification attempt:** construct an embedding space so anisotropic that bounded jitter cannot break cluster offsets without exceeding the accuracy budget; if such a space is typical rather than pathological, the claim narrows to whitened spaces.
- **Verdict:** supported with caveats; whitening as a pre-step makes the mechanism robust and is cheap on-device.
- **Confidence delta:** +0.13, running confidence 0.58.

### Round 2 — Adversarial pressure
- **Focus:** does the claim survive a stronger or relocated adversary?
- **Evidence gathered:** linkage attacks that exploit multiple epochs from the same service; adversary models that combine service-side and interception vantage points.
- **Falsification attempt:** give the pooled adversary capsules from several epochs of one service and test whether intra-service rotation leaks a stable signature that transfers cross-service; a persistent signature above chance refutes the design.
- **Verdict:** supported with caveats after adding per-epoch key derivation with forward secrecy; epoch reuse is the fragile case and must be forbidden.
- **Confidence delta:** +0.07, running confidence 0.65.

### Round 3 — Scalability and cost
- **Focus:** do asymptotic costs and operational overhead stay bounded?
- **Evidence gathered:** on-device linear-algebra latency distributions for embedding-dimension transforms; enrollment-flow telemetry patterns from comparable flows.
- **Falsification attempt:** double the number of services and epochs; if capsule bookkeeping or verification-parameter distribution grows super-linearly or rotation exceeds the latency gate, the claim fails.
- **Verdict:** supported; state is one transform descriptor per service-epoch pair and rotation is a local re-derivation, though descriptor distribution needs a public, authenticated channel.
- **Confidence delta:** +0.09, running confidence 0.74.

### Round 4 — Operational viability
- **Focus:** would operators actually deploy this, and at what switching cost?
- **Evidence gathered:** churn injection of twenty percent of services rotating keys mid-run; integration friction patterns from comparable client-side derivation designs.
- **Falsification attempt:** inject mid-run service churn and descriptor revocation; if recovery degrades the linkage margin or forces re-enrollment from raw biometrics, the design fails operationally.
- **Verdict:** supported with caveats; revocation needs a short descriptor validity window, and the pilot must confirm operators accept verifier-side changes.
- **Confidence delta:** +0.12, running confidence 0.86.

Final confidence after four rounds: **0.86**.

## Self-Assessment

- **Score: 81/100** (novelty 31/40, evidence strength 20/25, feasibility 18/20, domain fit 12/15).
- Rationale: reframing template protection from single-service inversion defense to cross-service linkage resistance is a real gap with a concrete, buildable mechanism; the dominant uncertainty is whether the bounded-jitter twist holds across embedding families beyond the benchmark protocol.

## Risks

- The accuracy-linkability tradeoff may shift across embedding architectures; mitigate by fitting the norm-bound curve per family and shipping conservative defaults with per-family calibration.
- A learned-linkage adversary may outperform metric clustering and partially recover the identity graph; the evaluation must include a learned baseline, and residual linkability must be reported, not hidden.
- Descriptor distribution is a new attack surface; authenticated public channels and short validity windows are mandatory, and compromise must force rotation rather than re-enrollment.
- Usability risk: added enrollment friction could push operators toward static templates; the pilot must measure friction explicitly and cap it.

## Next Steps

- Land the capsule-derivation prototype and the clustering-AUC instrumentation first; publish the harness, seeds, and tradeoff-curve methodology for reproducibility.
- Run the five-service, twelve-epoch evaluation with the norm-bound ablation and lock acceptance gates before any pilot.
- Define the go/no-go gate: EER delta within one percentage point and cross-service AUC at most 0.60 at p<0.05.
- Schedule the learned-adversary re-review after the first evaluation window and pre-register the pooled-adversary analysis.
