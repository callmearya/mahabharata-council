# Mahabharata Council Implementation Roadmap

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deliver an evidence-first, provider-neutral council of coding and knowledge-work subagents whose generated bundles run across supported hosts, whose findings are promoted only after mechanical verification, and whose performance claims never exceed published evidence.

**Architecture:** Canonical JSON contracts and prompt fragments compile into host-native bundles through five `BundleAdapter` implementations. A separate provider-neutral managed runtime characterizes tasks, probes observable capabilities, chooses the least-complex qualified strategy, preserves context through typed handoffs, verifies evidence outside the reporting model, and records signed qualification decisions. Deterministic conformance ships before live performance qualification.

**Tech Stack:** Node.js 24.19.0 LTS, npm 11.17.0, TypeScript 7.0.2, JSON Schema 2020-12, Ajv 8.20.0, Vitest 4.1.7, fast-check 4.9.0, Python 3.13.15 with uv 0.12.1 for statistical analysis, and rootless Podman 5.8.2 for the later VT2 reference verifier.

**Spec:** [System design](../specs/2026-08-21-mahabharata-council-design.md) and [Delegation Assurance design](../specs/2026-08-21-delegation-assurance-design.md)

## Global Constraints

- The first releasable artifact is a deterministic conformance release. It makes no claim that the council outperforms inline work, generic subagents, another framework, or any model.
- Automatic Council routing starts disabled and returns `none` before qualification. Bare native/default execution (`B0`) remains available only as an explicit host/user action outside Council routing and as an evaluation comparator; Council strategies, including I0, may run only as explicitly forced experimental conditions. I0 becomes routable only after its exact material profile and scope qualify, and failure of I0’s mandatory gates produces `do-not-route`.
- Compatibility means that a bundle compiles, installs, and passes target conformance. It does not mean that a model-host combination performs well.
- A finding is not verified because a model says that it ran a command. Promotion requires an independently produced verifier receipt whose subject, scope, freshness, and artifact digest match the finding.
- Unsupported, inaccessible, or weak models degrade to a simpler strategy or abstain. They never inherit a recommendation merely because their display name resembles a qualified model.
- The implementation spends no incremental money. Existing subscription or local-compute capacity may be used only when its account, quota, run identity, and artifacts can be recorded. Otherwise live qualification stops without blocking conformance work.
- Sealed confirmatory evidence requires independent custody. Developmental runs by project contributors remain labeled developmental.
- Canonical JSON sources are UTF-8, NFC-normalized, LF-only, duplicate-key-free, and LF-terminated. RFC 8785 canonical bytes are used before hashing.
- JSON Schema is the semantic source of truth. TypeScript types are generated and checked for byte-for-byte drift; no second validation schema is maintained in code.
- Build and test dependencies are exact versions in `package-lock.json`. CI uses `npm ci` and has no live-provider dependency.
- Public documentation, README content, cultural framing, examples, prompt language, and public rubrics are authored by Viji, who also coordinates their review. Interpretive acceptance for contested-source or persona changes comes from a second named non-author reviewer or a documented public review window under parent Section 5.3; Viji cannot self-approve those changes. Core schemas, compiler, runtime, adapters, installer, evaluation machinery, CI, and release mechanics are authored by Arya.
- Git authorship must describe actual work. Each contributor uses a separate substantive branch and pull request; commits are never rewritten merely to manufacture a contribution split.
- Repository-local work must not modify global Git identity, global credential helpers, the desktop Git account switcher, or another application’s account state.

## Locked Decisions

