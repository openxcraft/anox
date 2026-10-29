# Research Library — Privacy, Anonymity & Data Protection

A structured, deduplicated collection of original research records produced
by an autonomous, LLM-driven multi-agent research swarm. Each cycle searches
current public sources across the full privacy, anonymity, and
data-protection spectrum — anonymity tooling, metadata protection, encrypted
communications, tracking and fingerprinting defense, OSINT and exposure
defense, privacy-preserving computation, identity and pseudonymity, payment
privacy, mobile and IoT privacy, storage and backup encryption, biometric
privacy, AI and model privacy, AI egress control, decentralized systems,
censorship-resistant protocols, data-leak prevention, data-protection
operations, and quantum-safe privacy — and authors new mechanism-level
research records from what it finds.

## Snapshot

| metric | value |
|---|---|
| total records | 46 |
| domains | ai_egress (8), anonymity (9), biometric_privacy (1), censorship_resistance (9), data_leak_prevention (9), decentralization (6), osint_defense (1), privacy (3) |
| median score | 83 / 100 |
| last updated | 2026-10-29 |

## Layout

- `ideas/<domain>/<record_id>.md` — one record per research idea: validated
  frontmatter (id, tags, hypothesis, test method, score, rounds) plus a full
  body covering the idea, evidence background, hypothesis, testing plan,
  research rounds, self-assessment, risks, and next steps.
- `index.jsonl` — machine-readable index of every published record (one JSON
  object per line).

Every record passes strict schema validation, three-layer deduplication
(exact-hash, embedding-similarity, topic-graph neighborhood), a quality gate
(no placeholders, no demo content), and a sanitization review before
publication. Only finished research records are published here.
