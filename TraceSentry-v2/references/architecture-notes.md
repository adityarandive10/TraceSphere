# TraceSentry v2 — Architecture Notes

TraceSentry is intentionally skill-first. The skill guides a host coding agent; it does not claim to provision live telemetry or a production graph database.

## Core flow

```text
Incident input
   ↓
Normalize symptom and time window
   ↓
Collect read-only evidence (logs, stack frames, diff, CI, deploy/config records)
   ↓
Build bounded evidence map / table
   ↓
Generate candidate set from changes actually in scope
   ↓
Deterministic evidence scoring where structured data exists
   ↓
LLM explains ranking with citations to returned evidence
   ↓
Human reviews minimal proposed fix
   ↓
Targeted tests / integration checks / optional staging replay
   ↓
Verification verdict + redacted incident memory
```

## Evidence item shape

```json
{
  "id": "evidence-unique-id",
  "kind": "stack_frame | commit_diff | deployment | config_change | dependency_change | test_result | prior_incident",
  "source": "observed repository path, commit SHA, run URL, or incident ID",
  "observed_at": "timestamp if supplied by source",
  "claim": "short factual statement",
  "supports": ["hypothesis-id"],
  "contradicts": [],
  "limitations": []
}
```

Only observed evidence may appear in the `source` field. Missing values remain missing rather than being inferred.

## Candidate score concept

When implementing a real scorer, score candidates from explicit evidence features such as path linkage, diff relevance, deployment membership, dependency/config relation, timeline compatibility and verified incident history. Keep feature values and weights visible and versioned. Treat temporal proximity and lexical similarity as weak supporting signals. Penalize missing/conflicting evidence; do not convert absence of evidence into evidence of absence. Allow `ambiguous` and `no_code_evidence` verdicts.

Do not reproduce another project's weights or ranking algorithm. Calibrate this project's method against TraceSentry's own held-out dataset.

## Suggested graph model if the host platform supports graph storage

Nodes: Repository, Commit, Function, Endpoint, Service, Dependency, ConfigChange, Deployment, Incident, Error, Investigation, Evidence, Hypothesis, Fix, VerificationRun, PriorIncident.

Edges: MODIFIES, CALLS, HANDLES, DEPENDS_ON, INCLUDED_IN_DEPLOYMENT, OBSERVED_ON, THROWN_IN, CANDIDATE_FOR, SUPPORTED_BY, CONTRADICTED_BY, ADDRESSED_BY, VERIFIED_BY, RESEMBLES.

Scope graph reads to one repository/incident. Bound path depth, parameterize queries, limit execution time, and validate citations against retrieved evidence IDs. Keep graph storage optional; the skill should still work using a Markdown table or JSON evidence bundle.

## Future implementation boundaries

Potential modules if expanded beyond a SkillHub skill:
- evidence collector interfaces (Git diff/history, CI, deployment, stack traces);
- schema/versioned event model;
- repository graph adapter;
- deterministic ranking module;
- citation validator and structured output schema;
- verifier interface for tests or staging;
- redaction, audit log and evaluation harness.

These are future implementation ideas, not existing features of this skill package.