| Area | Decision | Consequence |
|---|---|---|
| Repository | One root npm project for v0.1 | Shared compiler options and one lockfile keep deterministic checks simple; internal package boundaries are directory and export boundaries. |
| Runtime floor | Node.js 24.19.0 LTS; npm 11.17.0 | Contributors and CI use the same runtime. Generated prompt bundles remain plain text/JSON and do not require Node at inference time. |
| Language | TypeScript 7.0.2 with `strict`, `noUncheckedIndexedAccess`, and ESM | Core code has one typed implementation language and explicit undefined handling. |
| Tests | Vitest 4.1.7 plus fast-check 4.9.0 | Example, integration, mutation, and property tests use one runner. |
| Formatting | Prettier 3.9.6; strict TypeScript; source-invariant checker | No linter dependency is added until a concrete rule gap is demonstrated. |
| Contract validation | JSON Schema 2020-12 through Ajv 8.20.0 and ajv-formats 3.0.1 | Stable `$id` values, strict validation, and schema-derived TypeScript types prevent split authority. |
| Source parsing | `jsonc-parser` 3.3.1 in JSON-only mode | The loader can reject duplicate object keys before ordinary JSON parsing erases them. |
| Canonicalization | `canonicalize` 3.0.0, checked against RFC 8785 vectors | The library is an implementation detail; project tests define the accepted byte output. |
| Fingerprints | SHA-256, base64url without padding, domain-separated; `mpf:jcs-rfc8785-v1:sha-256:<digest>` for material profiles | Equal semantic inputs have stable fingerprints; different record classes cannot collide by accidental reuse of identical JSON. |
| Signatures | Ed25519 over domain-separated RFC 8785 bytes; public keys are JWK; private keys use a non-exportable key-provider interface | Test keys can be ephemeral, while production recommendation activation remains disabled without real custody. |
| Host packaging | Five independent `BundleAdapter`s: Claude Code, Codex, Copilot/VS Code, Kiro CLI, and Kiro IDE | Native file layouts and feature degradation are target-specific and separately tested. |
| Managed execution | `ExecutionDriver` is separate from `BundleAdapter` | Packaging support cannot be mistaken for provider execution support. |
| Version-1 driver set | OpenAI-compatible Chat Completions, Ollama, and separately pinned Codex CLI, Claude Code CLI, and Kiro CLI brokers | Each driver earns its own status. A host CLI that cannot pass the automation, isolation, credential, egress, lifecycle, and cancellation suite remains native/install-only. |
| Strategy default | The Council router defaults to `none`; B0 is an explicit host/user bypass outside Council and an evaluation comparator; I0, I1, S1, and C1 are forced-experimental until exact qualification | Council assistance and delegation both earn activation through scope-matched evidence. |
| Verification | `VT0-self-asserted`, `VT1-reproducible-unisolated`, `VT2-controller-isolated`, and `VT3-independent-adjudication`; rootless Podman is the VT2 reference backend | Only VT2 promotes mechanically decidable findings; irreducibly semantic promotion requires eligible VT3 receipts. Unsupported isolation degrades to VT1. |
| VT2 platform order | Rootless Podman on native Linux first; macOS and Windows only after separate Podman-machine conformance | A Linux result never silently grants VT2 to another operating system. |
| Persistent run/journal execution | Run and execution-journal storage is implemented only in P07’s controller plan after crash-safety acceptance tests are specified; P05 separately owns the qualification/custody registry store and its tested storage abstraction | The foundation kernel does not accidentally make a run-durability promise, while registry activation still has an explicit persistence owner. |
| Evaluation | Native TypeScript launcher; locked Python analysis environment | Run control stays close to runtime contracts while statistical tooling remains reproducible. |
| Release | Conformance can ship with every performance record `unverified`; managed and performance labels require separate gates | Useful portability does not wait for costly live evidence, and marketing cannot outrun measurement. |

## Repository Layout

The following tree is the locked destination structure. A directory is introduced by the package that owns its first contract; empty directories are not committed.

```text
.
├── agents/
│   └── <agent-id>/
│       ├── contract.json
│       ├── provenance.json
│       └── fragments/*.md
├── workflows/
│   └── <workflow-id>/
│       ├── contract.json
│       └── fragments/*.md
├── skills/
│   └── <skill-id>/SKILL.md
├── schemas/
│   ├── 1.0.0/*.schema.json
│   ├── catalog.json
│   └── migrations/catalog.json
├── runtime/
│   └── src/
│       ├── contracts/
│       ├── profiles/
│       ├── findings/
│       ├── verification/
│       ├── output/
│       ├── registry/
│       ├── routing/
│       ├── context/
│       ├── security/
│       ├── execution/
│       └── drivers/
├── adapters/
│   ├── src/
│   └── targets.lock
├── installer/src/
├── generated/<target-id>/
├── lineage/
│   ├── src/
│   ├── examples/
│   └── curation/
├── eval/
│   ├── runner/src/
│   ├── analysis/
│   ├── programs/
│   │   ├── delegation-assurance/
│   │   └── mahabench/
│   ├── rubrics/
│   └── private/<lane-id>/
├── scripts/
├── tests/
│   ├── contracts/
│   ├── runtime/
│   ├── adapters/
│   ├── installer/
│   ├── lineage/
│   ├── eval/
│   ├── security/
│   └── tooling/
├── docs/
│   ├── superpowers/specs/
│   ├── superpowers/plans/
│   ├── architecture/
│   ├── evidence/
│   └── releases/
└── .github/workflows/
```

## Dependency and Promotion Gates

This table is the authoritative dependency map. P02 and P08 have explicit development and freeze stages so public DEV feedback can improve prompts without changing sealed conditions after freeze.

