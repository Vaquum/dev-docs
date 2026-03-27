A very simple way to understand a particular `System` (e.g. Limen), is that it wholly consist of `Slices`. `Slices` in turn wholly consist of `Steps` and each step always consist of three `Layers`. The three `Layers` are `Capability`, `Proof`, and `Guardrails`.

In other words, a `System` consists of `Capabilities`, `Proofs`, and `Guardrails`. Planning of work is entirely based on this taxonomy.

## Taxonomy Structure

Each line reads as **node** `-` **definition**; `|` continues the spine; `|-` / `` `-` `` mark middle vs last sibling. `Step` has exactly three sibling layers: `Capability`, `Proof`, `Guardrails`.

```
System - collection of slices
  |
  |- Slice - one logical task (`slice_id`, `task_type`, summary; done only when compiler verdict is `PASS`)
  |     |
  |     |- Step - ordered action unit (`step_id`, sequence, status)
  |           |
  |           |- Layer: Capability - contains claims
  |           |     |
  |           |     |- Outcome claim - what changes
  |           |     |- Scope claim - `in_scope` / `out_of_scope`
  |           |     |- Control claim - default / range / off / on semantics
  |           |     |- Interface claim - expected behavior contract
  |           |     `- Non-goal claim - what will not change
  |           |
  |           |- Layer: Proof - evidence that claims are true
  |           |     |
  |           |     |- Gate results - CI / E2E / contract / architecture / audio
  |           |     |- Oracles - behavior truth artifacts
  |           |     |- Attestation - `verdict.json`
  |           |     |
  |           |     `- Oracle - one machine-verifiable proof object per behavior claim
  |           |           |
  |           |           `- fields - `oracle_id`, `task_id`, `subject_sha`, `status`, `metrics`,
  |           |                     `threshold check`, `raw_artifact`, `raw_sha256`, `harness_version` (machine, not manual)
  |           |
  |           `- Layer: Guardrails - contains constraints
  |                 |
  |                 |- Tests - add or update per test contract(s)
  |                 |- Developer docs - add or update per dev-doc contract(s)
  |                 |- User docs - add or update per user-doc contract(s)
  |                 |- Changelog - keep up to date per changelog contract
  |                 |- Package version - keep aligned per semver / release policy
  |                 |- Commit and push - per commit contract(s)
  |                 |- Process constraints - no direct commit before `PASS`
  |                 |- Scope constraints - no unauthorized files
  |                 |- Quality constraints - no skip/fixme where forbidden
  |                 |- Anti-deception constraints - no self-asserted manual proof
  |                 `- Rerun constraints - read diagnostic -> apply acceptable recipe -> rerun until `PASS`
```

## Taxonomy Details

1. `System` -> collection of slices.
2. `Slice` -> one logical task (`slice_id`, `task_type`, summary, done only when compiler verdict is `PASS`).
3. `Step` -> ordered action unit inside a slice (`step_id`, sequence, status).
4. `Layer` -> each step has exactly 3 layers: `Capability`, `Proof`, `Guardrails`.

5. `Capability` -> contains **claims**.
6. `Capability claims` -> `Outcome claim` (what changes), `Scope claim` (`in_scope`/`out_of_scope`), `Control claim` (default/range/off/on semantics), `Interface claim` (expected behavior contract), `Non-goal claim` (what will not change).

7. `Proof` -> contains evidence that claims are true.
8. `Proof evidence types` -> `Gate results` (CI/E2E/contract/architecture/audio), `Oracles` (behavior truth artifacts), `Attestation` (`verdict.json`).
9. `Oracle` -> one machine-verifiable proof object for one behavior claim.
10. `Oracle fields` -> `oracle_id`, `task_id`, `subject_sha`, `status`, `metrics`, `threshold check`, `raw_artifact`, `raw_sha256`, `harness_version` (machine, not manual).

11. `Guardrails` -> contains **constraints**.
12. `Constraint types` -> **Every-step contracts (typical):** add tests per test contract(s); add developer docs per dev-doc contract(s); add user docs per user-doc contract(s); keep changelog up to date per changelog contract; keep package version up to date per semver; commit and push per commit contract(s). **Process / integrity:** process constraints (no direct commit before `PASS`), scope constraints (no unauthorized files), quality constraints (no skip/fixme where forbidden), anti-deception constraints (no self-asserted manual proof), rerun constraints (read diagnostic -> apply acceptable recipe -> rerun until `PASS`).

Depending on the repo, other contract-backed or policy items may belong on every step (for example dependency or license rules, security baselines, migration or compatibility notes for public API)—often spelled out in the same contract set or in gates rather than as a separate line item.

So the ontology is: `System -> Slice -> Step -> Layer -> (Claims | Evidence/Oracles | Constraints)`.


Phases: Setup Phase, Calibration Phase, Maintenance Phase
Roles: Operator (Human), Auditor (Gemini), Governance-Agent (Codex), Worker-Agent (Opus)
