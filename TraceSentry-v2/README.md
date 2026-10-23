# TraceSentry v2

**Evidence-driven root-cause investigation for coding agents.**

TraceSentry helps an agent link symptoms to runtime paths, commits, dependencies/configuration, and prior incidents; compare competing hypotheses; suggest a minimal change; and report verification honestly.

## What's improved in v2

- Lightweight evidence map with explicit entities/relationships, without requiring a graph database.
- Clearer separation between observations, inferences and unknowns.
- Candidate assessment across code-path, diff, deployment, dependency/config, timing and verified history.
- Deterministic scoring is preferred when structured evidence exists; the model explains the result and does not invent a numeric score.
- `ambiguous`, `no code evidence`, and abstention behavior for incomplete evidence.
- A precise verification state machine: VERIFIED, PARTIALLY VERIFIED, FAILED, INCONCLUSIVE, NOT RUN.
- Safer fix boundaries, prompt-injection awareness, redaction and approval gates.
- Evaluation plan with held-out scenarios and explicit baselines.

## Structure

- `SKILL.md` — publishable skill instructions.
- `references/architecture-notes.md` — evidence model and possible architecture extensions.
- `references/evaluation-plan.md` — proposed benchmark plan.
- `LICENSE` — MIT.

## Status

This is a skill package and design specification, **not** a deployed observability application. It has not yet been benchmarked. No accuracy, latency or user-feedback results are claimed. Use the evaluation plan to collect real results before describing it as evaluated.

## Inspired by public engineering patterns

The design takes general inspiration from public engineering ideas such as evidence-linked graph traversal, explainable candidate ranking, citation validation, incident memory and verification gates. TraceSentry is an independent skill specification; it does not copy Kaizen's source code, branding, exact scoring weights or implementation.