| Package or stage | Requires | Opens |
|---|---|---|
| G0 — design integration | Written design approval recorded and both specification commits reachable from the target branch | P00 |
| P00 — foundation contract kernel | G0 | P01 and P03 |
| P01 | P00 | P02a, P06, and P08a |
| P02a — initial semantic assets | P00 and P01 | P08a and P10’s public-DEV input |
| P03 | P00 | P04a, P05, P06, and P08a |
| P04a — evidence/verification structure | P00, P01, and P03 | P05, P06, P07, P09a, and P10 |
| P05 | P00, P03, and P04a | Signed recommendation lookup, custody-mechanics testing, P06, P11, P12, and P13 |
| P06 | P01, P03, P04a, and P05 | P07 and P10 |
| P07 | P04a and P06 | P10, P12, and P13 |
| P08a — compiler and development bundles | P00, P01, P02a, and P03 | P09a and P10 DEV execution |
| P09a — lineage and installer mechanics | P00, P04a, and P08a | P10 development execution |
| P10 — shared evaluation substrate and public DEV | P02a, P04a, P06, P07, P08a, and P09a | Viji’s P02b tuning/freeze; later DA and MahaBench programs |
| P02b — final tuning and semantic freeze | P10 public DEV evidence | P04b and P08b, which complete G5 together |
| P04b — frozen renderer language/goldens | P02b and P04a | P11, P12, P13, and the G5 join |
| P08b — frozen release bundles and goldens | P02b and P08a | P09b, P11, P12, P13, and the G5 join |
| G5 — semantic-freeze join | P02b, P04b, and P08b | DA diagnostic CAL and MahaBench PILOT/CAL condition freeze |
| P09b — frozen-bundle installer/lineage rerun | P08b and P09a | G6, P11, P12, and P13 |
| P11 — conformance release | P04b, P05, P08b, and P09b. P10 DEV is a transitive semantic-freeze prerequisite through P02b/P04b/P08b, but its reports are not direct conformance-claim evidence. | Public conformance release independent of live qualification |
| P12 — Delegation Assurance CAL/TEST | G5, P02b, P03, P04b, P05, P06, P07, P08b, P09b, P10, a valid DA custody lane, and complete zero-cost preflight | Exact-scope DA evidence and, only after all gates, routing recommendations |
| P13 — MahaBench external comparisons | P02b, P04b, P07, P08b, P09b, and P10. Sealed TEST additionally requires a separately governed MahaBench custody lane and complete zero-cost preflight. | Reconstruction-complete pilot-only evidence; exact tested confirmatory external-baseline evidence only after G8 |

Host bundles may reach conformance without managed execution or live evaluation. Automatic Council routing cannot activate without verified, signed, scope-matched Delegation Assurance qualification. MahaBench evidence never substitutes for Delegation Assurance evidence.

### Hard gates

| Gate | Required evidence | Opens |
|---|---|---|
| G0 — Design integration | Written approval is recorded and both referenced specifications are reachable from the target branch history. | P00 implementation. |
| G1 — Contract kernel | Cross-platform CI passes strict parsing, RFC 8785 vectors, schema validation, type drift, and migration tests. | All domain contracts. |
| G2 — Evidence integrity | Forged self-attestation, stale evidence, subject/digest mismatch, VT0/VT1-only support, invalid or conflicting VT3, output inflation, and verifier failure cannot create a verified main finding. | Managed findings and forced routing experiments. |
| G3 — Registry and custody mechanics | Qualification envelopes, signer registries, custody-authority onboarding, activation/lease/closure receipts, audit coverage, expiry, revocation, lane separation, and ambiguous inheritance fail closed in deterministic tests. | Recommendation lookup and custody preflight; test keys still cannot activate production. |
| G4 — Managed-runtime safety | Crash recovery, retry idempotency, lease fencing, secret redaction, path containment, resource ceilings, driver isolation, egress, and cancellation pass fault-injection tests. | `managed-conformant` for each exact passing driver. |
| G5 — Semantic freeze | Public DEV and inert-context sensitivity results are frozen; Viji finalizes role boundaries, triggers, evidence/abstention language, renderer templates, cultural provenance, examples, skills, and rubrics; runtime renderer goldens and release bundles regenerate without semantic drift. | DA diagnostic CAL and MahaBench PILOT/CAL condition freeze. |
| G6 — Five-target conformance | Every target compiles deterministically, declares capability loss, installs transactionally, verifies ownership, and rolls back cleanly. | Public conformance release. |
| G7 — Claim-controlled release | Public documentation matches machine-readable capabilities and claim records; absence of live evidence is rendered `unverified`; release artifacts reproduce and verify. | Conformance-only public release. |
| G8 — Confirmatory custody | For the exact evidence lane, an eligible independent authority controls sealed inputs, graders/oracles, execution hardware or tenant, inference account and credentials, keys, lease interval, interim outcomes, and reveal. DA and MahaBench use disjoint governance, manifests, task lineages, and evidence DAGs. | Confirmatory claims for that exact lane only. |
| G9 — Adaptive activation | The exact population and strategy pass the Delegation Value gate, all guardrails, drift checks, freshness policy, and revocation checks. | Automatic strategy recommendation for that exact scope only. |

## Package Sequence

Each package or explicitly named stage receives its own detailed implementation plan immediately before that unit executes, after the interfaces that unit consumes have landed. This prevents downstream plans from inventing APIs that upstream tests later reject. For P02, P04, P08, and P09, the `a` and `b` stages use separate plan files: the `a` plan remains an immutable execution record, while the `b` plan is authored only after its later dependency lands. P00 is detailed now because it consumes only the referenced specifications, G0 approval record, and locked toolchain.

### P00 — Foundation contract kernel

**Detailed plan:** [Foundation contract kernel](2026-08-22-foundation-contract-kernel.md)

**Owner:** Arya

**Produces:** Root toolchain, strict JSON loader, RFC 8785 canonicalization and domain-separated fingerprints, schema catalog, generated TypeScript types, append-only migration kernel, source-invariant checks, and provider-independent cross-platform CI.

**Claim ceiling:** Internal foundation; no compatibility, managed-runtime, or performance claim.

**Exit:** G1 passes on Linux, macOS, and Windows.

### P01 — Council, task, and workflow contracts

**Plan path:** `docs/superpowers/plans/2026-08-22-council-task-workflow-contracts.md`

**Owner:** Arya for schemas and validators; Viji reviews role semantics before P02.

**Produces:** Agent, provenance, task, risk-tier, workflow, handoff, approval, dissent, failure, `AssuranceContract`, and non-negotiable-clause schemas; Abhimanyu Gate state machine; minimum-sufficient-council constraints; canonical manifests for the council roster and eight workflows.

**Consumes:** P00 schema registry, fingerprinting, and migration API.

**Exit:** Contract fixtures cover valid, invalid, downgrade, unknown-field, dissent, approval, and reversible-failure paths.

### P02 — Prompt development, cultural provenance, skills, rubrics, and public assets

**Plan paths:** P02a uses `docs/superpowers/plans/2026-08-22-prompt-cultural-development-assets.md`; P02b uses `docs/superpowers/plans/2026-08-22-prompt-semantic-freeze.md` after P10 DEV lands.

**Owner:** Viji authors and owns the assets and coordinates review; a second named non-author reviewer or a documented public review window supplies any interpretive acceptance required by parent Section 5.3.

**Produces:** Canonical skill files, the semantic content of the `AssuranceContract`, stable non-negotiable clause IDs, full/compact/minimal prompt-pack source components, prompt fragments, trigger contracts, cultural provenance records, transliteration policy examples, workflow prose, public DEV tasks and grading rubrics, README, contributor-facing explanations, and cross-model prompt variants whose semantics remain equivalent.

**Consumes:** P01 role/task/workflow/AssuranceContract schemas and P00 source checks for P02a; P10’s public DEV and inert-context sensitivity results for P02b.

**Exit:** P02a provides versioned development assets to the compiler. After P10 public DEV, Viji performs final tuning and P02b passes prompt lint, role-boundary tests, clause coverage, evidence-language tests, injection fixtures, and cultural review. For every contested-source or persona change, the exit evidence includes either a named non-author interpretive-acceptance record or a documented completed public review window; author self-approval is invalid. P04b and P08b then prove the frozen assets in runtime and host projections to complete G5. Arya may review mechanics but does not author Viji’s public contribution. Later documentation-only edits cannot alter executable semantics or user instructions.

### P03 — Material profiles, safe probes, and populations

**Plan path:** `docs/superpowers/plans/2026-08-22-material-profile-probes.md`

**Owner:** Arya

**Produces:** Material profile and probe-observation schemas, deterministic normalization, capability dimensions, safe probe planner, probe cache/freshness rules, population membership, exact-match policy, and non-inheritance tests.

**Consumes:** P00 contract kernel.

**Exit:** Display-name changes do not affect equality; capability changes do; missing observations degrade conservatively; ambiguous population membership is rejected.

### P04 — Evidence, verification, and verified output

**Plan paths:** P04a uses `docs/superpowers/plans/2026-08-22-evidence-verification-structure.md`; P04b uses `docs/superpowers/plans/2026-08-22-frozen-output-renderer.md` after P02b lands.

**Owner:** Arya for contracts, gates, renderer grammar, escaping, ordering, and enforcement. Viji owns the human-visible fixed template language through P02a/P02b; P04 does not author that prose.

**Produces:** Finding, evidence-receipt, verification-request, verifier-receipt, artifact-subject, falsification, adjudicator-registry, adjudication-request, adjudication-receipt, and output schemas; evidence gate; verification gate; deterministic verified-output renderer; isolated Verifier Runner; exact VT0/VT1/VT2/VT3 semantics; signature, independence, conflict, expiry, and revocation checks for VT3.

**Consumes:** P00 fingerprints and schemas, P01 task/AssuranceContract bindings, and P03 material profiles for P04a. P04b consumes Viji’s P02b template assets and reruns byte-stable renderer goldens after semantic freeze.

**Exit:** P04a reaches G2, including explicit adversarial tests for self-authored receipts, stale evidence, subject mismatch, digest mismatch, VT1-only support, path escape, output spoofing, invalid/conflicting VT3 receipts, lost adjudicator independence, and run-level verifier failure. P04b later proves Viji’s frozen templates cannot add, inflate, or detach a factual claim.

### P05 — Signed qualification registry and custody boundary

**Plan path:** `docs/superpowers/plans/2026-08-22-qualification-registry-custody.md`

**Owner:** Arya for registry and custody-control code. Independent authorities, onboarding reviewers, and audit-sink operators own only the real operational evidence they actually produce.

**Produces:** Compatibility, evidence, recommendation, revocation, expiration, drift, and inheritance records; `QualificationSignerRegistry`; Ed25519 signer/verifier interfaces; qualification envelopes and full evidence-DAG resolver; `CustodyAuthorityRegistry`; authority proposal and two-reviewer onboarding receipts; audit-sink and interval-coverage records; `CustodyLease`, `CustodyActivationReceipt`, and `CustodyClosureReceipt`; quota/no-overage evidence bindings; the controller-owned append-only qualification/custody registry store and its storage-abstraction tests; atomic activation; lane-scoped IDs that reject DA/MahaBench evidence reuse; scope resolver; and fail-closed activation policy.

**Consumes:** P00 canonical bytes and migrations; P03 profiles; P04a receipts and adjudicator registry.

**Exit:** G3. Self-onboarded, shared-principal, unsigned, incomplete-DAG, public-projection, cross-lane, audit-gapped, expired, revoked, candidate-authored, or ambiguously inherited records cannot route. Test keys and synthetic custody fixtures prove mechanics only and cannot activate a production recommendation.

### P06 — Strategy routing, context, and handoff integrity

**Plan path:** `docs/superpowers/plans/2026-08-22-routing-context-handoff.md`

**Owner:** Arya for selector and contracts; Viji reviews human-facing route explanations.

**Produces:** Task characterizer; study contracts for B0, I0, I1, S0, S1, C0, C1, and adaptive A; production candidates I0/I1/S1/C1; deterministic least-complex policy engine; pre-execution routing receipt; context-package builder; typed handoffs; receipt binding; handoff-loss assay; fallback, `do-not-route`, and abstention paths.

**Consumes:** P01 task contracts, P03 profiles, P04a evidence gate, and P05 recommendation lookup.

**Exit:** Before qualification, automatic Council routing returns `none`, while B0 remains only the host/user’s explicit outside-Council behavior and an evaluation comparator; any Council strategy is explicitly `forced-experimental`. I0 becomes the incumbent only after its mandatory exact-scope gates pass; if I0 fails, the profile is `do-not-route`. Every automatic Council route, including I0, requires an exact active recommendation and is reproducible from recorded pre-execution state.

### P07 — Secure durable controller and execution drivers

**Plan path:** `docs/superpowers/plans/2026-08-22-controller-drivers-recovery.md`

**Owner:** Arya

**Produces:** Controller state machine, append-safe run journal, leases and fencing, idempotent resume, retry taxonomy, cancellation, timeouts, bounded output, secret redaction, path containment, network policy, a release-signed out-of-process driver broker, OpenAI-compatible driver, Ollama driver, separately pinned Codex CLI, Claude Code CLI, and Kiro CLI drivers, the adversarial driver conformance harness, and the rootless Podman VT2 backend.

**Consumes:** P04a verification API and P06 routing/context contracts.

**Exit:** G4. A backend earns only the tier it demonstrates; driver failure cannot manufacture a successful task or evidence receipt. Any CLI surface that fails or cannot expose a required managed control remains native/install-only and is excluded from managed claims.

### P08 — Compiler and five host BundleAdapters

**Plan paths:** P08a uses `docs/superpowers/plans/2026-08-22-development-compiler-host-adapters.md`; P08b uses `docs/superpowers/plans/2026-08-22-frozen-release-bundles.md` after P02b lands.

**Owner:** Arya for compiler/adapters; Viji supplies P02a development assets and later P02b frozen assets.

**Produces:** Canonical compiler IR, capability negotiation, deterministic target manifests, full/compact/minimal prompt-pack projections, stable-clause coverage maps, Claude Code adapter, Codex adapter, Copilot/VS Code adapter, Kiro CLI adapter, Kiro IDE adapter, downgrade declarations, golden bundles, and target lockfile.

**Consumes:** P01 canonical manifests, P02 prompt assets, P03 capability contracts, and P00 deterministic source rules.

**Exit:** P08a provides deterministic development bundles for P10 DEV. After P02b freezes, P08b regenerates every projection, clause map, manifest, and golden; each adapter passes independent schema, golden, path, determinism, and declared-loss tests. P08b proves adapter compatibility; G6 is reached only after P09b installs and reverses those exact frozen bundles.

### P09 — Lineage and transactional installer

**Plan paths:** P09a uses `docs/superpowers/plans/2026-08-22-lineage-installer-mechanics.md`; P09b uses `docs/superpowers/plans/2026-08-22-frozen-bundle-installer-rerun.md` after P08b lands.

**Owner:** Arya for implementation; Viji authors installation and privacy documentation.

**Produces:** Lineage record classes, consent/redaction/retention policy enforcement, retrieval boundaries, install manifest, dry run, backup, atomic write, ownership marker, conflict detection, rollback, update, uninstall, and post-install verification.

**Consumes:** P00 migrations, P04a evidence subjects, and P08a generated manifests for P09a; P08b frozen manifests for P09b.

**Exit:** P09a proves lineage and installer mechanics against development bundles. P09b reruns plan/install/update/uninstall/doctor, crash, ownership, conflict, provenance, and byte-identical rollback tests against every exact P08b frozen bundle; together with P08b it completes G6.

### P10 — Shared evaluation substrate and public DEV loop

**Plan path:** `docs/superpowers/plans/2026-08-22-evaluation-substrate-dev-loop.md`

**Owner:** Arya for launcher, isolation, manifests, randomization primitives, artifact capture, and locked analysis libraries; Viji for public DEV tasks, rubrics, interpretation, and prompt revisions.

**Produces:** A native launcher; disposable-workspace isolation; condition, task-family, quota-cost, freeze, custody-lane, and artifact schemas; provenance clustering; deterministic graders; resource accounting; sanitization and allowlisted publication; failure/retry taxonomy; intention-to-treat primitives; assignment schedules; bootstrap/randomization/exact-bound libraries; reproducible Python reports; separate `eval/programs/delegation-assurance/` and `eval/programs/mahabench/` namespaces; and the public DEV/inert-context loop used only for prompt and harness development.

**Consumes:** P02a rubrics, P04a evidence receipts, P06 strategies, P07 drivers, P08a development bundles, and P09a lineage/privacy boundaries.

**Exit:** Public DEV artifacts reconstruct, are visibly non-confirmatory, and give Viji the permitted feedback for P02b. Synthetic sealed-block fixtures validate mechanics without producing a recommendation or external performance claim. Better performance is an outcome later studies may or may not show, never an acceptance condition imposed on the data.

### P11 — Claim-controlled CI, documentation, and conformance release

**Plan path:** `docs/superpowers/plans/2026-08-22-claims-docs-conformance-release.md`

**Owner:** Viji for README and all public docs; Arya for claim registry mechanics, CI, provenance, SBOM, release automation, and artifact signing.

**Produces:** Claim registry and linter, compatibility matrix, evidence matrix, architecture and threat-model docs, limitations, reproducibility guide, contribution guide, release manifest, checksums, SBOM, provenance, and conformance release notes.

**Consumes:** P04b frozen renderer, P05 registry/custody mechanics, P08b target conformance, and P09b installer/lineage conformance. P10 DEV is already a transitive prerequisite of those frozen artifacts through P02b, but P10 developmental reports are not direct claim evidence and no live qualification result gates a conformance-only release.

**Exit:** G7 after G6 plus documentation review. The initial matrix marks performance `unverified` wherever confirmatory evidence is absent.

### P12 — Conditional Delegation Assurance qualification and adaptive-policy evidence

**Plan path:** `docs/superpowers/plans/2026-08-22-delegation-assurance-live-operations.md`

**Owner:** Arya authors the harness and may operate public DEV only. Viji reviews public interpretation but receives no task-level DA-CAL outcomes while unchanged conditions remain eligible for DA-TEST. An eligible independent DA custody authority exclusively administers sealed CAL/TEST hardware or service tenant, inference account and credentials, plaintext, graders/oracles, execution, audit-covered leases, interim outcomes, and controlled reveal; Arya and Viji have no administrative or remote-access path during custody.

**Produces:** A signed diagnostic freeze; the disjoint seven-condition diagnostic CAL (168 top-level sessions per population); exact I0 qualification and conditional I1 promotion with its 144-session TEST; each required S1/C1 leaf’s 48-session CAL, 144-session TEST, and separately counted handoff sidecars; final adaptive-policy fingerprint; disjoint 72-session policy CAL; 216-session three-arm B0/I*/A operational TEST; signed quota, activation, execution, reveal, closure, analysis, and recommendation records; drift monitoring; expiry; revocation; and a reconstruction-complete public evidence package.

**Consumes:** P02b frozen semantics, P03 material profiles/populations, P04b verification/VT3/renderer, P05 DA custody/qualification mechanics, P06 strategy/handoff contracts, P07 drivers/controller, P08b frozen bundles, P09b privacy/lineage boundaries, and P10 evaluation substrate. P11’s conformance release is independent and is not a prerequisite.

**Exit:** P12 may complete with a valid G8 DA evidence lane and a reconstruction-complete positive, negative, or inconclusive result. G9 opens only for each exact strategy/population scope that passes Delegation Value and every guardrail; otherwise the bounded result is published and automatic Council routing remains disabled. If independent administration, two-reviewer onboarding, uninterrupted audit coverage, quota/no-overage proof, exact host/model observability, or complete block capacity is absent, sealed execution does not start and no recommendation is emitted.

### P13 — Separate MahaBench external-comparison program

**Plan path:** `docs/superpowers/plans/2026-08-22-mahabench-external-comparisons.md`

**Owner:** Arya authors the runner, manifests, and analysis and may operate public DEV/PILOT work permitted by the parent design. Viji authors public rubrics and post-reveal documentation. A separately governed MahaBench custodian controls TEST plaintext, task-specific graders, oracle validation, keys, sealed execution inputs, and reveal without prompt-lane access.

**Produces:** The parent design’s unchanged engineering comparison (24 families × 3 repetitions × 2 arms = 144 sessions, Council versus an exact pinned public Compound Engineering release); knowledge comparison (24 × 3 × 2 = 144 sessions, Council versus clean Codex native/default); optional Kiro CLI and Copilot/VS Code transportability comparisons (12 families × 2 repetitions × 2 arms = 48 sessions per surface) against their exact clean defaults; separate 12-family, two-repetition-per-arm PILOT/CAL blocks; paired assignment; feasibility and power report; two independently analyzed domain endpoints; sanitized run artifacts; and reconstruction-complete positive, negative, or inconclusive reports.

**Consumes:** Public PILOT consumes P02b frozen semantics, P04b verification/renderer, P07 drivers, P08b bundles, P09b privacy/installer boundaries, and P10 evaluation substrate. Sealed TEST additionally consumes a MahaBench custody lane whose tasks, generators, source packets, manifests, accounts, leases, and evidence DAG are disjoint from Delegation Assurance, plus a complete zero-cost preflight.

**Exit:** P13 may complete with a reconstruction-complete pilot-only report when feasibility, power, no-cost capacity, or custody blocks TEST. G8 and confirmatory external-baseline claims open only for an eligible sealed MahaBench TEST lane; reconstruction-complete positive, negative, and inconclusive TEST outcomes are equally valid release results. Every result remains bounded to the exact tested host, client, model, condition, task population, baseline, budget, and endpoint and never activates Council routing.

## Ownership Matrix

| Artifact class | Arya | Viji | Independent custodian |
|---|---:|---:|---:|
| Core schemas, hashing, migrations | Responsible | Consulted | — |
| Runtime, security, drivers, recovery | Responsible | Consulted on user-visible behavior | — |
| Compiler, adapters, installer, CI | Responsible | Consulted on generated language | — |
| Prompt language and fine-tuning | Mechanical review | Responsible | — |
| Cultural provenance and examples | Contract support | Responsible | — |
| README and all public documentation | Technical fact check | Responsible | — |
| Public rubrics | Runner integration | Responsible | Consulted for sealed use |
| Public DEV operation | Responsible for harness and reproducibility | Responsible for tuning, rubrics, and interpretation | — |
| DA sealed CAL/TEST administration | No execution-account, plaintext, interim-outcome, or remote-admin access | No task-level CAL/TEST access before permitted reveal | DA authority is responsible for hardware/tenant, account, credentials, execution, graders/oracles, leases, and reveal |
| MahaBench sealed TEST administration | No TEST plaintext or task-specific grader access before reveal | No TEST plaintext or task-specific grader access before reveal | Separately governed MahaBench custodian is responsible |
| Release mechanics and artifacts | Responsible | Documentation approval | Signs evidence when applicable |

## Claim Ceiling by Milestone

| Milestone | Permitted statement | Prohibited interpretation |
|---|---|---|
| P00–P04 complete | “The core contracts and evidence gates pass deterministic tests.” | The agents are useful, compatible, secure in production, or better than a baseline. |
| P05–P07 complete | “The managed runtime passes its published provider-independent conformance and fault-injection suite.” | Every provider/model is supported or performance-qualified. |
| P08b–P09b complete | “These exact host targets compile and install according to the published conformance matrix.” | The same bundles perform equally across hosts or models. |
| P10 public DEV complete | “In this published development sample, the observed diagnostics were …” | General superiority, sealed confirmation, or recommendation for any profile. |
| P11 conformance release | “This release is mechanically conformant for the listed versions and limitations; performance is unverified.” | Automatic Council routing is justified. |
| P12 DA confirmatory complete | “For this exact signed population, strategy, material profile, budget, date window, and DA estimand, the result was …” | External-system superiority or a universal claim across models. |
| P13 MahaBench confirmatory complete | “For this exact tested host, client, model, domain, external baseline, task population, budget, and endpoint, the result was …” | Delegation itself caused the result or automatic routing is justified. |

## Cross-Host and Cross-Model Policy

1. Host support is versioned by adapter target and native manifest shape.
2. Provider execution is versioned by driver protocol and observed feature behavior.
3. Model capability is represented by the material profile and safe probe observations, not a brand-name allowlist.
4. Backdated and open-weight models are eligible when they meet the same observable capability floor and verification contract.
5. A weaker model may receive a smaller prompt, narrower task, explicitly forced I0 experiment, qualified I0 route, or abstention. It does not receive a lower truth standard.
6. Prompt variants may differ in compression and scaffolding, but role boundaries, evidence requirements, stop conditions, and output semantics must remain invariant.
7. Qualification inheritance is explicit, conservative, and testable. Missing or ambiguous scope fails closed.

## Evaluation Programs and Comparators

The two normative programs are disjoint. They do not share task families, generators, source packets, sealed manifests, evidence DAGs, or headline estimands.

### Delegation Assurance

- **B0:** bare native inline behavior without the Council universal envelope.
- **I0:** `U`, the managed universal task/evidence/permission envelope without task skill content.
- **I1:** `U + K`, adding the task/domain skill.
- **S0:** `U + R_S`, specialist mechanics without `K`; diagnostic only.
- **S1:** `U + R_S + K`, the skill-bearing specialist candidate.
- **C0:** `U + R_C`, Council mechanics without `K`; diagnostic only.
- **C1:** `U + R_C + K`, the skill-bearing Council candidate.
- **A:** the frozen adaptive policy selecting only independently qualified production leaves.

Diagnostic CAL compares all seven forced conditions. I0 must first pass its mandatory gates; I1 can become `I*` only after its exact confirmatory comparison with I0. Every complexity-adding reachable leaf qualifies against its named simpler incumbent. The operational TEST compares exactly B0, `I*`, and A and supports only the preregistered `A − I*`, `A − B0`, and `I* − B0` claims.

### MahaBench-PoC-1

- **Engineering:** Council managed mode versus an exact pinned public Compound Engineering release using its documented self-routing entry point.
- **Knowledge:** Council managed mode versus clean Codex native/default with no Council, Compound Engineering, user agent, or user skill customization.
- **Transportability:** Kiro CLI and Copilot/VS Code Council bundles versus exact pinned clean defaults on their separately declared subsets. These results are never pooled with Codex or each other.

MahaBench’s engineering and knowledge estimands remain separate; a generic invented subagent condition does not replace either governed baseline. Beating an external baseline cannot prove that delegation added value over inline execution, and DA evidence cannot prove superiority to an external framework.

Within each program, budgets are matched according to its frozen manifest: tool permissions, elapsed-time ceiling, model access, context inputs, token/accounting method where observable, retries, internal/sidecar calls, verifier opportunity, and quota source. Failed, timed-out, malformed, and abstained runs remain intention-to-treat outcomes. The primary estimands and guardrails freeze before sealed execution.

## Branch and Pull-Request Sequence

1. Record explicit written design approval and integrate the specification branch into the target branch through its existing pull request.
2. Rebase or recreate `plans/foundation-roadmap` from that target so the plans do not merge the specification indirectly.
3. Open the planning pull request containing only this roadmap and P00’s detailed plan.
4. Execute P00 on an Arya-owned implementation branch and pull request.
5. Create each later Arya-owned package or named-stage branch only after its consumed interfaces land.
6. Create Viji-owned P02a, P02b, and documentation changes as substantive Viji branches and pull requests at their declared dependency points.
7. Preserve each contributor’s real commit identity and review trail; do not split one person’s change into synthetic authorship.
8. Keep CLI authentication changes scoped to the operation that needs them and verify the active account before any push. Do not alter desktop account-switcher state as a side effect.

## Plan Authoring Order

Before a package or named stage begins, that unit’s detailed plan must contain:

- exact file paths and exported interfaces;
- failing test code before implementation code;
- deterministic commands with expected failure and success signals;
- negative, mutation, recovery, and platform cases appropriate to the risk;
- schema and migration effects;
- security and privacy boundaries;
- claim-registry effects;
- ownership and commit boundaries;
- a final spec-to-test traceability table.

The next plan after P00 is P01. P03 may be planned in parallel once P00 interfaces are stable. P02a is planned and executed after P01’s role and workflow schema review; P08a is separately planned and compiles its development assets; P10 runs public DEV; only then is Viji’s P02b plan authored for final tuning and semantic freeze. P04b and P08b each receive their own post-P02b plan for runtime/host goldens, and P09b receives its own post-P08b plan for lifecycle safety on the frozen bundles. No `b`-stage implementation steps are guessed inside an earlier `a`-stage plan. P11 can release conformance after P04b, P05, P08b, and P09b even when P12 and P13 never start.

## Roadmap Completion Criteria

- Every package exit gate has machine-readable receipts and human-readable limitations.
- Five host adapters produce deterministic native bundles and declare every degradation.
- At least one local/open-weight route and one OpenAI-compatible route pass driver conformance without implying model quality.
- No unverified finding is rendered as verified.
- No automatic Council route, including I0, occurs without an active, exact-scope, signed recommendation.
- The installer proves dry-run, ownership, conflict, rollback, update, and uninstall safety.
- Public DEV, DA-CAL/TEST, MahaBench PILOT/CAL/TEST, and conformance evidence are visibly separated in files, custody governance, UI language, and claims.
- README and public documentation are authored on Viji’s substantive branch and match the machine-readable claim registry.
- The first public release may be conformance-only; a performance release occurs only if the data supports it.
