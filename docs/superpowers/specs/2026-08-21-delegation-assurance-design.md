# Mahabharata Council: Delegation Assurance Design

**Status:** Proposed — assurance-reviewed; written specification awaiting approval

**Date:** 2026-08-21

**Repository:** `callmearya/mahabharata-council`

**License:** Apache License 2.0

**Relationship:** Normative addendum to `2026-08-21-mahabharata-council-design.md`

## 1. Purpose

Mahabharata Council must not treat a skill, subagent, named persona, larger model, higher reasoning setting, or deeper council as inherently better than direct inline work. Delegation can add useful specialization and challenge, but it can also lose context, amplify correlated mistakes, consume more resources, and produce impressive-looking findings without verification.

This addendum defines a model-, vendor-, host-, task-, and persona-neutral Delegation Assurance Layer. It selects the least complex execution strategy that is mechanically safe and empirically valuable for the current task and observable execution capabilities. It prevents unsupported claims from being promoted as findings and measures whether the adaptive routing policy gives users more value than the strongest inline alternative.

Named products or models may appear in test manifests and receipts for reproducibility. They are never the architectural unit of support or a proxy for quality.

## 2. Goals and non-goals

### 2.1 Goals

1. Make inline execution the explicit counterfactual for every skill or delegation claim.
2. Choose among inline, inline-with-skill, specialist, and Council strategies using task fit, capability evidence, and qualification results.
3. Require resolvable evidence and an appropriate verification action before a substantive claim appears as a verified finding.
4. Make “no verified findings” and calibrated abstention valid successful outcomes.
5. Qualify content-addressed material profiles plus time-bounded probe observations rather than hard-coded model or vendor names.
6. Permit new, old, hosted, local, proprietary, and open-weight inference systems to enter through the same capability probes and conformance rules.
7. Expose context loss, false findings, verification coverage, routing mistakes, latency, tokens, tool use, and retries separately.
8. Default to the least complex strategy that passes safety, correctness, handoff, and practical-value gates.
9. Invalidate recommendations when a material model, host, prompt, skill, adapter, tool, permission, or budget input changes.
10. Preserve the zero-incremental-cost and honest-claims policies from the parent design.

### 2.2 Non-goals

- Guaranteeing identical quality across models, hosts, or hardware.
- Claiming that compatibility or successful installation proves task performance.
- Treating subagent agreement, persona authority, confidence language, or longer reasoning as verification.
- Forcing delegation to justify the project’s theme.
- Combining correctness, false findings, and resource cost into one opaque “Council score.”
- Automatically downloading, redistributing, or relicensing model weights.
- Enabling paid inference, metered fallback, or network access without explicit authorization.
- Collecting hidden chain-of-thought to judge quality or handoff fidelity.
- Generalizing a recommendation from one material profile to an entire model family, provider, host, or capability tier without a separate generalization study.

## 3. Design principles

### 3.1 Least-complex adequate execution

The router starts with inline execution and adds instructions, specialization, or orchestration only when a more complex strategy is eligible and its exact record proves a predeclared practical benefit over the named simpler comparator. “Least complex” applies among strategies that satisfy the selected recommendation contract; it does not discard a more complex strategy that the same confirmatory record proved materially better. Equivalent or ambiguous evidence selects the simpler mode.

### 3.2 Evidence is not prose

A generated explanation, citation-shaped string, or second agent’s agreement is not evidence. Evidence must resolve against the frozen task state. Verification must test the exact claim using a suitable deterministic predicate, artifact inspection, execution trace, or independently reviewable source.

### 3.3 Verification before promotion

Models may generate hypotheses. The runtime controls whether those hypotheses become findings. A claim that fails evidence or verification checks is rejected or quarantined as a hypothesis; it is never repaired by merely asking another model to agree.

### 3.4 Capability over identity

Routing code uses observable capabilities and task features. Provider, model, host, and inference-setting identifiers remain in receipts and qualification fingerprints so results are reproducible, but names are not capability assumptions.

### 3.5 Scoped evidence

Qualification is always scoped to an exact `ExecutionCandidateFingerprint`, every selected leaf `MaterialProfileFingerprint` for an adaptive candidate, a declared target-population specification, workflow/risk stratum, and budget regime. The sample manifest used to test that population is recorded separately. “Recommended” without this scope and an observable population-membership rule is invalid.

### 3.6 Abstention-aware quality

Accuracy-only evaluation rewards guessing, while precision alone rewards silence. Every finding workflow reports precision, recall where an oracle exists, false discoveries, abstention, coverage, and task outcome together.

### 3.7 Operational honesty

Resource-matched experiments answer whether delegation itself adds value under equal budgets. Operational experiments answer whether the shipped configuration works better in practice. The project never substitutes one claim for the other.

## 4. Core terminology

### 4.1 Material profile and probe observations

A `ProfileIntent` is the pre-resolution request: driver and endpoint configuration, requested inference identity, authorization references without secret values, intended tools/permissions/egress, host surface, and condition inputs. It has its own intent hash but makes no resolved-identity or compatibility claim. The driver resolves the intent and emits an identity receipt before the controller constructs the material profile.

A `MaterialProfile` is the canonical, content-addressed description of inputs that can change execution semantics, authorization, privacy eligibility, or resource behavior. Its `MaterialProfileFingerprint` is the canonical hash of three explicit subprofiles:

1. `ExecutionEnvironment`, whose `ExecutionEnvironmentFingerprint` covers canonicalization version; requested and resolved inference identity; identity assurance and provenance; provider protocol and endpoint-origin class; runtime, driver, driver-signer/provenance, driver-sandbox backend/policy, host, client, extension, harness, Council-runtime, and BundleAdapter builds; quantization, chat-template, tool parser, inference settings, seed availability, context/output ceilings; base tool versions and permissions; operating-system and hardware class; and runtime/weight license metadata.
2. `AuthorizationAndPolicyFingerprint`, containing a locally keyed pseudonym for account/tenant scope; `credentialBindingMode` and `credentialScopeClass` without credential material; the exact `originEnforcement`, `credentialContainment`, and `payloadEnforcement` dispositions; subscription/plan and quota class; region/residency; retention, training, and telemetry policy; organization feature/tool entitlements; provider data-policy version; and authorization expiry.
3. `ConditionFingerprint`, containing strategy; prompt, skill, tool-schema, workflow, and leaf-local orchestration hashes; condition-specific tools, permissions, egress rules, and delegation mode; resource budget and allocation policy; and fallback policy. It excludes the outer adaptive feature extractor, router, ordered branch map, and enable/disable state.

Raw account, tenant, and credential identifiers never enter a public artifact. Local keyed pseudonyms prevent accidental evidence sharing across accounts while public reports expose only the policy/entitlement classes needed to interpret the result.

The pseudonymous account/tenant binding is a non-transferable local security boundary. An exact-profile recommendation therefore routes only on the account used to qualify it. Cross-account performance portability is a separate, optional construct: a `PerformancePolicyEquivalenceClassFingerprint` covers resolved inference identity and assurance level; provider protocol; runtime, host/client, driver/sandbox, parser, and inference settings; plan/quota, region, retention/training/telemetry, entitlement, `credentialBindingMode`, `credentialScopeClass`, `originEnforcement`, `credentialContainment`, `payloadEnforcement`, tool/permission, context, budget, and data-policy classes; and every other performance- or safety-material field, while excluding only the keyed account pseudonym and credential material.

No equivalence class exists by assertion. A preregistered multi-account study treats independently administered accounts as the top-level clusters, uses a power-derived account count that is never fewer than five, keeps at least two entire accounts sealed from CAL and class-definition decisions, and retains 24 independent task clusters per confirmatory account/block. The class gates must pass in the account-cluster model, on each held-out account without pooled rescue, and under a frozen heterogeneity ceiling. All accounts use the same class rule and independently owned credentials; no credential is shared. For IA1 identities, the portable validity expires at the earliest member `StudyWindow` deadline or canary invalidation.

A new account may activate a signed portable class envelope only after constructing its exact local `MaterialProfile`, matching every class field, passing fresh local capability/privacy/egress probes and canaries, and receiving a controller-signed `ClassActivationReceipt`; the local pseudonym and authorization remain part of every run. Any mismatch selects inline and requires new evidence. Without the powered held-out multi-account study, hosted results remain exact-account recommendations for the custody authority or historical public evidence and cannot drive automatic routing for authors or other users.

Comparative studies pair the execution environment while intentionally varying frozen condition fingerprints. Every study carries an `AllowedDifferencesManifest` listing the treatment fields permitted to differ; any unlisted difference invalidates the pair.

An adaptive arm is represented by an `AdaptivePolicyFingerprint`, the canonical hash of the outer router and feature extractor; ordered predicate-to-leaf map; every referenced leaf `MaterialProfileFingerprint`; eligibility, fallback, exit, and budget rules; and policy version. Its inline leaf is the frozen `I*`; every other reachable leaf is a separately named adaptive branch. A `LeafQualificationProgram` is the evidence program for any exact complexity-adding `I1`, `S1`, or `C1` leaf, regardless of whether that leaf later becomes `I*` or an adaptive branch. `I0` is the foundational managed-inline baseline, not a `LeafQualificationProgram`: its diagnostic CAL is governed by the `DiagnosticFreezeManifest`, and any confirmatory `I0 − B0` observation is governed by the later `PolicyFreezeManifest` and the frozen `DA-TEST-O-1` block. It cannot receive a production recommendation before that TEST passes. `ExecutionCandidateFingerprint` means either a leaf material-profile fingerprint or an adaptive-policy fingerprint. Every adaptive run records both the policy fingerprint and the selected leaf fingerprint; a missing, reordered, enabled, disabled, or changed leaf creates a different policy without changing any unchanged leaf fingerprint or invalidating that leaf's independent evidence.

The fingerprint excludes probe timestamps, probe-suite versions, probe results, latency samples, run IDs, expiry state, and qualification status. Those are observations made after the material configuration exists.

A `ProbeObservation` is an append-only record keyed by:

```text
MaterialProfileFingerprint
× probe-suite hash
× observed capability
× observation time
```

It records the declared capability, disposition, enforcement source, measured probe result, raw-receipt hash, expiry, and invalidating conditions. Repeating a probe creates a new observation, not a new material profile. An `ExecutionProfileView` joins one material profile with the exact unexpired observation IDs used for a routing decision. Missing, expired, or conflicting observations fail closed to `unknown` or the lower proven tier.

Every material profile records both requested and resolved identity with one assurance level:

| Level | Evidence | Maximum use |
|---|---|---|
| `IA0-requested` | User-facing alias only; serving artifact is unknown | Forced experimental execution only; no reusable qualification |
| `IA1-runtime-reported` | Host or provider reports a resolved model/build without proving immutability | A sealed run-window claim and short-lived scoped recommendation only under the `StudyWindow` controls below |
| `IA2-immutable-pinned` | Documented immutable hosted snapshot or locally computed artifact digest bound to runtime, template, and parser | Exact-profile confirmatory qualification permitted |
| `IA3-provenance-attested` | IA2 plus a verified signature, trusted manifest, or equivalent attestation | Artifact-origin or redistribution-integrity claims permitted |

A provider-reported identity remains provenance-labeled evidence rather than an independently verified fact. IA1 evidence requires a signed `StudyWindow` committed before TEST with first-run deadline, block-completion deadline, `valid_until`, exact identity/client canaries, probe cadence, and immediate invalidation triggers. `valid_until` is no later than seven days after block completion and no later than 21 days after the first TEST session. Automatic routing is permitted only inside that interval, within the qualified population, after canaries pass before each production block and at least daily. Provider notice, reported-identity/client change, failed capability canary, host update, fallback, parser/tool drift, or missed cadence makes the recommendation stale immediately. Canaries reduce observable drift risk; they do not prove hidden artifact identity.

A resolved-identity change between probing, routing, and execution stops promotion, creates a new material profile, and requires fresh probes. The historical run-window claim remains scoped to its observed interval after routing validity expires. IA3 is not required for ordinary private inference.

### 4.2 Strategy

The production strategy ladder is:

| ID | Strategy | Description |
|---|---|---|
| `I0` | Managed inline | Host agent works directly with only the universal task, evidence, abstention, and receipt envelope; no domain skill or persona content |
| `I1` | Inline + skill | Same inline context plus the relevant Council skill and verification contract |
| `S1` | Specialist | One bounded specialist receives the task contract, evidence packet, and same skill content |
| `C1` | Council | The smallest eligible multi-role workflow, subject to the parent design’s parallelism and review ceilings |

DEV and DA-CAL may also run `S0` and `C0`, which preserve the universal envelope and minimal role/handoff scaffolding while removing Council skill content at the corresponding orchestration depth. The resulting `3 × 2` component ablation diagnoses bundled mode differences; it is not described as a factorial isolation of skill or orchestration because role prompts, packaging, synthesis, and compute also change. Production recommendations use the four shipped strategies.

`B0` denotes a bare native-host inline control outside the managed production ladder. It receives only the user task and host defaults and is used to measure the value of the universal assurance envelope itself. It never becomes an automatic Council routing target or a positive production recommendation. If `B0` beats or invalidates the managed envelope, the applicable managed candidate receives `do-not-route`; native-host bypass remains an explicit user/host action outside Council. The adaptive production policy is compared with both `B0` and the frozen managed inline incumbent `I*`; external native-default and Compound Engineering comparisons remain separate MahaBench blocks.

### 4.3 Finding

A `Finding` is an atomic, externally checkable claim with scope, severity, state, evidence locators, verification receipts, uncertainty, and invalidating conditions. Finding states are:

- `verified`
- `hypothesis`
- `rejected`
- `stale`
- `not-verifiable`

Only `verified` findings appear in the main findings section. Hypotheses may appear in a separately labeled investigation queue when useful; they never affect verified-finding metrics or headline conclusions.

An `EvaluatedClaim` is a condition-blind, post-run scoring object used to compare factual assertions from every arm, including unstructured `B0`. Its labels are `supported`, `unsupported`, `unverifiable`, or `out-of-scope`. An `unsafe assertion` is an unsupported claim or an unverifiable claim presented as a factual conclusion; an explicitly labeled hypothesis, uncertainty, or abstention is not a factual assertion. Evaluation labels never change a runtime `Finding` state or grant verification authority.

### 4.4 Qualification scope and population membership

Qualification keys have the form:

```text
ExecutionCandidateFingerprint
× TargetPopulationSpec hash
× workflow and risk stratum
× resource-budget regime
```

A `TargetPopulationSpec` declares observable membership predicates, included and excluded workflows, risk ceilings, evidence requirements, context ranges, out-of-distribution rules, representative stratum prevalence, and any analysis weights. An `EvaluationManifestHash` separately identifies the sampled families, fixtures, strata, and graders used to estimate performance. This distinction permits a new real task to use a validated stratum without pretending it was a benchmark item.

The routing key names the candidate leaf or adaptive-policy fingerprint. Its evidence record additionally carries every selected leaf, every comparator material-profile fingerprint, and the `AllowedDifferencesManifest`, so a recommendation cannot hide changes in model, host, permissions, budgets, or other non-treatment fields.

The deterministic membership rule is frozen before TEST and evaluated on a metadata-only suite of 48 held-out families: 24 adjudicated in-scope and 24 boundary/out-of-distribution. Every classification must match before automatic delegation is enabled; these fixtures require no inference sessions. The 24 TEST families must also classify in-scope before any arm begins. A predicate, range, or feature-extractor change creates a new population hash and requires a new held-out suite.

Automatic delegation is allowed only when the current task satisfies a validated predicate. Unmatched or ambiguous tasks stay inline or may run as explicitly forced experimental work. Repetitions estimate stochastic variability; task families or declared clusters are the population units.

Every DA-CAL and DA-TEST manifest carries a signed family-provenance graph and similarity audit before plaintext execution. Units called independent may not share a base repository commit, source packet, generator lineage, or dominant expected failure mechanism. Near-duplicate prompts, mutations, repositories, or sources fail the independence audit even when their hashes differ. Any unavoidable sharing forms one declared cluster; assignment, randomization, resampling, power, and effective `N` move to that cluster level. Fixed-count core blocks select at most one family from each cluster; related extras may be published descriptively but cannot increase `N`. CAL therefore requires 12 audited independent clusters and confirmatory TEST requires 24, not 12 or 24 labels inside fewer lineages, and the power report models the frozen cluster structure.

## 5. Delegation Assurance architecture

The managed runtime adds nine isolated components:

1. **Task Characterizer:** derives observable task features and risk without using outcomes or sealed-test knowledge.
2. **Capability Probe:** resolves the current `MaterialProfile` and emits append-only `ProbeObservation` records.
3. **Eligibility Filter:** removes strategies that cannot satisfy required permissions, output contracts, evidence access, or budgets.
4. **Strategy Selector:** chooses the least complex strategy supported by the qualification registry and current task predicate.
5. **Context Packager:** constructs a hash-linked task contract and evidence packet without lossy implicit context.
6. **Evidence and Verification Gate:** resolves evidence, executes or validates verification predicates, and assigns finding state.
7. **Verifier Runner:** executes allowlisted predicates in a controller-isolated snapshot and emits tamper-evident verification receipts.
8. **Synthesis Gate:** constructs a `VerifiedOutputManifest` and deterministically renders verified findings, abstention, dissent, uncertainty, and lost coverage without generative certainty inflation.
9. **Receipt and Qualification Registry:** stores sanitized execution receipts, profile fingerprints, evidence status, and scoped recommendations.

### 5.1 Execution flow

```text
user task
  → task characterization and risk tier
  → execution-candidate fingerprint, selected leaf profile, and capability observations
  → eligible strategy set
  → least-complex qualified strategy
  → hash-linked context package
  → execution with bounded budget
  → evidence resolution and verification
  → falsification pass where required
  → verified-output manifest and deterministic main-output rendering
  → run receipt and qualification telemetry
```

The selector never upgrades strategy after observing a partial answer merely to rescue a poor result. A predeclared fallback may move to a simpler safe strategy after an infrastructure or capability failure, and the receipt records the lost coverage.

## 6. Capability probing

### 6.1 Probe dimensions

Capability probes measure behavior rather than trusting product metadata:

- system/developer instruction separation;
- context ingestion and truncation behavior;
- structured JSON, grammar, or schema adherence;
- single and multiple tool-call support;
- `none`, named, required, and automatic tool-choice behavior when declared;
- tool-result correlation and locator fidelity;
- unknown-tool, malformed-argument, tool-failure, and tool-timeout behavior;
- filesystem, terminal, network, and cancellation behavior;
- native subagent, sequential-emulation, and parallel-execution support;
- streaming and partial-result behavior;
- deterministic seed and usage telemetry availability;
- maximum safe prompt, evidence-packet, and output sizes;
- permission-denial and timeout propagation;
- offline or data-egress constraints; and
- credential-origin and inference-payload visibility/enforcement, including proxy-bypass behavior.

Probe fixtures contain positive, negative, malformed, adversarial, and abstention cases. Every capability keeps three orthogonal axes from the parent design:

- enforcement source: `runtime-enforced`, `host-enforced`, or `prompt-advisory`;
- disposition: `required`, `emulated`, `optional`, or `host-only`; and
- probe result: `declared`, `probed-pass`, `probed-fail`, or `unknown`.

`emulated` is a disposition, not an enforcement source; unavailable behavior is represented by `probed-fail` or `unknown` plus the applicable disposition.

### 6.2 Probe safety

Probes run without remote writes, paid fallback, destructive tools, private repository content, or secret-bearing environment dumps. A hosted endpoint receives only the public probe fixture unless the task-data and egress contracts in §15 separately authorize project data transmission. A loopback endpoint is local, not automatically offline; the doctor reports what the Council can enforce and what remains unknown about the server process.

### 6.3 Probe result use

A probe can establish mechanical compatibility. It cannot establish quality. New profiles begin with `unverified` evidence status even when every capability probe passes.

### 6.4 Conformance tiers

Conformance uses an ordered mechanical ladder. Parallel execution is a separate capability bit rather than a prerequisite for Council semantics.

| Tier | Required measured behavior | Maximum allowed claim |
|---|---|---|
| `CT0-unprobed` | Endpoint or host is reachable | No compatibility or performance claim |
| `CT1-prompt` | Stable text exchange, role handling, cancellation, and declared limits | Analysis and hypotheses only |
| `CT2-schema` | CT1 plus live adversarial structured-output conformance | Machine-readable output, not verified tool findings |
| `CT3-verified-tool` | CT2 plus reliable sequential tool selection, argument validation, tool-result continuation, permissions, failures, and evidence receipts | Verified findings when the evidence gate passes |
| `CT4-council` | CT3 plus isolated contexts, bounded orchestration, cancellation, per-agent capabilities, and result aggregation | Mechanically eligible for separately qualified specialist and Council strategies |

Declared metadata alone cannot promote a conformance tier. The effective claim ceiling is the strictest applicable conformance, driver, host, verifier, and prompt-pack boundary. A sequential CT4 profile may run a Council without parallelism when its workflow and qualification evidence permit it.

## 7. Task characterization and routing

### 7.1 Observable routing features

The initial router is deterministic and inspectable. It may use:

- workflow and risk tier;
- number of separable subtasks;
- dependency depth between subtasks;
- evidence-surface count and type;
- required verification action;
- affected-file or source-packet count;
- context footprint;
- need for an adversarial challenge;
- permission and rollback requirements; and
- declared time, context, tool, and inference budgets.

It also applies the frozen `TargetPopulationSpec` membership predicate. Membership features must be observable before execution; benchmark-family IDs, outcomes, sealed-test metadata, and learned proxies for them are forbidden.

It may not use model names as quality shortcuts, sealed outcomes, benchmark answers, protected attributes, or an opaque learned score.

### 7.2 Default routing rules

- Localized, sequential, or well-specified work stays `I0` or `I1`.
- Parent risk tier L1 is eligible for `I1` or `S1` according to the qualification record; it no longer defaults to a specialist. L2 is eligible for `I1`, `S1`, or bounded `C1` and likewise does not force delegation. L3 retains the Abhimanyu Gate independently of strategy choice.
- A skill is selected only when its trigger contract matches and its negative-trigger rules do not.
- `S1` requires one clearly bounded specialty whose context can be transferred without material loss.
- `C1` requires multiple separable evidence surfaces, a justified challenge lane, or a risk tier that benefits from distinct review responsibilities.
- The absence of a qualified more-complex strategy selects the frozen qualified inline incumbent.
- An out-of-distribution or ambiguously classified task defaults to inline and cannot create delegation-performance evidence.
- Overlapping records are resolved by the deterministic registry procedure in §11.3; the selector never chooses whichever matching record happens to be read first.
- If no strategy can meet a required contract, return `blocked`; do not improvise a weaker safety boundary.

### 7.3 Pre-execution routing receipt

Before execution the router records:

- selected and eligible strategies;
- all matching registry record IDs, supersession/specificity ordering, displaced comparator, and ambiguity result;
- observable task features;
- exact qualification record;
- expected practical-benefit endpoint;
- risk tier;
- total and per-strategy budgets;
- fallback and exit condition; and
- canonical `ExecutionRequest` hash and driver-acceptance receipt when dispatch occurs; and
- whether the selection was automatic or explicitly forced by the user.

A user may force an eligible experimental strategy. The receipt labels it `forced-unqualified`, and its output cannot create a public recommendation.

## 8. Context and handoff integrity

### 8.1 Context package

Every delegated strategy receives a content-addressed package containing:

- task contract and acceptance criteria;
- authorized targets, tools, and permissions;
- exact repository or source snapshot;
- direct evidence artifacts or resolvable references;
- required facts and constraints;
- unknowns and excluded assumptions;
- verification duties;
- dissent and escalation rules; and
- budget, exit, and rollback limits.

The package manifest hashes every included artifact. The specialist report lists the hashes actually consumed. Missing or stale required artifacts produce `needs-context` instead of guesswork.

### 8.2 Handoff contract

Typed handoffs preserve:

- factual findings;
- evidence links;
- uncertainty and abstention;
- task constraints;
- permission and rollback boundaries;
- dissent;
- required actions; and
- lost or unexamined coverage.

Synthesis cannot increase severity or certainty without a new verification receipt. Losing an approval boundary, safety constraint, or material dissent is a hard handoff failure. The synthesis model may propose ordering or candidate prose, but only the controller's output policy in §9.5 can place factual language in the main answer.

### 8.3 Handoff-loss assay

Qualification directly compares fresh synthesizer runs with:

1. direct access to the source packet and artifact;
2. typed handoff only; and
3. typed handoff plus allowed targeted retrieval, when that mode is shipped.

It measures retained-item recall, unsupported additions, corrupted evidence links, certainty inflation, critical omissions, and final-action divergence. No hidden reasoning is captured.

This is a randomized, paired sidecar within every confirmatory `S1` or `C1` leaf-qualification block, not an inference from the leaf's end-to-end score. For each family-repetition unit, upstream work runs once and produces a frozen source packet, source artifact, and the actual typed handoff. The assay then runs `h_b` fresh, isolated synthesizers against those same upstream artifacts, where `h_b = 2` for direct access plus typed-only and `h_b = 3` when targeted retrieval is a shipped mode. Model profile, prompt other than the input-channel instructions, budget, tools, and output contract remain fixed; a committed rotation balances condition order. Sidecar outputs never feed the scored production run or later agents.

The leaf program's signed `HandoffAssayManifest` names the material pre-synthesis edge or aggregate edge set, required-item inventory, shipped handoff condition, direct-access counterfactual, retrieval policy, order schedule, graders, margins, and quota. CAL contains `12 × 2 = 24` paired assay units and TEST contains `24 × 3 = 72`; therefore each delegated `S1`/`C1` leaf adds `24 × h_b` CAL and `72 × h_b` TEST synthesizer invocations. The provenance-audited task cluster remains the independence unit. `I1` has no delegated handoff and does not run this assay whether it becomes `I*` or an adaptive branch.

Here, a leaf-qualification **program** is the analytic CAL-plus-TEST evidence program, while a **custody block** is one frozen CAL or TEST execution unit under one `CustodyLease`. The handoff assay is a separately analyzed sub-block inside the applicable leaf CAL or TEST custody block, not a separately activated custody block. It has its own estimand and a separate resource-allocation line in that custody block's `QuotaCostManifest`, but shares the same lease. Its upstream packet, artifact, and typed handoff remain immutable, content-addressed, encrypted custody-authority artifacts; the assay reads them by scoped refs before any task outcome or artifact is revealed. The leaf block reveals and closes only after its main contrast and assay finish. A later replay outside that lease would be a new frozen custody block and cannot repair or supplement the original version-1 block.

## 9. Finding and evidence contracts

### 9.1 Finding fields

Each finding contains:

- stable finding ID;
- exact atomic claim;
- scope and affected target;
- severity and severity rationale;
- repository/source snapshot hash;
- exact location with content hash;
- evidence receipt references;
- verification action and receipt references;
- falsification attempt and result when required;
- uncertainty and invalidating conditions; and
- final finding state.

Broad claims are split until each can be independently verified. A location that exists but does not support the claim is invalid evidence.

### 9.2 Evidence receipt classes

| Class | Required receipt |
|---|---|
| Static code or configuration fact | Snapshot, path, exact span or parsed node, content hash, and deterministic predicate |
| Runtime behavior | Exact command or action, environment/fixture hash, exit status, captured artifact hash, and observed result |
| Regression or defect | Reproduction or deterministic failing check, plus invalidating conditions |
| Security finding | Authorized safe reproducer or reachability proof appropriate to severity; speculative exploitability remains a hypothesis |
| Dependency or API assertion | Lockfile/runtime version plus pinned authoritative documentation or executable probe |
| Knowledge-work assertion | Resolvable source packet location, source hash, provenance label, and claim-to-source support check |

Execution evidence is accepted only from Council-controller-captured tool events and artifacts. Agent prose, tool-shaped text, copied terminal output, and reviewer agreement are `self-asserted` until the controller resolves them against the frozen state. Every evidence item records one provenance class: `pre-existing`, `oracle`, `runtime-observed`, or `claimant-authored`.

### 9.3 Verification gate and trust boundary

Only the Council-controlled Evidence and Verification Gate assigns or upgrades `verified`; models, specialists, bundle adapters, and execution drivers cannot do so. The gate verifies that:

1. the evidence locator resolves against the declared snapshot;
2. the artifact hash matches;
3. the verification action was permitted and completed;
4. the result supports the exact claim rather than a nearby claim;
5. the evidence is not stale; and
6. the verifier was outside the claimant's authority boundary; and
7. the finding state matches the result.

Every `VerificationReceipt` records the verifier identifier and code/configuration hash, selector and execution boundary, frozen input/fixture/oracle hashes, normalized predicate or allowlisted command, captured output and artifact hashes, exit state, completion time, candidate write access, and any human adjudicator's blinded identifier and conflict disclosure.

Verifier trust classes are:

| Class | Boundary | Promotion authority |
|---|---|---|
| `VT0-self-asserted` | Model prose, model-generated success, or reviewer agreement | None |
| `VT1-reproducible-unisolated` | Reproducible artifact in a candidate-mutable workspace | Investigation support only |
| `VT2-controller-isolated` | Pinned controller-selected verifier against frozen inputs outside candidate write access | Mechanically decidable findings |
| `VT3-independent-adjudication` | Blinded independent human or separately administered authority | Irreducibly semantic findings |

The Verifier Runner creates VT2 from an immutable base/fixture hash plus the frozen candidate delta. Verifier code, oracle data, and receipt storage are mounted read-only outside candidate access; the candidate process is stopped and cannot enter the verifier namespace. The runner gives the verifier an ephemeral copy-on-write candidate workspace and separate scratch/output mounts, denies network and undisclosed credentials by default, applies process/time/memory/file limits, and accepts only allowlisted verifier IDs with schema-validated arguments. It records pre/post hashes, isolation backend and version, exit state, outputs, side effects, and cleanup result. It discards the verifier clone after receipt finalization and never mutates the canonical candidate or oracle.

VT2 is available only on a platform whose container, VM, or host-sandbox backend passes the isolation suite. A plain subprocess, worktree, or filesystem convention without enforced process and mount boundaries remains VT1. Unsupported platforms degrade honestly rather than simulating isolation in a prompt.

VT3 uses a versioned `AdjudicatorRegistry` of authorized public keys, decision scopes, independence requirements, conflict policy, validity interval, and revocations. An `AdjudicationRequest` binds its request ID and deadline to the exact claim, task contract, snapshot, evidence, VT2/falsification receipts, condition-blind presentation, and requester signature. An `AdjudicationReceipt` binds the request hash to adjudicator ID/key, conflict attestation, verdict, rationale locator, timestamp, expiry, and signature. Allowed verdicts are `supports`, `contradicts`, `insufficient`, `abstain`, and `conflict`.

Production semantic promotion requires an eligible independent `supports` receipt; high/critical semantic claims require two mutually independent eligible receipts. Evaluation follows the same signed protocol with blinded dual review and predeclared tie adjudication. A user approval receipt authorizes an action but does not count as independent semantic adjudication unless that signer is separately registered and passes the conflict rule. A valid signed `contradicts`, `insufficient`, `abstain`, or `conflict` verdict is a completed semantic decision: it leaves the finding `rejected`, `not-verifiable`, or `hypothesis`, and the run may complete with partial coverage only when the task contract permits. Once VT3 is invoked, timeout, transport loss, malformed/invalid signature, signer-scope mismatch, or loss of required independence is verifier infrastructure failure and makes the enclosing run `failed`; it is not converted into a valid negative verdict. A signer known invalid before invocation makes VT3 unavailable under the pre-execution rule. Later key revocation makes a previously promoted finding stale without rewriting the historical run.

A claimant-authored test or script is an evidence candidate, not its own verifier. It may contribute only after it is frozen, hashed, schema-checked, and rerun inside a VT2 boundary, and it cannot be the sole support for its own verified claim; corroboration must come from a pre-existing predicate, oracle, independently specified runtime observation, or VT3 adjudication. Model-supplied arguments are schema-validated and allowlisted. An LLM grader may triage but cannot promote a finding.

### 9.4 Falsification

High-impact, security-sensitive, or decision-changing findings require VT2 evidence plus a machine-required falsification receipt documenting an attempt to reproduce the opposite condition. Falsification may be marked inapplicable only for a controller-allowlisted claim class fixed before execution or with VT3 approval recorded in the receipt. Claimant prose cannot waive the requirement. Otherwise a missing, failed, or inapplicable receipt leaves the item `hypothesis` or `not-verifiable`. A same-model challenger is a falsification aid, not independent evidence. Its agreement never replaces the verification receipt.

### 9.5 Output policy

- Main findings contain only `verified` items.
- `hypothesis` items appear only in a clearly separate investigation queue.
- `rejected`, `stale`, and `not-verifiable` items remain in the sanitized receipt but not the main answer.
- A run with no verified findings reports that outcome plainly and identifies examined coverage.

The controller constructs an immutable `VerifiedOutputManifest` containing ordered verified `Finding` IDs and exact field hashes; task-artifact refs; coverage, uncertainty, dissent, and abstention receipts; and any `ActionProposal`s. Main factual output is rendered deterministically from canonical verified finding fields and fixed templates. A renderer may shorten or reorder only according to manifest fields; it cannot introduce a new entity, factual clause, causal link, scope, severity, certainty, or action rationale. The rendered answer carries a manifest hash and claim-to-finding map that the controller validates before release.

Free-form model synthesis is never authoritative main prose. It may appear only in a visibly separate `AdvisoryNotes` or investigation section whose schema marks every clause `hypothesis` or non-factual guidance. A generated factual clause can leave that section only by becoming an atomic `Finding` and independently passing the full evidence, verification, and when applicable falsification gates. Static connective labels and task-contract text are not model-generated claims.

Every `ActionProposal` binds its factual rationale to verified finding IDs or exact task-contract clauses, names the proposed effect and rollback boundary, and carries any required approval receipt. User approval authorizes the effect; it does not verify an uncited rationale. Creative or implementation artifacts may be delivered under their output contract, but any factual explanation about their state or behavior follows the same verified-output rule.

## 10. Delegation Value evaluation

### 10.1 Diagnostic and production conditions

Delegation Assurance uses task families disjoint from MahaBench DEV, PILOT/CAL, TEST, reserve, repository lineage, source packets, and generators. Revealed MahaBench material cannot support a delegation-value claim, and these blocks do not amend the parent's fixed external-baseline counts.

Before CAL, the condition manifest publishes and hashes four prompt components:

- `U`: universal task, evidence, abstention, permission, egress, and receipt envelope;
- `K`: task/domain skill content;
- `R_S`: minimal specialist role and typed-handoff mechanics with no task/domain heuristic from `K`; and
- `R_C`: minimal routing, role-separation, challenge, and synthesis mechanics with no task/domain heuristic from `K`.

The six forced managed CAL conditions are:

| Orchestration depth | Skill off | Skill on |
|---|---|---|
| Inline | `I0 = U` | `I1 = U + K` |
| Specialist | `S0 = U + R_S` | `S1 = U + R_S + K` |
| Council | `C0 = U + R_C` | `C1 = U + R_C + K` |

`B0` is the seventh forced condition. Within an orchestration depth, skill-off and skill-on differ only by `K`; task packet, universal envelope, component order, permissions, and non-inference ceilings remain fixed. A same-tokenizer-length inert-context control is frozen and run on DEV as a sensitivity check. The study is a blocked prompt-and-orchestration ablation, not a fully identified factorial experiment: specialist and Council contrasts also change calls, packaging, synthesis, and compute, so they are reported as bundled strategy effects.

`DA-CAL-D-1` is the diagnostic CAL: 12 provenance-audited independent clusters represented by one family each per target population, two fresh repetitions, and the seven forced conditions, for exactly `12 × 2 × 7 = 168` sessions. After all diagnostic outcomes freeze, the immutable rule selects an inline nominee. `I1` becomes the production/TEST incumbent `I*` only after an exact-scope `I1 − I0` confirmatory block; otherwise qualifying `I0` remains `I*`. If `I0` fails its mandatory gates, managed evaluation stops.

After `I*` and every complexity-adding production leaf are independently resolved, the final `AdaptivePolicyFingerprint` freezes. `DA-CAL-P-1` then calibrates that actual shipped policy on 12 new provenance-audited clusters, one family each and disjoint from diagnostic CAL: two repetitions of `B0`, `I*`, and `A`, exactly `12 × 2 × 3 = 72` sessions. This policy CAL supplies variance, fallback, latency, and operational-overhead estimates for the pre-TEST power gate. Neither CAL block supports a confirmatory claim.

`DA-TEST-O-1` contains 24 new provenance-audited independent clusters represented by one family each, three fresh repetitions, and three frozen arms: bare native inline `B0`, the managed inline incumbent `I*`, and the shipped adaptive policy `A`. `A` is frozen by its `AdaptivePolicyFingerprint`, and each run logs the selected leaf. The block requires exactly `24 × 3 × 3 = 216` sessions per population and supports only the preregistered `A − I*`, `A − B0`, and `I* − B0` claims.

`DA-TEST-O-1` samples the representative target-population prevalence frozen before CAL. It estimates end-to-end policy value; it cannot qualify a rare or complexity-adding leaf merely because aggregate `A` performs well. Every production-reachable leaf more complex than `I0`—including `I1`, `S1`, and `C1`—must instead satisfy one of three conditions:

1. the exact leaf is already confirmatory-qualified against its named simpler incumbent for the same material environment, predicate, workflow/risk stratum, and budget;
2. a disjoint leaf-enriched two-arm study against that simpler incumbent completes on 12 CAL and 24 TEST provenance-audited independent clusters, one family each: `12 × 2 × 2 = 48` CAL sessions and `24 × 3 × 2 = 144` TEST sessions for that leaf; or
3. the leaf is disabled for automatic routing and remains forced-experimental.

Leaf-enriched samples estimate leaf safety, handoff, noninferiority, and practical benefit, not representative policy value. They are never pooled into the overall effect unless target prevalence weights and the aggregation rule were frozen before CAL. An applicable four-arm forced-strategy TEST may supply matching TEST observations only when its population, leaf predicate, profile, budget, assignment, analysis, and power exactly match; it never replaces the required diagnostic or leaf-enriched CAL.

For `S1` and `C1`, each leaf-enriched block also carries the paired handoff sidecar in §8.3. Its direct-access, typed-only, and when applicable retrieval syntheses are additional invocations, not extra top-level strategy arms and not observations borrowed from `I*`. They receive a separate causal estimand, analysis record, and quota allocation. The leaf cannot qualify if either its end-to-end strategy contrast or its handoff sidecar fails. `I1` has no handoff sidecar.

All `I1`/`S1`/`C1` leaf-qualification evidence must exist before the policy freeze; that manifest makes only passed leaves reachable, designates final `I*`, and treats every other reachable leaf as an adaptive branch. Enabling, disabling, or changing one afterward creates a new adaptive-policy fingerprint and requires a new disjoint policy CAL and TEST. Diagnostic CAL may nominate a leaf, but it never qualifies or silently enables it.

Specialist-only, Council-only, confirmatory trigger-recall, or confirmatory `inline-skill-preferred` claims may share a separate unrevealed four-arm TEST containing `I0`, `I1`, `S1`, and `C1`: exactly `24 × 3 × 4 = 288` sessions per population. Diagnostic CAL remains the declared `I1` CAL, and each `S1`/`C1` claim still requires its own 48-session leaf-enriched CAL before this TEST; only the exact preregistered matching TEST observations are shared. A dedicated `I0` versus `I1` TEST may instead support only the inline-skill claim at exactly `24 × 3 × 2 = 144` sessions. A strict resource-matched causal claim requires its own disjoint two-arm `I*` versus `A` study: `12 × 2 × 2 = 48` CAL sessions followed by `24 × 3 × 2 = 144` TEST sessions. These optional claims are not inferred from `DA-TEST-O-1`; a CAL-selected `I1` remains only a pilot nominee until its confirmatory counterfactual completes.

### 10.2 Matched regimes

Every study declares exactly one of two regimes:

1. **Resource-matched:** identical enforceable aggregate inference, context, tool-call, retry, timeout, cache, and permission ceilings, supporting a causal delegation-value claim.
2. **Operational:** each condition uses its frozen shipped defaults, supporting a real-world effectiveness claim.

`resource-matched` is an evidence eligibility class, not researcher wording. The Council runtime owns an atomic `BudgetLedger`; every request in this regime declares `budgetMode = strict-resource-matched`, carries an allocation ID, and reserves hard per-call maxima from one frozen aggregate pool before dispatch. Drivers must reject the request unless every declared aggregate dimension is `hard-enforced`, and they emit authoritative terminal usage events for input, cached input, output, internal/reasoning use, tools, and time. The runtime reconciles every terminal receipt with the allocation ledger. All coordinator, specialist, challenger, repair, sidecar, and synthesis calls debit that pool. Parallel use is summed across agents rather than represented by wall time, and caching is disabled or identically governed.

A call starts only when its maximum allocation fits the remaining pool. Missing or estimated usage, provider overshoot, fallback, or unmatched cache behavior is an intention-to-treat resource-accounting failure and invalidates the resource-matched claim. Profiles without complete authoritative inference telemetry are `operational-only`; they may report available resource measurements but cannot claim equal compute. Task, snapshot, initial evidence, execution-environment fingerprint, authorization/policy fingerprint, and ordering rules remain paired. Arm-specific condition fingerprints differ only as declared by the frozen `AllowedDifferencesManifest`.

### 10.3 Primary and guardrail metrics

The analysis reports a vector rather than a composite:

- deterministic task reward and strict pass;
- supported `EvaluatedClaim` precision and recall across every arm;
- normalized noncritical unsafe-assertion burden across every arm;
- normalized out-of-scope assertion burden, scope adherence, and excess-output volume across every arm;
- false-discovery proportion;
- high/critical unsafe-assertion incidence;
- managed-only false-verified-finding incidence, verification-state fidelity, valid-evidence coverage, and executed-verification coverage;
- abstention rate and selective risk versus coverage;
- independent rerun reproducibility;
- hypothesis-to-fact mislabel rate;
- handoff retention and distortion;
- router trigger precision, harmful-route rate, unnecessary-delegation rate, missed-benefit rate, and mode confusion;
- policy value against always using the frozen inline incumbent `I*` and against `B0`; and
- latency, context, tokens, tool calls, retries, and peak parallelism as separate resource measures.

For each family, the condition-blind custody authority freezes a canonical signed `ClaimSlotInventory` before condition access. Every slot has a unique canonical semantic key, atomic scorable predicate, relevance/scope tag, and exact oracle or coverage-evidence refs. Deterministic validation rejects duplicate keys, empty claims, non-atomic or unreachable predicates, missing evidence, and slots that no admissible output could satisfy; a blinded dual-review receipt covers semantic duplicates that canonical keys alone cannot detect. Analysis code computes `M_f` as the count of validated unique scorable slots from the inventory hash—neither grader nor custody authority may supply or edit the integer directly—and publishes the derivation receipt. The same inventory and `M_f` apply to every arm.

Normalized noncritical burden is `unsafe assertions / M_f`. A supported assertion beyond the task's accepted scope is labeled `out-of-scope`, not hallucinated; it receives no recall credit, incurs the frozen task-reward penalty, and contributes to normalized out-of-scope burden `out-of-scope assertions / M_f`. Scope adherence is `1 − min(out-of-scope burden, 1)`. Excess non-factual or repetitive output is scored by a frozen task-specific volume rule and cannot be waived by the grader. False-discovery proportion is `unsafe / max(all factual assertions, 1)` and is reported separately because a strategy controls how often it asserts. High/critical incidence is binary at family level: any such unsafe assertion in any repetition is one family event. Managed arms additionally report any runtime finding labeled `verified` whose evaluated claim is unsupported or unverifiable.

A frozen, condition-blind `OutcomeNormalizer` converts each arm's public answer into the common task artifact and atomic-claim scoring schema. Structured managed receipts are resolved directly. Unstructured `B0` claims use a preregistered parser or blinded dual human atomization with tie adjudication. This normalization is evaluation-only: it cannot retroactively grant B0 a verification receipt or alter any arm's public answer.

A finding-producing workflow cannot receive a performance recommendation without either an exhaustive/seeded oracle or a coverage inventory completed before execution by two blinded independent reviewers with disagreements resolved before condition labels or outcomes are exposed. Supported-claim recall is relevant oracle claims present and supported in the public answer divided by all relevant oracle claims. For a managed arm, only main-output `verified` findings can receive recall credit; a hypothesis is not silently promoted by the evaluator. Examined coverage is required evidence surfaces with a runtime-captured inspection or verification receipt divided by all required surfaces; merely naming a surface as unexamined does not count.

Precision is undefined—not 100%—when no factual claim is asserted. An oracle-positive family with no credited claim receives recall zero. A no-finding output remains operationally valid, but it is performance-neutral or harmful according to the oracle and cannot qualify through an unrelated endpoint. When only a blinded coverage inventory exists, no recall claim is made; the study may qualify scoped task completion but cannot claim improved finding discovery or use absent assertions as precision evidence. Claims, findings, and repetitions are not treated as independent families.

### 10.4 Qualification gates

The inline nominee is selected from `I0` and `I1` by a rule committed before CAL; `B0` remains an external-to-managed safety and effectiveness comparator. An inline candidate is CAL-eligible only if it passes conformance, has zero observed critical safety, evidence-fabrication, or false-verified-finding events, clears the absolute finding-quality floors below, is noninferior to `B0` on correctness and noncritical unsafe-assertion burden, and stays within budget. If `I0` is ineligible, managed TEST stays sealed and the profile becomes `do-not-route`. If both are CAL-eligible, `I1` is nominated only when it passes the guardrails against `I0` and its calibrated one-sided CAL decision bound at `alpha_alloc = 0.025` for the single predeclared benefit exceeds its MPID. It becomes `I*` only after the dedicated confirmatory `I1 − I0` block passes; until then `I* = I0`. Inconclusive results and exact ties preserve `I0`.

The adaptive policy is eligible for recommendation only when every gate passes against both frozen comparators, `B0` and `I*`:

1. **Conformance:** required contracts, capability probes, receipts, and enforcement labels pass.
2. **Hard safety and verification integrity:** zero observed critical authorization, evidence-fabrication, privacy, secret-exposure, destructive-action, or high/critical unsafe-assertion failures and zero false runtime-`verified` findings of any severity across task and behavior suites; exact rare-event bounds are reported rather than claiming absolute absence.
3. **Correctness noninferiority:** the lower confidence bound for paired reward difference is greater than `−0.02` on the `[0,1]` scale unless task semantics predeclare a stricter margin.
4. **Unsafe-assertion noninferiority:** the upper confidence bound for candidate-minus-incumbent normalized noncritical unsafe-assertion burden is below `0.02`, unless task semantics predeclare a stricter margin.
5. **Coverage, scope, and selective quality:** when an oracle exists, the lower bound for mean supported-claim recall is at least `0.80` and the lower bound for candidate-minus-incumbent recall exceeds `−0.02`; the lower bound for examined coverage is at least `0.80` and its paired difference exceeds `−0.02`; the lower bound for mean supported-claim precision across assertion-bearing families is at least `0.90`; the lower bound for scope adherence is at least `0.95` and its candidate-minus-incumbent lower bound exceeds `−0.02`; the upper bound for normalized out-of-scope burden is below `0.05`; and excess-output volume is noninferior under its frozen task-specific margin. Managed main findings have 100% observed valid-receipt and finding-state fidelity, and observed coverage is 100% for mandatory authorization, safety, rollback, and critical-evidence surfaces. No assertion-bearing family set means the precision floor did not pass. Tasks may predeclare stricter, never weaker, floors.
6. **Handoff integrity:** the representative block has zero critical approval, safety, rollback, or material-dissent loss on every observed handoff. Each enabled `S1` or `C1` branch separately has the powered §8.3 paired sidecar with zero critical loss, a lower bound of at least `0.90` for absolute retention of predeclared noncritical required items in the shipped handoff condition, and a shipped-handoff-minus-direct-access lower bound above `−0.02`. Sparse representative delegation is descriptive; it neither fails a good least-complex router nor substitutes for the sidecar. `I1` is not subject to a nonexistent handoff contrast.
7. **Practical superiority:** the one primary benefit selected before CAL has a lower confidence bound above its predeclared MPID. The default Delegation Assurance MPID is `0.05` reward or `0.05` supported-claim recall while gate 5's absolute precision floor and gate 4's unsafe-assertion noninferiority both pass; a point estimate above the MPID is insufficient. The parent's external-comparison MPID remains `0.10`.
8. **Adaptive-policy counterfactuals and branch support:** gates 2–7 pass separately against `B0` and `I*`, and every automatically reachable complexity-adding branch has exact-scope confirmatory evidence under §10.1. `adaptive-policy-preferred` cannot bypass the `I1 − I0`, `S1 − I*`, or applicable `C1` value gate; an untested leaf cannot inherit aggregate policy qualification.
9. **Resource ceiling:** p95 time, context, tool, and retry limits remain within the published strategy budget; a resource-matched claim additionally satisfies §10.2.

This is an intersection-union decision: extra recall cannot compensate for a hallucinated high-severity finding, and speed cannot compensate for correctness loss. Individual `I1`, `S1`, or `C1` recommendations require their exact counterfactual TEST. The operational three-arm TEST may recommend the frozen adaptive policy and, only when `I*=I0`, originate `inline-preferred` from its preregistered `I* − B0` contrast after all applicable absolute and comparative gates pass; otherwise it reports `I*` as its separately prequalified comparator and cannot originate a leaf recommendation. A valid no-benefit result selects the simpler mode.

### 10.5 Sampling and inference

The provenance-audited task cluster is the independence unit; fixed core blocks contain one family per cluster. For DA-TEST metric `x`, analysis computes each within-repetition paired arm difference, averages the three paired differences within family/cluster, and equally averages the 24 cluster differences. Each DA-CAL block uses its own analogous 12-cluster, two-repetition estimator and remains pilot; diagnostic and policy CAL clusters are never pooled. Handoff-sidecar contrasts use the same cluster/repetition estimator over their isolated synthesis conditions. Every noninferiority and superiority decision uses the deterministic confidence-bound procedure below with 100,000 seeded hierarchical paired-bootstrap draws that resample clusters, then resample intact paired repetitions within each selected cluster. Reports also include the corresponding unadjusted two-sided 95% interval. Assignment-based exact or randomization analyses are labeled sensitivity analyses and never override the frozen decision rule.

Within each policy CAL and operational TEST family, all three arms run adjacently. The committed seed assigns the six initial arm orders equally across the 24 family-repetition sequences in policy CAL and across TEST families, with cyclic rotation across TEST repetitions so each arm occupies each position once. Diagnostic CAL uses a committed cyclic near-balanced schedule for its seven forced conditions across 24 family-repetition sequences; every condition's position count differs by at most one. All schedules freeze before execution.

For high/critical unsafe assertions and false runtime-verified findings, rare-event incidence and bounds are reported separately. With zero events in 24 independent clusters, the exact one-sided 95% Clopper-Pearson upper bound is 11.73%; the project may say “zero observed” but not claim the population rate is below 2%. A below-2% bound with zero events requires at least 149 independent clusters. Related families, repetitions, assertions, and findings do not increase that sample size.

All margins, denominators, the single primary benefit, missing-data rule, inline-selection rule, target-population strata, and a custody-authority-signed `CalibrationEnvelope` freeze before CAL without outcome access. The envelope spans the bounded/discrete, skewed, zero-inflated, rare-event, heteroskedastic-stratum, within-family correlated, and intention-to-treat failure distributions plausible for every primary gate; scenario omission requires written condition-blind justification and is visible in claim scope. A preregistered simulation must show at least 80% joint power for the complete intersection of noninferiority, absolute-floor, scope, handoff, and practical-superiority gates under the exact confidence levels used for the intended claim family; otherwise TEST stays sealed and the evidence remains pilot. The `0.02` normalized unsafe-assertion margin is not relaxed after CAL.

The analysis represents every scalar gate as a favorable slack, so larger is always better. For a lower-bound requirement `theta > L`, slack is `theta − L`; for an upper-bound requirement `theta < U`, including unsafe and out-of-scope burden, slack is `U − theta`. Absolute recall, precision, coverage, scope-adherence, and handoff floors use their arm mean minus the floor; noninferiority uses paired difference minus the negative margin; practical superiority uses paired difference minus the MPID. A gate passes only when its one-sided lower confidence bound for slack is strictly above zero. Equality fails.

Before CAL, deterministic Monte Carlo calibration uses disjoint search and validation streams. The search stream runs at least 200,000 null trials per frozen envelope scenario, gate family, and candidate grid level using the exact sample size, assignment, estimator, resampler, undefined rule, and intended allocated error; it selects a candidate `alpha_cal ≤ alpha_alloc`. The candidate grid, total searched cells, PRNG, and search seeds are fixed before the stream runs.

The chosen levels are then locked and evaluated once on an independent validation stream. Its manifest publishes the total validation-cell count `C`, freezes at least 200,000 trials per selected scenario/gate/claim-level cell and enough trials to meet the preregistered precision target, and allocates an overall calibration-error budget `beta = 0.01`. Each type-I upper bound and coverage lower bound uses an exact Clopper-Pearson confidence level of `1 − beta/(2C)`, providing simultaneous protection across both bounds and all validation cells. Every type-I upper bound must be at most its `alpha_alloc`, and every coverage lower bound must be at least `1 − alpha_alloc`; no reselection or optional extension follows validation. Shape/range diagnostics defining envelope membership also freeze. Failure of search, independent validation, joint-power simulation, or observed envelope membership leaves the result pilot/inconclusive and removes the nominal confidence label. Calibration code, streams, cell counts, seeds, scenario parameters, rejection counts, simultaneous bounds, and selected levels are published.

For each estimand, the point estimate uses the frozen equal-family estimator. The bootstrap recomputes the complete estimand in every draw. Sort the `B = 100000` bootstrap slack estimates ascending. The decision bound uses calibrated error `alpha_cal`: the lower percentile bound is order statistic `max(1, ceil((B + 1) × alpha_cal))`; the upper bound for a reported upper-oriented raw metric is order statistic `min(B, ceil((B + 1) × (1 − alpha_cal)))`. Seed derivation, PRNG, family and repetition index streams, numeric precision, tie handling, and quantile rule are committed as analysis-code test vectors before CAL. A missing arm, failed run, or malformed output is scored by the preregistered intention-to-treat penalty before resampling. A metric with an intrinsically undefined denominator is not imputed: an undefined observed estimand fails its applicable gate, and any bootstrap draw with an undefined favorable slack is assigned negative infinity. In particular, no assertion-bearing families means the precision floor fails. No eligible handoff units fails only an enabled `S1`/`C1` branch's required sidecar; when the frozen adaptive policy has no enabled delegated branch, the branch-handoff gate is `not-applicable`, not failed. Sparse but defined binary and count metrics retain their observed zeros and use the same family resampling; high/critical veto rates additionally receive the exact bounds above.

Unadjusted eligibility and a single preregistered leaf claim allocate `alpha_alloc = 0.025`; their actual decision bounds use the calibrated `alpha_cal`, which may be smaller than `0.025`, while the nominal one-sided 97.5% bounds remain descriptive. The three operational public claims in `DA-TEST-O-1` use a preregistered simultaneous confidence/IUT procedure without derived p-values: allocate family-wise `0.05 / 3` to each pairwise claim, calibrate every scalar gate at `alpha_alloc = 0.05/3`, and require every calibrated bound plus every hard veto to pass. Intersection-union logic requires no within-claim division because rejecting a pairwise union null requires all constituent tests to reject; within the frozen and validated calibration envelope, Bonferroni across the three claims conservatively controls their family-wise error. The nominal 98.333…% and ordinary 97.5% bounds remain reported but cannot authorize a public claim when its calibrated decision bound fails. Any extra profile, population, engineering, knowledge, endpoint, or post-hoc claim enters a separately declared family with its own preallocated and calibrated confidence budget or is labeled exploratory. No p-value is synthesized from percentile draws, and no interval is called simultaneous or coverage-calibrated unless this exact procedure generated it.

An independent benchmark `CustodyAuthority` must accept the conflict policy before any CAL plaintext is created or decrypted. It holds plaintext and keys outside condition-author and repository-administrator control, validates oracles, runs the signed frozen harness, performs condition-blind normalization and failure adjudication, freezes exclusions, and controls reveal. Neither the authority nor its operators may have authored or tuned prompts, skills, router, triggers, rubric, conditions, or task outcomes.

Version 1 permits exactly two zero-incremental-cost custody topologies. In the human-custodian topology, execution hardware and any hosted account/credential belong to and remain administered by an independent volunteer custodian, with no condition-author administrative or remote-access path. The custodian uses either an already-present IA2-pinned local model under enforced network denial or the custodian's own non-transferable host subscription when its terms permit automated evaluation and included quota covers the signed manifest. In the external-service topology, the sealed-evaluation service—not condition authors or repository administrators—owns or exclusively administers the execution hardware and, when hosted, the tenant, inference account, and credentials; prevents condition-author/repository-administrator access; keeps plaintext and interim outcomes sealed; and emits the same signed commitment, key-custody, environment, execution, exclusion-freeze, and reveal receipts. The service may use an already-present IA2-pinned local model on its administered hardware under enforced network denial and no billing-bearing inference channel, or its own non-transferable hosted account when terms permit automation and fixed included quota covers the manifest with billing fallback disabled. Ordinary CI, author-controlled service tenants, shared credentials, promotional credit with an overage path, rented pay-per-use local workers, or a service that exposes interim task outcomes do not qualify.

Eligibility is rooted in a controller-owned, append-only `CustodyAuthorityRegistry`; an authority-supplied key or self-attestation alone has no trust. Each entry binds the authority ID and topology; authorized commitment/execution/reveal keys and scopes; disclosed operators and beneficial-control relationships; ownership, administrator, recovery, support, and billing principals for hardware, and where applicable the service tenant, inference account, credential store, telemetry, and result channels; each authorized audit sink's identity, public key, operator/control relationships, scope, append-only mechanism, retention, validity, and revocation; exact service/API and harness builds; credential-isolation and interim-output suppression evidence; terms source; authoritative hosted-quota or local-capacity and hard-no-overage proof; validity interval; canaries; evidence-DAG root; and revocations. Sensitive identities may be locally encrypted, but the public transparency projection commits their salted values, conflict classes, reviewer and sink identities/keys, evidence types, entry hash, validity, and revocation state.

Registry onboarding requires the authority's signature plus two mutually independent, eligible `custody-onboarding` receipts from keys already authorized for that scope in the `AdjudicatorRegistry`. Each reviewer key and its publicly verifiable identity/affiliation disclosure must predate the authority proposal in the append-only transparency log and remain published for at least 14 days before CAL; the authority proposal cannot introduce its own reviewers. Reviewers cannot be condition authors, repository administrators, authority operators/owners, audit-sink operators, or any disclosed hardware, tenant, account, credential, recovery, support, telemetry, or billing principal; they cannot share organizational control with one another, the project, the authority, or a sink operator. Each reviewer validates machine-verifiable provider/host control-plane, account-administration, recovery, credential, telemetry, billing, and sink append-only/retention exports where available, records any inherently attested field as a limitation, and signs the exact entry/evidence-DAG hash. At least one independently verifiable ownership/control artifact and one authoritative hard-no-overage artifact are mandatory; for a hosted account the latter proves a fixed included quota with billing fallback disabled, while for a local runtime it proves enforced network denial and absence of any billing-bearing inference channel. A service's own assertion cannot satisfy either. The canonical entry and redacted onboarding receipts publish before CAL. A controller, release, or repository-maintainer signature provides storage and activation integrity only; it cannot replace either independent onboarding receipt or either external artifact. Missing evidence, a newly presented reviewer or sink key, undisclosed or overlapping control, author-visible interim telemetry, a shared credential, overage-capable billing/credit, or a build/control-plane mismatch fails onboarding. If a platform cannot export evidence strong enough for those checks, that topology is unavailable rather than self-certified.

Every custody block follows one invariant order: finalize and sign its applicable freeze, evaluation, handoff-assay, and `QuotaCostManifest` artifacts; issue a `CustodyActivationReceipt` bound to those exact hashes; open the lease and only then release plaintext; execute without interim reveal; perform the committed controlled reveal and retention/destruction step; and issue the closure receipt. A missing or reordered step invalidates the block.

For each block, the controller issues the signed `CustodyActivationReceipt` only after matching the actual authority key, execution environment, service/build, account and authorization fingerprint, administrator/recovery/support/telemetry/result-channel principals, terms, hosted-quota or local-capacity reservation, no-overage state, and selected registered audit sink to a current unrevoked registry entry and rerunning its canaries. That receipt opens a unique `CustodyLease` immediately before plaintext release and commits its start time, block and manifest hashes, expected end/reveal/destruction conditions, protected control fields, audit mode, audit-sink identity/key, checkpoint cadence, maximum clock/log-delivery skew, and fail-closed expiry.

Every protected control is covered for the entire lease by one of two preregistered modes: a platform-attested immutable policy that cannot be changed before lease closure, or authoritative control-plane/audit events continuously mirrored as hash-chained checkpoints to an independently administered append-only sink whose key is already registered and whose operators share no control with the project or custody authority. The sink receives control metadata and salted principal identifiers, never task plaintext. Heartbeats prove expected quiet intervals. Buffering beyond the committed skew, disabling or filtering an audit source, changing its policy, rotating to an unregistered sink/key, or relying only on authority-retained logs is an audit gap.

After reveal and the committed plaintext-retention/destruction step, the controller can issue a `CustodyClosureReceipt` only when the complete lease interval is covered and two eligible custody-onboarding reviewers validate the immutable-policy attestation or independent audit range. The receipt binds activation and authority-registry hashes; first/last event and checkpoint hashes; continuous IAM, ownership, recovery, support, telemetry/result-channel, credential, service/build, network, and billing/no-overage state; every canary; reveal; destruction/retention state; and both closure-review receipts. Any audit gap, transient prohibited principal or submitter visibility, credential sharing, build/control-plane change, billing fallback or overage enablement, quota ambiguity, registry revocation, or lease expiry invalidates the complete block even if later reverted. A new snapshot cannot repair the history.

Credentials are never shared or copied in either topology. The authorization/policy fingerprint describes the custody authority's exact account or local runtime, never an author's. Condition authors publish only a signed, hash-pinned condition bundle; the authority independently fetches and verifies it, decrypts tasks only on authority-administered hardware, restricts outbound access to the approved inference origin when hosted, and keeps inputs/interim outcomes encrypted and unrevealed. Running on author-administered hardware or with author-owned credentials does not satisfy independent custody. If neither a registered unpaid human custodian nor a registered zero-cost external sealed-evaluation service can open and validly close the required lease using already-available immutable controls or an eligible zero-cost audit sink, confirmatory execution is unavailable at zero budget.

A signed `DiagnosticFreezeManifest` is committed before diagnostic CAL. It binds the `TargetPopulationSpec`; diagnostic `EvaluationManifestHash`, including encrypted family, fixture, oracle, generator, stratum, family-provenance-graph/similarity-audit, and `ClaimSlotInventory` hashes plus the frozen rejection taxonomy, thresholds, and one-to-one primary-to-reserve map; exact execution-environment and authorization/policy fingerprints; all seven condition and comparator material-profile fingerprints; the diagnostic `AllowedDifferencesManifest`; hashes of `U`, `K`, `R_S`, `R_C`, all prompts and skills; endpoints, denominators, margins, graders/rubric, assignment schedule, budget, failure taxonomy, and analysis code; and the inline nomination/promotion rule. It also commits both possible `I0`-incumbent and `I1`-incumbent adaptive-policy templates—or one parameterized equivalent—including the feature extractor/router skeleton, ordered predicate-to-branch map, branch candidate predicates, and deterministic leaf promotion/disable transformation. Diagnostic CAL may only fill those committed parameters, nominate `I1`, estimate forced-condition contrasts, and inform the predeclared feasibility decision; it cannot redesign the policy after outcomes.

Every exact `LeafQualificationProgram` has its own signed `LeafFreezeManifest` before that leaf's CAL is decrypted. It binds the leaf's target-population and predicate/workflow/risk stratum, encrypted CAL and TEST evaluation manifests with family/fixture/oracle/generator, family-provenance-graph/similarity-audit, and `ClaimSlotInventory` hashes and the same frozen replacement controls, candidate and named simpler-incumbent material profiles, execution and authorization profiles, `AllowedDifferencesManifest`, applicable `HandoffAssayManifest`, endpoints, denominators, margins, schedules, quota, failure rules, and analysis code. A leaf whose TEST set, comparator, profile, or predicate was not committed before its CAL cannot become production-reachable.

For `I1`, the leaf manifest is co-committed with the diagnostic manifest: the diagnostic `I0`/`I1` observations are its declared CAL, and its disjoint 144-session TEST set is encrypted and committed before diagnostic reveal. It cannot choose a new TEST after nomination. An `S1` or `C1` leaf uses its later leaf-enriched CAL, so its leaf manifest freezes after `I*` resolves but before any plaintext or outcomes from that leaf CAL are available.

After the diagnostic decision and applicable leaf evidence mechanically resolve `I*` and enabled branches, a signed `PolicyFreezeManifest` commits the final adaptive-policy fingerprint; all leaf fingerprints/evidence IDs, including the diagnostic evidence ID for an `I0` incumbent; the exact `B0` and `I*` comparator material profiles; execution and authorization profiles; the policy `AllowedDifferencesManifest`; target-population definition, prevalence, and membership-suite hash; encrypted policy-CAL and TEST `EvaluationManifestHash` values, including family, fixture, oracle, generator, stratum, family-provenance-graph/similarity-audit, and `ClaimSlotInventory` hashes plus the frozen rejection taxonomy, thresholds, and one-to-one primary-to-reserve map; graders/rubric; endpoints, denominators, margins, schedules, budgets, failure taxonomy, and analysis code. These encrypted holdout commitments exist before policy-CAL decryption. Policy CAL may estimate variance/power and operational overhead; it cannot tune the frozen policy. Only the transformation committed in the diagnostic manifest may produce the policy manifest.

Before any experimental arm in a block runs, oracle validation may reject a primary family only through a frozen machine-checkable code and threshold: fixture corruption, grader nondeterminism beyond tolerance, license failure, oracle inability to reach the declared feasible score, or source-packet hash mismatch. Rejection automatically selects that primary's one-to-one mapped reserve in the same stratum; the custody authority cannot choose among reserves. If the mapped reserve fails, the block does not begin. Every measurement, rejection, mapping, and substitution is committed and later published.

After any experimental arm starts, there are no task substitutions. Agent, driver, timeout, malformed-output, and verification failures are intention-to-treat outcomes. Only a predeclared, machine-coded condition-independent infrastructure failure outside all arms and before a scorable result may invalidate the complete paired repetition symmetrically; the frozen retry schedule may repeat that whole unit without examining condition outcomes, never one arm alone. Otherwise the block is incomplete rather than selectively excluded.

For every block, the custody authority's signed execution and reveal receipts enumerate the committed primary/reserve mappings, validation measurements, executed family/fixture hashes, complete-pair retries, and exclusions in order. They prove that every substitution was the unique mapped pre-arm reserve and that every post-start invalidation was symmetric and taxonomy-valid. An undeclared, discretionary, result-aware, one-arm, out-of-order, or newly generated replacement invalidates the block.

Any semantic prompt, skill, example, rubric, trigger, router, or population-predicate change after diagnostic CAL access creates a new condition version and requires new disjoint diagnostic and policy CAL. A change limited to the final policy after policy CAL requires a new policy version, disjoint policy CAL, and new TEST. Authors may continue non-semantic editorial documentation work but do not receive task-level CAL outcomes while the condition remains eligible for TEST. The same qualifying custody authority may hold both CAL blocks and TEST only when no forbidden revision occurs and the attestation remains valid. If no eligible unpaid human custodian or qualifying zero-cost external service exists, the project may publish public DEV evidence but cannot open neutral CAL or TEST or issue a confirmatory recommendation.

Top-level arm-session counts are not resource units and do not include every internal or assay model invocation. Before a block, a signed `QuotaCostManifest` enumerates every inference resource source and selects an explicit branch for each:

- `hosted-included-quota` converts each arm's maximum model calls, every §8.3 sidecar invocation, model/premium multipliers, and token ceilings using the host's authoritative quota unit. It commits `Q_block = ceil(1.20 × Σ q_max(all top-level arms, internal calls, and sidecars))`, the 20% reserve, account and authorization fingerprint, renewal-window allocation and schedule, authoritative quota source or account-owner attestation where the host exposes no receipt, and hard-disabled billing fallback/overage.
- `local-owned-capacity` binds the exact already-owned or authority-administered hardware/runtime profile; converts every top-level, internal, and sidecar invocation into frozen token, accelerator/CPU-time, memory, storage, and wall-clock maxima; commits a scheduled capacity window plus 20% reserve; sets billing and renewal-window fields to `not-applicable`; and binds enforced network denial plus proof that no rented worker or other billing-bearing inference channel exists.

A mixed block carries one entry per source and all entries must pass independently; incomparable hosted and local units are never collapsed into a single number. Unknown conversion, an uncovered hosted renewal window, insufficient local capacity, a pay-as-you-go path, or an unreserved source fails preflight. Session count alone never certifies affordability.

The representative operational program is 168 diagnostic-CAL plus 72 policy-CAL plus 216 TEST top-level arm sessions—456 per target population—and oracle/prevalidation work has a separate worst-case resource line. Evaluating a diagnostic-CAL-nominated `I1` for promotion adds its dedicated 144-session TEST regardless of outcome. Each other complexity-adding leaf lacking exact prior qualification adds a 48-session CAL and 144-session TEST block, or 192 top-level arm sessions, before policy CAL. Each `S1` or `C1` leaf additionally adds `24 × h_b` CAL plus `72 × h_b` TEST sidecar synthesis invocations—`192` total when `h_b = 2`, or `288` when `h_b = 3`—under a separate handoff line in the same quota manifest. A four-arm 288-session TEST receives its own manifest and may replace only exact matching leaf TEST observations; every leaf CAL remains counted, and the four-arm block does not replace a sidecar unless it contains the exact frozen handoff assay. Populations are staged separately. If every required hosted-quota and local-capacity entry cannot cover the complete manifest and reserve without incremental charges, the block does not start; TEST remains sealed and the release stays conformance-only or pilot.

Agent-caused timeout, malformed output, unsupported finding, or verification failure remains an intention-to-treat outcome. No result-dependent stopping, task replacement after arm start, or holdout reuse after a condition change is allowed. All TEST arms complete before reveal. The parent MahaBench blocks, custody, `144` sessions per domain, two-sided `α = 0.025` power rule, and external MPID of `0.10` remain unchanged.

External-baseline MahaBench comparisons remain separate. Beating an external system does not prove delegation added value over inline execution.

## 11. Qualification registry

The parent claim registry's evidence-status enum remains authoritative. Delegation Assurance extends each append-only record with orthogonal compatibility, recommendation, and validity fields; it does not replace or reinterpret the parent status.

### 11.1 Compatibility

- `unprobed`
- `unknown`
- `unsupported`
- `conformant`

`unprobed` means no valid observation exists; `unknown` means required observations are incomplete, expired, or conflicting; `unsupported` means a required probe failed; and `conformant` means every required mechanical contract passed.

### 11.2 Evidence

- `unverified`
- `mechanically-verified`
- `pilot`
- `confirmatory`

`mechanically-verified` may support a conformance statement but never a task-performance recommendation. Legacy `untested` maps to `unverified`; `pilot` and `confirmatory` retain their parent meanings.

### 11.3 Recommendation

- `none`
- `inline-preferred`
- `inline-skill-preferred`
- `specialist-preferred`
- `council-preferred`
- `adaptive-policy-preferred`
- `simpler-mode-preferred`
- `do-not-route`
- `no-qualified-mode`

Each recommendation carries `applies_to`, `selected_strategy`, and `displaced_strategy` fields. `none` means no performance recommendation exists. The five positive `*-preferred` values name the selected shipped strategy or policy for the exact scope. `inline-preferred` identifies managed `I0`, never `B0`, and originates from `DA-TEST-O-1` only when `I*=I0` and the preregistered `I0 − B0` gates pass; `inline-skill-preferred` requires the dedicated confirmatory `I1 − I0` evidence and cannot originate from the three-arm policy TEST. A favorable `B0` comparison can create a non-routable evaluation result and `do-not-route` for its managed comparator, not a positive route to bare native execution. `simpler-mode-preferred` is a candidate-level negative result that preserves the named managed incumbent. `do-not-route` excludes a named candidate after a safety, noninferiority, handoff, or contract failure. `no-qualified-mode` is scope-level and means no managed strategy can satisfy mandatory contracts.

Routing precedence is: `validity=current`; then `compatibility=conformant`; then `evidence=confirmatory`; then no scope-level `no-qualified-mode`; then no candidate-level `do-not-route`; then a positive recommendation whose task predicate matches. Any failed prerequisite suppresses automatic delegation. Pilot evidence may inform an explicit forced experiment but never automatic routing.

When several current positive records match, resolution is deterministic:

1. collapse each append-only `supersedes` chain to its newest valid record;
2. require exact execution-environment, authorization/policy, budget, and workflow/risk matches;
3. choose the most-specific predicate by set containment, never merely the record with more prose fields;
4. allow a policy record to displace its named leaf record only when its confirmatory comparison explicitly proved policy benefit over that leaf for the same scope; otherwise choose the least complex passing strategy; and
5. if two non-superseding records remain incomparable, equally specific, or contradictory, fail closed to the current qualified inline incumbent and emit a registry-ambiguity receipt.

Record iteration order, creation time outside a supersession chain, and provider/model name never break a tie.

### 11.4 Validity

- `current`
- `stale`

Every recommendation contains its exact task predicate, workflow and risk stratum, candidate execution fingerprint, selected adaptive leaf fingerprints when applicable, comparator material-profile fingerprints, authorization/policy fingerprint, `AllowedDifferencesManifest`, selected probe-observation IDs, target-population hash, evaluation-manifest hash, budget regime, metrics, intervals, evidence IDs, limitations, and expiry triggers. Public projections replace the local account/tenant pseudonym with non-identifying policy and entitlement classes. Adaptive records additionally list every branch predicate, enabled state, independent support count, branch intervals, evidence record, and disable reason. A conformant profile with only unverified or mechanically verified evidence is compatible but has `recommendation=none`.

### 11.5 Record integrity, migration, drift, and inheritance

Records carry `schema_version`, `canonicalization_version`, `hash_algorithm`, `record_id`, `created_at`, `supersedes`, and provenance hashes. Material identifiers are namespaced as `mpf:<canonicalization-version>:<algorithm>:<digest>`. Migrations never overwrite or silently rehash an existing record: a deterministic migrator emits a linked record and its own code hash. Status may carry forward only when every material, qualification, and expiry field is losslessly equivalent and a machine-checkable equivalence receipt exists; otherwise evidence becomes `unverified`, validity becomes `stale`, and routing fails closed. Unknown fields are preserved, and an older reader may display but not route from a newer schema.

No record is routable as a bare data object. Activation requires a `QualificationRecordEnvelope` signed by a non-exportable qualification-engine release key listed in a versioned `QualificationSignerRegistry`. The envelope binds the canonical record, complete evidence-DAG root, analysis build and output hashes, freeze manifests, exact `CustodyAuthorityRegistry` entry/version, each block's `CustodyActivationReceipt` and `CustodyClosureReceipt`, custody-authority execution/reveal receipts, signer scope, issuance/expiry, and supersession target. Before activation, the controller resolves and verifies the complete DAG, every signature and key scope, the two independent custody-onboarding receipts and their disclosed-principal conflict checks, current authority entry, full-duration lease coverage, independent audit-sink or immutable-policy evidence, both closure-review receipts, exact candidate/comparator/population/profile bindings, and evidence-status transition; a hash that merely points to missing, gapped, self-authored, self-attested, expired, revoked, or conflicting custody evidence is invalid.

The active qualification and custody-authority registries are controller-owned storage outside candidate workspaces, driver sandboxes, generated bundles, and model-visible paths. Activation is an atomic controller transaction; candidate deltas and adapters cannot write either registry. Public qualification projections use a separate signing domain, set `routable=false`, contain only the source-envelope hash and sanitized fields, and are rejected by the activation parser. Unsigned, unknown/revoked-key, incomplete-DAG, public-projection, candidate-authored, self-onboarded, or scope-mismatched input resolves to `recommendation=none`, emits a security receipt, and cannot participate in precedence. Qualification or custody key rotation and revocation are append-only and make affected active records stale until re-signed from still-valid evidence under a current authority entry.

A `PortableQualificationEnvelope` is not a public projection. It uses an authorized portable-class signing scope and binds the complete multi-account evidence DAG, `PerformancePolicyEquivalenceClassFingerprint`, independent account-cluster and held-out-account results, heterogeneity test, minimum validity/expiry, and local activation requirements. It contains no routable account pseudonym by design and can enter the active registry only through a valid `ClassActivationReceipt` that binds it to the new account's exact local material profile. An exact-account envelope cannot be converted into a portable one by redaction.

Deployment uses dual-read, shadow migration, count/hash reconciliation, and an atomic active-version switch with rollback. Conflicts, partial migration, or failed reconciliation leave the old registry authoritative and suppress new recommendations.

A material fingerprint change or observation expiry makes the applicable record `stale`; observation expiry does not change the material fingerprint itself. For an IA2/IA3 material profile, a fresh observation may restore mechanical eligibility without rerunning performance only when the probe-suite hash is unchanged, every required result is equivalent or stricter, no invalidating trigger fired, and the original record explicitly permits observation renewal. The new record supersedes rather than overwrites the old one. IA0/IA1 run-window evidence cannot be extended this way. Exact-account performance recommendations do not inherit across material profiles or accounts. The sole cross-account path is the preregistered portable-class procedure above; capability-tier or other profile generalization likewise requires a preregistered multi-profile study with held-out-profile validation.

## 12. Provider- and model-neutral execution

### 12.1 Driver interface

A `BundleAdapter` and an `ExecutionDriver` are distinct contracts. A BundleAdapter is a pure compiler/installer that converts canonical manifests into host agent, skill, prompt, or configuration files. It performs no inference, owns no live process or remote session, and makes no cancellation or runtime-capability claim.

An ExecutionDriver controls a documented automatable execution surface. It probes, starts runs, normalizes controller-observed events, enforces tool boundaries, reports the actual resolved identity, and attempts cancellation:

```ts
interface ExecutionDriver {
  readonly id: string;
  resolve(
    intent: ProfileIntent,
    services: ResolutionProbeServices,
  ): Promise<ResolvedProfileInputs>;
  probe(
    profile: MaterialProfile,
    services: ResolutionProbeServices,
  ): Promise<ProbeObservation[]>;
  prepare(
    request: ExecutionRequest,
    services: ControllerServices,
  ): Promise<PrepareResult>;
  dispatch(
    prepared: PreparedRun,
    token: DispatchToken,
    signal: AbortSignal,
  ): Promise<DispatchResult>;
}

interface ResolutionProbeServices {
  openBrokeredTransport(request: BrokeredTransportRequest): Promise<BrokeredTransport>;
  readPublicProbeArtifact(ref: ArtifactRef, sessionHash: string): Promise<VerifiedBytes>;
}

interface ControllerServices extends ResolutionProbeServices {
  readArtifact(ref: ArtifactRef, requestHash: string): Promise<VerifiedBytes>;
  acquireWorkspace(request: WorkspaceLeaseRequest): Promise<WorkspaceLease>;
  finalizeWorkspace(
    lease: WorkspaceLease,
    disposition: "discard" | "propose-delta",
  ): Promise<WorkspaceFinalizationReceipt>;
  beginArtifact(request: ArtifactWriteRequest): Promise<ArtifactWriteHandle>;
  appendArtifact(handle: ArtifactWriteHandle, chunk: Uint8Array): Promise<void>;
  finalizeArtifact(
    handle: ArtifactWriteHandle,
    metadata: ArtifactFinalizeMetadata,
  ): Promise<ArtifactRef>;
  abortArtifact(handle: ArtifactWriteHandle, reason: string): Promise<ArtifactAbortReceipt>;
  bindCredential(
    ref: CredentialRef,
    origin: Origin,
    requestHash: string,
  ): Promise<CredentialBinding>;
  authorizeTool(request: ToolAuthorizationRequest): Promise<ToolAuthorizationReceipt>;
  authorizeEffect(request: EffectAuthorizationRequest): Promise<EffectAuthorizationReceipt>;
  reserveUsage(request: UsageReservation): Promise<UsageReservationReceipt>;
  debitUsage(request: UsageDebit): Promise<UsageDebitReceipt>;
}

interface BrokeredTransport {
  readonly sessionId: string;
  readonly bindingReceipt: string;
  exchange(
    payload: ArtifactRef,
    signal: AbortSignal,
  ): AsyncIterable<BrokeredTransportEvent>;
  close(): Promise<TransportCloseReceipt>;
}

interface ResolvedProfileInputs {
  readonly environment: ExecutionEnvironment;
  readonly authorizationPolicy: AuthorizationAndPolicyProfile;
  readonly identityReceipt: string;
  readonly policyReceipt: string;
}

interface RunHandle {
  readonly runId: string;
  readonly requestHash: string;
  readonly acceptanceReceipt: string;
  events(): AsyncIterable<ExecutionEvent>;
  cancel(reason: string): Promise<CancelResult>;
}

interface PreparedRun {
  readonly runId: string;
  readonly requestHash: string;
  readonly acceptanceReceipt: string;
  readonly resolvedArtifactRefs: readonly ArtifactRef[];
  readonly budgetDispositions: readonly BudgetDisposition[];
}

type PrepareResult =
  | { state: "prepared"; prepared: PreparedRun }
  | { state: "rejected"; rejection: DriverRejection };

type DispatchResult =
  | { state: "running"; handle: RunHandle }
  | { state: "reattached"; handle: RunHandle }
  | { state: "rejected"; rejection: DriverRejection };

interface DriverRejection {
  readonly phase: "prepare" | "dispatch";
  readonly code: string;
  readonly unsupportedFields: readonly string[];
  readonly requestHash: string;
  readonly receipt: string;
}

type CancelResult =
  | { state: "confirmed"; receipt: string }
  | { state: "local-terminated"; receipt: string }
  | { state: "unconfirmed"; receipt: string }
  | { state: "failed"; receipt: string };
```

`ExecutionRequest` is a versioned canonical schema containing: request ID, attempt ID, idempotency key and optional `retryOf`; immutable content-addressed `ArtifactRef`s for the task contract, repository/source snapshot, context-package manifest and members, compiled prompt/messages, and output contract; execution-candidate and selected-leaf fingerprints; required probe-observation IDs; tool, permission, egress, and credential-reference policy hashes; `budgetMode`; budget-allocation ID and per-dimension requested ceilings; deadline; fallback/exit policy hash; authorized side-effect classes; and controller signature. Each `ArtifactRef` includes digest, canonicalization/media type, byte length, and sensitivity class. Small inline payloads are permitted only as canonical bytes embedded in and covered by the request signature. Credential references are opaque IDs, never secret values.

Execution drivers are part of a small release-attested TCB but never load into the controller process. Only repository-shipped, reproducibly built driver binaries whose signatures and build provenance match the active `DriverSignerRegistry` may run; arbitrary third-party plugins are unsupported. Each driver runs in a separate OS sandbox/broker process with an empty inherited environment and descriptor table except authenticated controller IPC. By default it has no home-directory or arbitrary filesystem access, keychain, host IPC/Mach services, Docker or SSH-agent sockets, Unix sockets, raw network, or process-discovery authority. All direct transport, artifact, workspace, credential, tool, and effect access crosses the brokered services below. A separately sandboxed pinned host CLI may receive only the explicit exceptions recorded by its host-managed binding and egress disposition. Signature trust does not substitute for sandbox enforcement.

`ResolutionProbeServices` is scoped to a signed public-probe or resolution session with its own egress policy, deadline, quota, and journal. `openBrokeredTransport` accepts only a declared protocol/origin/method, credential-binding mode, payload-enforcement disposition, redirect rule, and session/request hash. The controller broker owns direct HTTP, loopback, or Unix-socket syscalls for OpenAI-compatible and Ollama transports; performs DNS/origin/redirect and payload checks; writes streamed input/output through artifact refs; and emits signed transport events and usage receipts. A host-CLI broker instead launches only the pinned attested CLI under its separately declared sandbox, credential, network-origin, and payload-scope exceptions. Resolution and probes cannot bypass this broker, use private task data, or inherit later execution authority. Execution receives the same broker through request-bound `ControllerServices`.

The per-run `ControllerServices` object is a narrow capability surface bound to the signed request hash. `readArtifact` resolves only refs enumerated by that request or emitted by that request's artifact writer and returns bytes only after digest, size, media type, and sensitivity checks pass. Every input payload must match its signed ref; mismatch or unavailable content is a typed pre-execution failure. The driver receives neither a general artifact-store handle nor path-based ambient access.

Generated model output, tool results, traces, and files cross the controller boundary only through the request-scoped artifact writer. `beginArtifact` fixes purpose, expected media type, sensitivity ceiling, and hard byte limit; append is streaming and quota-debited; finalize performs digesting, sensitivity inheritance, malware/content-policy checks where applicable, and storage commitment before returning an immutable `ArtifactRef`. Events may cite only finalized refs. Partial, abandoned, oversized, misclassified, or scanner-failed writes are aborted or quarantined, are not resolvable as evidence or synthesis input, and produce cleanup receipts; their bytes are deleted under the frozen retention policy after forensic metadata is recorded. Cancellation aborts every open writer.

Repository work uses a controller-created `WorkspaceLease`, never a general filesystem path supplied by the driver. The lease is bound to the request and exact source-snapshot ref; materializes an immutable base plus a copy-on-write layer; returns one scoped working-directory handle; and fixes readable and writable target allowlists, symlink/mount/device rules, process and network policy, byte/inode limits, deadline, and cleanup key. The task-facing filesystem and tools receive only that working directory inside the declared OS sandbox. Reads outside the lease, host-device access, symlink escape, and writes to the immutable base fail closed. A preauthenticated CLI may additionally receive only the separately fingerprinted, read-only runtime/config and non-enumerable keychain handles declared by its `host-managed-binding`; those launch capabilities are not visible as task files or general tools. If the platform cannot enforce that separation, the profile is unsupported for workspace-bearing tasks rather than silently granted ambient access.

Every workspace mutation enters an append-only change/effect journal. Finalization either discards the layer or seals the journal and canonical patch/file delta as immutable artifact refs in a signed `WorkspaceFinalizationReceipt`; cancellation and failure default to discard after evidence capture. Applying a proposed delta to the user's live workspace is a separate controller-authorized external effect with exact base-hash preconditions, conflict detection, transactional staging, rollback material, and an application receipt. A driver or model cannot commit, apply, or roll back the live workspace directly.

Credential binding has three explicit modes. `controller-transport-lease` is a non-exportable, origin-, method-, policy-, request-, and deadline-bound capability for a transport owned by the Council controller; neither driver code nor model context can read the underlying secret, and a redirect requires a new authorization. `host-managed-binding` launches a pinned, isolated, already-authenticated CLI profile whose third-party process reads its own keychain/config credential; Council never reads, copies, injects, or represents that secret as a lease. `none` is permitted only when a signed broker probe proves the request has no credential reference, authorization header, URI userinfo, client certificate, credential-bearing environment/config/keychain handle, or other credential channel.

Every binding receipt independently records `originEnforcement` as `controller-enforced`, `host-tcb-attested`, `advisory`, or `unavailable`, and `credentialContainment` as `controller-enforced`, `host-tcb-attested`, `not-applicable-no-credential`, `advisory`, or `unavailable`, with the exact binary/build signature, launch-policy hash, host/profile identity, credential-subsystem and keychain/config ACL evidence where applicable, probe receipts, and validity interval. Managed execution requires `controller-enforced`, `host-tcb-attested`, or a valid `not-applicable-no-credential` credential-containment receipt; `advisory` and `unavailable` are non-routable for every managed request, including public-data requests. `not-applicable-no-credential` is valid only with `credentialBindingMode=none` and the negative broker probe above; introducing any credential channel invalidates the profile before routing. A `host-tcb-attested` disposition means the pinned vendor-signed CLI binary, its isolated launch policy, and the credential/keychain subsystem are explicitly inside the attested trusted computing base. Conformance uses a disposable canary credential and controlled endpoint to prove that the exact build does not expose raw credential material through serialized model payloads, task-visible child environments or arguments, stdout/stderr, artifact writers, or workspace files. Binary, signature, credential ACL, launch-policy, or probe drift invalidates the disposition before routing.

The Council's credential-containment claim covers escape from that attested boundary; compromise of the attested host CLI or credential subsystem itself is outside the guarantee and must never be described as Council-enforced secrecy. Payload authorization is a separate decision: a `controller-inspected`, `host-whole-scope-enforced`, or `opaque` payload disposition neither proves nor weakens credential containment. Thus an opaque CLI may be eligible for public, field-unrestricted task data only after it independently has `host-tcb-attested` credential containment, while an inspected payload path cannot rescue advisory credential isolation. A host-managed binding is also ineligible whenever the task's egress, sensitivity, tenancy, or origin policy requires a stronger boundary than the receipt proves. Native or prompt-pack use outside these rules is unmanaged and carries no Council credential-containment claim. Tool calls, external effects, reservations, and debits require the corresponding request-bound service receipt before dispatch; denial, service loss, or post-terminal use fails closed. Services expire on terminal state or cancellation and expose no authority beyond the exact request.

Every inference profile also declares `payloadEnforcement`: `controller-inspected`, `host-whole-scope-enforced`, or `opaque`. `controller-inspected` means every serialized inference payload traverses a Council-controlled proxy or transport and receives the exact per-transmission field check. `host-whole-scope-enforced` means an attested host boundary restricts destination and access, but Council cannot see individual serialized payloads; it is eligible only when the user preauthorizes the entire declared context/workspace sensitivity scope for that destination with no field-level exclusions. `opaque` means neither payload visibility nor a whole-scope host guarantee is proven and is eligible only for public data with no field-restriction requirement. Default-`restricted`, confidential, tenant-constrained, or field-restricted work therefore cannot use an opaque subscription CLI. A CLI whose internal transport bypasses an expected proxy is downgraded to `opaque` and fails the affected request.

Each requested budget dimension resolves in the acceptance receipt to `{ enforcement: "hard-enforced" | "bounded-local" | "advisory" | "unavailable", ceiling?, unit, provenance, receipt? }`. `hard-enforced` means the full aggregate ceiling is rejected before overshoot; `bounded-local` limits only Council-controlled local work and never implies a remote inference cap; `advisory` is a requested host limit without enforcement proof; and `unavailable` means no meaningful limit or observation exists. `strict-resource-matched` accepts only `hard-enforced` for every declared aggregate dimension. `operational` may accept weaker dispositions only when the task's frozen risk, permission, egress, data-sensitivity, billing, and side-effect rules permit them; overshoot and unknown use remain intention-to-treat outcomes. Budget weakness never relaxes tool, credential, egress, authorization, or destructive-effect enforcement.

The controller canonicalizes, hashes, and signs the request before `prepare`. Preparation may resolve refs, validate policy, acquire revocable leases, and create a run ID, but it cannot start inference, a host process, a tool, or an external effect. `PrepareResult` returns either a prepared record with a signed `DriverAcceptanceReceipt` bound to the request hash, ordered ref-to-resolved-hash list, resolved identity, payload and per-dimension enforcement dispositions, driver build, and run ID, or a typed `DriverRejection` naming every unsupported or unresolved mandatory field. After fsync of dispatch intent, the controller issues a signed, single-use `DispatchToken` bound to the request hash, run ID, idempotency key, prepared-receipt hash, and expiry. `DispatchResult` returns a new or reattached handle or a typed rejection. `dispatch` accepts only that prepared record and token; reusing the exact token reattaches or returns the existing handle and can never launch again, while a mismatched or second token is rejected.

Reusing an idempotency key with the same request hash returns the durably recorded prepared handle, live handle, or terminal receipt and cannot dispatch new work; reuse with a different hash is rejected. A permitted retry has a new attempt/key, links the prior terminal or reconciliation receipt, preserves all semantic refs and hashes, and starts only after external effects reconcile. If dispatch may have occurred but cannot be proved absent or safely reattached, automatic retry is forbidden.

The Council extends the parent Lineage Ledger with a durable, append-only `RunJournal` and write-ahead log. Its monotone states are `request-recorded → resources-reserved → driver-prepared → dispatch-intent-recorded → running → cancelling|reconciling → terminal`; `dispatch-unknown` and `failed-unreconciled` are fail-closed substates, never success terminals. Each transition is compare-and-swap/idempotent on request hash, run ID, and idempotency key and is durably committed before the next irreversible action. Allocation reservations, capability/credential/workspace/artifact lease IDs, driver acceptance, dispatch intent, every event, tool/effect authorization, artifact finalization, workspace delta, cancellation attempt, reconciliation, and terminal receipt share that journal lineage. A success terminal commits only after usage, artifacts, workspaces, and effects reconcile.

Those names are internal journal state, not new parent handoff statuses. `dispatch-unknown` or `failed-unreconciled` always emits external handoff `status=failed` and terminal event `run_failed`, with a separate `runJournalState` field carrying the internal substate and reconciliation receipt. The addendum does not extend the parent status enum.

Host CLI processes launch through a durable supervisor that owns the run ID, child identity, scoped workspace, stdout/event capture, and cancellation handle across controller restart. Remote transports use a provider idempotency primitive when one is proven; without it, a lost dispatch acknowledgment becomes `dispatch-unknown` and is never replayed automatically. External effects record a durable intent and unique effect key before authorization or dispatch, then a result/reconciliation receipt afterward. These controls provide fail-closed recovery, not a false universal exactly-once claim.

On startup, recovery scans every nonterminal journal. Reservations remain consumed; new service calls and credential/tool capabilities remain revoked until reauthorized; surviving supervised processes are reattached or cancellation is attempted; open artifact writers and COW workspaces are quarantined; and uncertain remote runs or external effects block retry, live-workspace apply, recommendation use, and budget release. Every orphan receives external `status=failed`/`run_failed` with `runJournalState=failed-unreconciled` until a signed reconciliation receipt proves terminal provider/process state, effect state, artifact cleanup, workspace cleanup, and final usage or explicitly records what remains unknowable.

`ExecutionEvent` is a versioned discriminated union. Every event carries `schemaVersion`, `requestHash`, `runId`, a strictly increasing `seq`, timestamp, execution-candidate fingerprint, selected leaf material-profile fingerprint, and allocation ID. Required variants are:

- `run_started`, containing resolved identity and selected probe-observation IDs;
- `tool_requested` and `tool_result`, correlated by a unique call ID and immutable finalized argument/result `ArtifactRef`s;
- `artifact_observed`, containing an immutable finalized `ArtifactRef` and provenance refs;
- `workspace_changed`, containing the lease ID, journal sequence, target-relative path hash, operation class, before/after refs, and no unsealed file bytes;
- `external_effect`, containing effect ID, authorization receipt, target, dispatch state, and reconciliation state;
- `usage`, carrying each counter as `{ availability: "authoritative" | "host-reported-estimate" | "unavailable", value?, unit?, provenance, receipt? }`; and
- exactly one terminal `run_completed`, `run_failed`, or `run_cancelled`, containing the finalized output `ArtifactRef` when available; `workspace: { state: "not-acquired" } | { state: "finalized", receipt, proposedDeltaRefs? }`; aborted/quarantined artifact receipts; the aggregate typed-availability usage vector; outstanding-effect list; and reconciliation result.

Operational drivers must report unknowns rather than invent counters. `host-reported-estimate` and `unavailable` satisfy event-schema honesty but make resource-matched eligibility fail; §10.2 requires every relevant counter to be `authoritative` with a receipt.

Unknown required event versions, sequence gaps or duplicates, wrong request/run/profile/allocation IDs, an unpaired tool result, events after a terminal, conflicting terminals, or a stream ending without a terminal produce `run_failed` with `runJournalState=failed-unreconciled`. Such a run cannot promote findings or satisfy resource/cancellation claims. Late remote effects are recorded for reconciliation but cannot re-enter the Council event state machine.

Cancellation is idempotent and deadline-bounded. Closing a stream, discarding a response, or killing a hosted CLI does not prove that remote inference or queued work stopped. Once cancellation starts, the controller revokes further Council tool authorization and rejects late events from affecting Council-controlled findings. Already dispatched external effects require explicit reconciliation and keep the run unsafe until their state is known.

`local-terminated` satisfies cancellation conformance only for a proven local-contained execution with enforced network denial, no remote worker, and no external side-effect channel. Hosted work requires a provider/host cancellation receipt and `confirmed`; otherwise it remains `unconfirmed`. An unconfirmed run makes the profile ineligible for secret-bearing, externally mutating, or strict-budget work until expiry and reconciliation.

Resolution runs under the public-probe egress policy. The driver returns only the resolved execution environment, redacted authorization/policy profile, and their receipts; it never constructs Council-owned prompts, router, condition permissions, or budgets. The controller verifies the receipts, combines both resolved subprofiles with its canonical `ConditionFingerprint` to construct and hash the `MaterialProfile`, and only then requests keyed probe observations. `prepare` and `dispatch` are unavailable until a resolved profile and required unexpired observations exist, eliminating a circular “probe an already resolved profile” assumption.

The Council runtime owns task contracts, budgets, evidence gates, receipts, and qualification. Drivers translate controller-observed transport and host events; they do not redefine safety semantics. A host can have a BundleAdapter without an ExecutionDriver and is then installable/native-conformant, not Council-managed.

### 12.2 Version-1 driver scope

Version 1 implements BundleAdapters for every pinned host surface promised in the parent design: Claude Code, Codex, Copilot/VS Code, Kiro CLI, and Kiro IDE. IDE-only surfaces remain install/native-conformance targets until a documented bridge passes the execution-driver suite.

Version 1 implements ExecutionDrivers for:

- separately pinned and probed host CLI surfaces that expose documented automation, observable lifecycle events, and bounded cancellation;
- `openai-compatible-chat` for Chat Completions-style endpoints that pass the declared compatibility probes; and
- `ollama` for explicit local-runtime behavior not faithfully represented by the generic protocol.

Direct/local protocol drivers use `controller-transport-lease` credentials when authentication is needed. An unauthenticated direct/local endpoint uses `credentialBindingMode=none` and is managed only after the signed no-credential broker probe yields `credentialContainment=not-applicable-no-credential`; it does not fabricate a credential-containment claim. Preauthenticated host CLI drivers use only `host-managed-binding`; their separately probed profile records keychain/config isolation, origin control, credential-containment disposition, and the independent payload-enforcement disposition. A host CLI cannot become managed while credential containment is `advisory` or `unavailable`, and it is automatically ineligible whenever task policy requires a stronger credential, origin, payload-visibility, or field-level boundary than the profile proves. A CLI driver for codebase work must also launch inside an enforceable `WorkspaceLease`; an authenticated CLI alone is not managed conformance.

There is no undifferentiated `host-native` runtime claim. Codex CLI, Claude Code CLI, and Kiro CLI profiles become managed only one surface/version at a time after their driver suite passes; a generated IDE bundle does not imply that result. Copilot CLI requires the separate future target ID and conformance work required by the parent design rather than being smuggled into the Copilot/VS Code bundle.

Responses-style semantics are an optional, separately probed extension rather than an assumption of the generic driver. Other local servers can use `openai-compatible-chat` when their measured behavior passes. llama.cpp and LM Studio profiles begin experimental through that transport; vLLM remains an experimental server profile in version 1. A direct driver is added only when protocol differences cannot be expressed through capabilities. A shared URL shape never implies semantic compatibility.

A host CLI may pass managed operational conformance while reporting usage as estimated or unavailable. That profile is explicitly `operational-only`; it cannot enter a resource-matched block or claim equal inference compute.

The zero-budget baseline does not add direct metered-provider API drivers. Existing hosted subscriptions remain behind their separately pinned host CLI drivers, and subscription credentials are never repurposed as API keys.

### 12.3 Degradation tiers

| Tier | Minimum capability | Available strategy |
|---|---|---|
| `managed-full` | Reliable structured events, tool results, cancellation, and native or runtime-managed delegation | `I0`, `I1`, `S1`, and `C1` as qualified |
| `managed-sequential` | Reliable single-session instruction following and structured handoff, but no safe parallel/native delegation | `I0`, `I1`, sequential `S1`, and sequential `C1` as qualified |
| `prompt-pack` | Basic text generation without reliable tools or structured events | `I0` or compact `I1`; all findings remain advisory unless an independent outer runtime verifies them |
| `unsupported` | Cannot satisfy instruction separation, minimum context, cancellation, or output-safety requirements | No execution |

Compact prompt packs preserve evidence and abstention language while removing orchestration that the profile cannot support. The compiler reports lost coverage. Effective authority is always the strictest applicable boundary: a prompt-only profile cannot create a verified finding. If an independent outer VT2/VT3 runtime later verifies a claim, the receipt attributes verification to that outer runtime rather than the prompt-pack profile.

### 12.4 Local and open-weight operation

- The Council connects local drivers only to explicit approved loopback endpoints and disables its own network tools by default. This does not prove the separately running model server is offline; doctor reports server/network properties it cannot enforce.
- Network tools remain disabled unless separately authorized for the task.
- No weights are downloaded automatically.
- Model files, licenses, acceptable-use terms, and redistribution rights remain outside the Apache-2.0 repository license and are recorded in local profile metadata.
- License records include runtime license, weight license identifier and source, license-text hash, revision/digest, commercial-use status, redistribution status, derivative/fine-tuning status, output-publication restrictions, and gating terms. Unknown redistribution rights fail closed for bundling.
- Public qualification artifacts report runtime, quantization, context, and hardware class without publishing private device identifiers.
- Hardware or quantization changes invalidate the exact performance profile when they alter context, tool behavior, latency budget, or output reliability.

## 13. Skill and prompt assurance

### 13.1 Trigger contracts

Every skill declares:

- positive activation examples;
- adversarial negative examples;
- tasks where inline work is preferred;
- required evidence and tools;
- maximum useful context and iteration budget;
- abstention conditions; and
- the qualification records supporting automatic activation.

Trigger evaluation reports precision, recall, harmful activation, unnecessary activation, and missed-benefit rates. A two-arm router test cannot claim trigger recall without forced-strategy counterfactuals.

### 13.2 Prompt requirements

The versioned canonical `AssuranceContract` is the sole Council-controlled semantic authority. Full, compact, and minimal prompt packs are generated projections, not independent specifications. Every non-negotiable clause has a stable ID, and the compiler emits a coverage map proving that every pack preserves evidence, abstention, finding-state, permission, egress, cancellation, budget, and claim-ceiling clauses.

All agent and skill prompts state:

- do not invent a finding to justify delegation;
- inspect direct evidence before asserting repository state;
- distinguish observation, inference, hypothesis, and verified finding;
- return no verified findings when evidence does not support one;
- do not treat another agent’s agreement as verification;
- return proposed ordering and references rather than adding factual synthesis prose outside verified finding fields;
- report unexamined coverage and tool failures; and
- stop at the declared budget or exit condition.

Progressive disclosure may remove examples, explanations, optional roles, and unsupported orchestration. It cannot weaken a non-negotiable clause, increase permissions or finding authority, or evade a stricter host policy. Conflicting generated packs fail the build. Degradation is monotone: fewer proven capabilities can only remove strategies and lower claim ceilings. The canonical contract, generated-pack, and clause-coverage-map hashes are material-profile inputs. Fine-tuning may improve wording but cannot alter or omit non-negotiable semantics.

## 14. Failure and fallback behavior

| Failure | Required behavior |
|---|---|
| Capability probe fails or drifts | Mark profile unsupported or stale; choose a qualified simpler strategy or return `blocked` |
| Structured output is malformed | One bounded schema-repair attempt without new substantive reasoning; then fail or return hypotheses only |
| Evidence locator is missing, stale, or non-supporting | Reject or quarantine the claim; never promote it |
| Verification is unavailable before execution | If required by the task or a mandatory surface, return `blocked`; otherwise mark only the affected finding `not-verifiable` and complete solely when the task contract permits the resulting partial coverage |
| An available verifier is invoked but fails, times out, or loses isolation | Return run status `failed` with diagnostic and changed-state receipts, preserving the parent §6.7 rule; the affected finding is `not-verifiable` |
| VT3 returns a valid signed `contradicts`, `insufficient`, `abstain`, or `conflict` verdict | Set the finding to `rejected`, `not-verifiable`, or `hypothesis`; complete only if the task contract permits partial verified coverage |
| Invoked VT3 times out, loses transport/independence, or returns an invalid signature | Return run status `failed` under the verifier-failure rule; do not substitute user approval or model agreement |
| Specialist fails | Report lost specialty coverage; do not silently substitute a persona |
| Handoff loses an approval, safety boundary, or dissent | Hard failure; synthesis cannot proceed as completed |
| Strict budget cannot be enforced or reconciled | Reject `strict-resource-matched`; never relabel the run operational after seeing an outcome |
| Operational advisory budget is reached, overshoots, or becomes unobservable | Attempt the bounded stop, preserve partial verified work, record actual disposition and unknowns, and fail any task whose frozen risk/billing/side-effect contract required a hard bound |
| Workspace lease cannot be enforced, escapes, conflicts, or cannot finalize | Stop the process, revoke tool/effect authority, quarantine the COW layer for bounded evidence capture, do not apply a delta, and return parent `status=failed`/`run_failed` with `runJournalState=failed-unreconciled` until cleanup and live-workspace state are proven |
| Controller or supervisor crashes, or dispatch/effect state is ambiguous | Recover from the durable RunJournal; retain reservations, revoke capabilities, quarantine open artifacts/workspaces, forbid replay or apply, and return parent `status=failed`/`run_failed` with the internal substate until signed reconciliation |
| Exit limit is reached | Stop, preserve partial verified work, and report residual uncertainty |
| Hosted or local provider changes behavior | Invalidate qualification and require probes before automatic delegation resumes |
| User forces an experimental strategy | Execute only if eligible and safe; label the receipt and suppress recommendation use |

No fallback may expand permissions, enable billing, transmit new data, or change the task contract.

## 15. Security and privacy

- Every task artifact and derived context item is labeled `public`, `internal`, `confidential`, or `restricted`; missing classification defaults to `restricted`, and derived data inherits the highest input sensitivity. Summarization, encoding, or hashing does not declassify data. A deterministic declassification policy and receipt are required.
- Every external destination—including hosted inference, MCP, web tools, telemetry, logs, and cross-model review—has an `EgressPolicy` covering allowed labels, exact endpoint origin and purpose, allowed fields, region, retention, training use, telemetry, credential channel, and authorization expiry.
- Before context packaging and every Council-controlled transmission, the controller-owned Egress Gate compares the exact payload manifest with the destination policy. Denied, unknown, redirected, or origin-mismatched requests are blocked. A host-managed CLI's internal serialization is not falsely described as inspected: preflight applies its frozen payload-enforcement disposition, permits whole-scope transmission only under explicit whole-scope authorization, and restricts opaque transmission to public, field-unrestricted data. Non-public data requires explicit destination authorization; generic endpoints use an origin allowlist, disable redirects by default, and never forward credentials across origins.
- Retrieved code, documents, model cards, tool output, and provider messages are untrusted data.
- Evidence locators are root-contained and content-addressed; path traversal, symlink swaps, forged command output, and stale snapshots are tested.
- Codebase processes run only in request-bound COW workspace leases with target allowlists and OS-enforced path, symlink, mount, device, process, and network policy. Live-workspace application is a separately authorized transactional effect with base-hash preconditions and rollback receipt.
- Release-signed drivers run out of process with no ambient filesystem, inherited descriptors, host IPC, agent sockets, keychain, or raw network. Controller-owned transports use non-exportable controller leases whose secrets cannot enter driver/model context. A preauthenticated host CLI is managed only with `host-tcb-attested` credential containment; the exact CLI, launch policy, and credential subsystem are then an explicit attested TCB, and Council guarantees containment outside—not compromise of—that TCB. Advisory or unavailable containment is non-routable. Credential containment and task-payload authorization remain independent gates.
- Driver-produced outputs enter the controller only through request-scoped, size-bounded artifact writers. Sensitivity inheritance and required scanners run before finalization; partial, oversized, or scanner-failed bytes remain quarantined and cannot become evidence, verification input, or public receipt content.
- Redaction occurs before driver serialization and logging. Its receipt identifies the policy and removed field classes without retaining secret values.
- Public receipts exclude secrets, private account identifiers, absolute home paths, raw prompts containing private code, and hidden reasoning.
- Verification commands run in the task’s authorized isolation boundary.
- Model/provider account scope, entitlements, region, telemetry, retention, training-use, and data-policy state appear in the redacted authorization/policy subprofile; incompatible or unknown privacy requirements make the strategy ineligible for affected data.
- No fallback broadens destination, retention, telemetry, network, or sensitivity allowances. The Council can enforce its own loopback connection and network-tool policy but never claims that an external local server is offline without an enforced network-deny probe.
- A same-provider or same-model reviewer is never described as independent verification.

## 16. Testing strategy

### 16.1 Mechanical tests

- strict schema and canonical-hash tests for material profiles, probe observations, findings, evidence, routing, populations, and qualification records;
- qualification-envelope tests for signer scope/revocation, complete evidence-DAG resolution, current `CustodyAuthorityRegistry` entry and per-block `CustodyActivationReceipt`/`CustodyClosureReceipt` binding, full-duration lease/audit coverage, public-projection non-routability, candidate-workspace isolation, atomic activation, exact-account non-transferability, portable-class held-out/account-cluster requirements, class-field mismatch, and local `ClassActivationReceipt` binding;
- custody-authority onboarding, lease, and closure tests rejecting a self-signed or newly introduced authority/reviewer/audit-sink key; condition-author or repository-administrator ownership, administration, support, billing, telemetry, result-channel, or recovery access; shared organizational control between reviewers or sink operators; incomplete or self-asserted control evidence; submitter-visible interim telemetry; shared credentials; promotional credit, billing fallback, or any overage-capable quota path; service/API, principal, account, or control-plane drift; expired or revoked authority entries; activation before manifest finalization, plaintext release before activation, or sidecar replay after reveal; missing/late/reordered checkpoints or heartbeats; audit filtering or policy changes; grant-then-revoke prohibited access; enable-then-disable billing fallback; transient build/credential changes; and a final clean snapshot after an interval violation;
- identity-assurance, observation-expiry, append-only migration, and stale-record tests;
- RunJournal crash-point tests at every state transition, including fsync/CAS recovery, duplicate request, dispatch-unknown, durable supervisor reattach/cancel, retained reservation, capability revocation, orphan quarantine, effect reconciliation, and retry prohibition;
- capability-probe fixtures for full, sequential, prompt-only, malformed, drifting, and adversarial drivers;
- malicious-driver sandbox tests for home-directory and inherited-FD reads, arbitrary process/IPC access, Docker and SSH-agent sockets, keychain access, raw-network exfiltration, and unsigned driver loading;
- credential-containment tests rejecting `advisory` or `unavailable` isolation for managed execution; accepting `not-applicable-no-credential` only with `credentialBindingMode=none` and a negative broker probe that detects hidden headers, URI userinfo, client certificates, environment/config/keychain handles, and other credential channels; and using a disposable canary credential plus controlled endpoint to detect a faulty or compromised host CLI that places credential material in a serialized model payload, task-visible child environment or argument, stdout/stderr, artifact writer, or workspace; the suite also proves that payload authorization cannot substitute for credential containment and that any binary/signature/ACL/launch-policy drift revokes `host-tcb-attested` status;
- evidence-resolution tests for valid, stale, forged, out-of-root, wrong-span, wrong-hash, and non-supporting locators;
- verification tests for trust classes, claimant-authored scripts, static predicates, commands, traces, sources, falsification, timeouts, and permission denial;
- verifier-runner tests for immutable oracle mounts, candidate-process exclusion, copy-on-write workspaces, network/process limits, pre/post hashes, side-effect capture, and cleanup;
- adjudication tests for registry scope, signatures, claim/evidence binding, independence/conflicts, dual-review, timeout/refusal, expiry, and revocation;
- finding-state tests proving unsupported claims cannot enter the main report;
- verified-output tests rejecting novel entities or factual clauses, uncited causal/action rationale, scope or severity expansion, mismatched finding hashes, and free-form prose in the authoritative main section;
- router positive, negative, ambiguity, risk, budget, and simpler-mode tests;
- held-out in-scope, boundary, out-of-distribution, and indeterminate population-membership tests;
- family-provenance graph and similarity tests that collapse shared commits, source packets, generator lineages, and dominant failure mechanisms into cluster-level `N`;
- handoff tests for constraint, evidence, dissent, permission, and certainty preservation;
- prompt compilation and token-budget tests for full, compact, and minimal variants;
- driver contract tests for event versioning, sequence/order, tool correlation, terminal uniqueness, missing terminal, usage/effect reconciliation, cancellation, partial output, and fallback prohibition;
- execution-request tests for canonical hashes/signatures, content-addressed payload resolution, scoped controller services, brokered resolution/probe/execution transport and origin/redirect binding, COW workspace containment/journaling/delta/apply/rollback, streamed artifact finalize/quarantine/cleanup, controller-lease and host-managed credential binding, side-effect-free prepare, signed single-use dispatch tokens and duplicate-token reattachment, acceptance/rejection, policy binding, budget modes and per-dimension dispositions, idempotency, and safe retry linkage;
- egress tests for sensitivity inheritance, unknown destination policy, redirects, cross-origin credentials, retention mismatch, redaction-before-serialization, whole-scope authorization, opaque-CLI rejection for non-public data, and internal CLI transport bypass of an expected proxy;
- resource-accounting tests for aggregate reservation, parallel summation, cache policy, receipt reconciliation, overshoot, and missing telemetry;
- outcome-normalizer tests proving B0 atomization is condition-blind, immutable, and unable to manufacture runtime verification;
- claim-slot inventory tests for canonical uniqueness, atomicity, scorability, reachability, evidence binding, semantic-duplicate adjudication, derived `M_f`, and direct-integer rejection;
- statistical test vectors for favorable-slack transforms, hierarchical resampling, calibrated-alpha selection, type-I/coverage bounds, simultaneous IUT allocation, undefined metrics, replacement/retry taxonomy, and reconstruction from published seeds; and
- claim-linter tests preventing capability or pilot evidence from becoming a performance recommendation.

### 16.2 Behavioral tests

- seeded true, false, ambiguous, and unverifiable findings;
- no-finding and abstention tasks;
- correct-but-out-of-scope and needlessly verbose outputs that must fail the scope guardrail;
- correlated-agent agreement without evidence;
- irrelevant skill activation;
- sequential tasks where delegation should be rejected;
- parallel tasks where specialization can help;
- context truncation and lossy handoff;
- prompt injection in code and documents;
- false severity inflation;
- agent-caused timeout and malformed output; and
- profile drift between routing and execution.

### 16.3 Live qualification tests

Live tests follow the DEV, disjoint DA-CAL, and sealed DA-TEST rules in §10. They publish raw sanitized receipts, all exclusions, resource vectors, and analysis reconstruction. Deterministic graders are primary; blinded dual human adjudication handles irreducible semantic cases; LLM graders remain secondary.

## 17. Integration with the parent design

This addendum refines the parent design as follows:

- §3.3 “Minimum sufficient council” becomes the broader least-complex adequate strategy rule.
- §6.1 L1 and L2 strategy defaults are replaced by §7.2: risk determines safeguards and eligibility, while qualification determines inline versus delegated execution.
- §6 routing records the chosen inline or delegated counterfactual and its qualification evidence.
- §8 managed runtime gains task characterization, capability probing, evidence gating, provider drivers, and the qualification registry.
- §11 public claims distinguish compatibility, pilot evidence, confirmatory evidence, and scoped routing recommendation.
- §12 MahaBench retains its frozen external comparisons and counts; Delegation Assurance uses separately preregistered, disjoint delegation-value and adaptive-policy blocks.
- The existing four-host/five-surface BundleAdapter commitments remain; managed ExecutionDriver support is a separately probed lower-level boundary.
- The existing Abhimanyu Gate, approval receipts, Lineage Ledger, cultural policy, and installer guarantees remain unchanged.
- Parent §6.7 verifier failure remains run-level `failed`; `not-verifiable` is the affected finding state, not a way to relabel an invoked verifier failure as successful completion.

If language in the parent design implies that a subagent or skill is automatically preferable, this addendum controls.

## 18. Ownership

### 18.1 `callmearya`

Arya owns the Delegation Assurance architecture, schemas, runtime, provider drivers, capability probes, evidence/verification machinery, router, qualification harness, statistics, CI, integration, and releases.

### 18.2 `viji-saravanan`

Viji owns prompt fine-tuning, cultural nuance review, examples, public DEV rubric development, README, and public documentation. Executable prompt, skill, example, rubric, and trigger work ends before the applicable `DiagnosticFreezeManifest`; a later leaf-specific semantic asset also ends before its `LeafFreezeManifest`. Any later semantic change creates a new version and requires the applicable fully disjoint DA-CAL and DA-TEST blocks; post-freeze edits are limited to documentation that cannot alter executable semantics or user instructions. Viji may improve wording and usability before freeze but cannot weaken evidence, abstention, permission, egress, or claim boundaries.

Prompt/rubric authors do not receive task-level DA-CAL outcomes while the condition remains eligible for unchanged DA-TEST, and they never receive sealed DA-TEST content. Delegation Assurance custody follows §10.5 and remains outside both prompt tuning and CAL-driven revision; the parent MahaBench custody lane remains separate.

### 18.3 Independent benchmark custody

Before DA-CAL, the project registers one qualifying `CustodyAuthority` under §10.5: either an independent volunteer custodian on custodian-owned/administered hardware and account, or an external sealed-evaluation service using service-owned or exclusively service-administered local hardware or, when hosted, its own tenant, account, and credentials. In both paths, two eligible independent onboarding reviewers validate the disclosed ownership/admin/recovery/telemetry/billing and audit-sink principals plus mandatory external control/no-overage artifacts; condition authors and repository administrators have no administrative or remote-access path; and credentials are never shared. Registration persists only while its evidence remains current; it is not a block activation. For every diagnostic CAL, `I1` promotion TEST, leaf CAL, leaf TEST, policy CAL, operational TEST, forced-strategy TEST, or resource-matched custody block, the applicable freeze/evaluation/resource manifests are signed first; only then does the controller issue a fresh `CustodyActivationReceipt` and open a distinct lease whose entire interval is covered by attested immutable controls or a registered independent append-only audit sink. A handoff assay is an analytic sub-block inside its leaf CAL/TEST custody block and same lease, with a separate resource line; it is not separately activated. Each lease closes after all of its main and sidecar work plus the controlled reveal/retention step, with its own valid `CustodyClosureReceipt` and dual closure review before its evidence can feed a later decision. Ordinary author-administered CI, authority-only audit logs, and self-attested service keys cannot qualify. The authority owns Delegation Assurance plaintext and keys, oracle validation, condition-blind outcome normalization, failure/exclusion freeze, signed custody/execution/reveal receipts, and controlled reveal; the parent MahaBench custody lane remains independently governed. This is an evaluation-integrity role rather than artificial code authorship. Public attribution names a human custodian only with consent and actual contribution, or identifies the service and receipt provenance for the service path. If neither topology is available without incremental spending, confirmation does not run and the project reports only DEV or pilot evidence.

Authorship represents actual substantive work and follows the account-safety and authorization rules in the parent design.

## 19. Delivery stages

DA-1 through DA-5 are dependency-ordered assurance gates. DA-6 has a baseline documentation lane that runs alongside DA-1 through DA-3 and an evidence-update lane after each later block; numbering does not defer public conformance documentation.

### DA-1: Contracts and mechanical evidence gate

Implement material-profile, probe-observation, finding, evidence, verification, adjudication, verified-output, routing, population, signed qualification-envelope, `CustodyAuthorityRegistry`, `CustodyLease`, `CustodyActivationReceipt`, and `CustodyClosureReceipt` schemas; controller-owned registry activation; the controller-isolated Verifier Runner; deterministic main-output rendering; and deterministic validation. Gate: forged, stale, self-asserted, unsupported, non-supporting, VT1-only, invalid VT3, synthesis-only prose, a self-onboarded custody authority, a point-in-time-only custody check, or an unsigned/incomplete qualification DAG cannot produce a verified finding, authoritative factual clause, or automatic route.

### DA-2: Strategy selector and handoff integrity

Implement task characterization, eligibility, least-complex selection, context manifests, synthesis constraints, claim-to-finding mapping, and handoff assays. Gate: negative-routing, novel-synthesis-claim, and critical-loss suites pass.

### DA-3: Provider drivers and degradation

Implement BundleAdapters for all parent host surfaces; managed drivers only as release-signed out-of-process brokers for automatable host CLIs that pass sandbox, credential, payload-egress, and lifecycle probes; OpenAI-compatible Chat Completions and Ollama drivers; optional separately probed Responses behavior; the durable RunJournal/supervisor and prepare-dispatch recovery protocol; capability probes; bounded cancellation; loopback defaults; and full/compact/minimal prompt compilation. Gate: malicious-driver and opaque-egress tests fail closed; crash injection cannot redispatch unknown work, duplicate effects, release unreconciled budget, or apply an orphaned workspace; install-only surfaces are not mislabeled managed, and every declared tier exposes its lost coverage.

### DA-4: Diagnostic qualification

Register one eligible `CustodyAuthority` with two independent onboarding receipts. Apply the same order to every constituent custody block: sign the applicable freeze/evaluation/handoff and `QuotaCostManifest` artifacts; issue a fresh `CustodyActivationReceipt` bound to their hashes; open the distinct lease and release plaintext; execute; reveal under custody; then close and independently review the lease before its evidence feeds the next decision. Thus the diagnostic `DiagnosticFreezeManifest` and resource manifest commit before diagnostic activation; the co-committed `I1` leaf/TEST manifests commit before any later promotion-TEST activation; every `S1`/`C1` `LeafFreezeManifest`, including its assay and resource lines, commits before that leaf CAL/TEST activation; and `PolicyFreezeManifest` plus policy resource manifest commit before policy-CAL activation. Run the seven-condition, 168-session-per-population diagnostic CAL; nominate the inline incumbent mechanically; promote `I1` only with its exact confirmatory counterfactual; then, after final `I*` resolves, complete each remaining leaf's exact-scope program against that leaf's frozen named simpler incumbent. Every delegated `S1`/`C1` leaf carries its separately analyzed and resource-counted §8.3 handoff assay inside the same leaf CAL/TEST lease; its sealed upstream artifacts never cross a reveal boundary. Make only passing leaves reachable, commit the final policy freeze, and run the disjoint three-arm, 72-session-per-population policy CAL. Every custody block must have uninterrupted lease coverage, its own `CustodyClosureReceipt`, and dual closure review. Gate: custody receipts, leaf and handoff support, resource capacity, feasibility, and conservative joint-power requirements pass before sealed TEST.

### DA-5: Adaptive-policy evidence

For the frozen three-arm operational TEST—and separately for any forced-strategy or resource-matched TEST—first finalize and sign the applicable freeze/evaluation and `QuotaCostManifest` artifacts. Then refresh authority eligibility, issue a new `CustodyActivationReceipt` bound to those hashes, and open a distinct custody lease before plaintext release. Run the 216-session-per-population operational TEST against `B0` and `I*`; close the lease after controlled reveal with uninterrupted coverage, its own `CustodyClosureReceipt`, and dual review; publish all outcomes; and populate scoped registry records only after closure validates. Gate: recommendations follow every intersection-union requirement; inconclusive or negative outcomes remain public. Forced-strategy and resource-matched claims require their own independently frozen, activated, and closed custody blocks.

### DA-6: Documentation and release integration

Begin baseline README, provider setup, evidence semantics, capability/claim-ceiling matrices, and limitations in parallel with DA-1 through DA-3 so a conformance-only public release never waits for confirmatory evidence. After DA-4/DA-5, update recommendation matrices, raw-result links, calibration/custody receipts, and drift status for each completed block. Gate: pre-evidence documentation cannot imply performance, and every evidence update passes claim lint and reconstruction.

## 20. Success criteria

The Delegation Assurance addendum is implemented when:

1. routing logic contains no model-name quality shortcuts;
2. every execution records its `ExecutionCandidateFingerprint`, the selected leaf `MaterialProfileFingerprint` for adaptive runs, exact valid probe-observation IDs, and routing receipt;
3. unsupported or stale evidence cannot appear as a verified main finding or authoritative factual synthesis clause;
4. no-finding and abstention paths pass behavioral tests;
5. skills and delegated modes have positive and negative trigger tests;
6. the runtime selects the least complex qualified strategy and explains why;
7. full, sequential, prompt-only, and unsupported capability tiers fail or degrade honestly;
8. automatic routing reads only controller-activated signed qualification envelopes whose complete evidence DAG is scoped, valid, drift-sensitive, and reproducible;
9. exact-account recommendations remain non-transferable, and cross-account activation requires powered held-out multi-account class evidence plus fresh local mapping/probes;
10. the managed envelope is tested against bare native inline, and the adaptive policy is tested against both `B0` and `I*` operationally; a resource-matched claim is made only after its separately eligible block;
11. external-baseline, skill-value, delegation-value, and routing-policy claims remain distinct;
12. all quality and resource metrics are published separately without an opaque composite;
13. verifier provenance prevents agent-authored prose or tests from self-certifying findings;
14. sandboxed drivers and independently measured credential-containment and payload-enforcement dispositions block ambient access and unapproved data transmission; advisory credential isolation is never managed, and credential-secrecy claims explicitly exclude compromise of an attested host-CLI/credential TCB;
15. confirmatory confidence claims pass the frozen type-I/coverage calibration envelope as well as power and practical-value gates;
16. family-provenance and claim-slot derivations prevent pseudo-replication and denominator inflation;
17. reserve replacement and post-start retry rules cannot selectively shape the evaluated sample;
18. durable recovery prevents ambiguous dispatch replay, duplicate effects, orphaned workspace application, and premature budget release;
19. no human custodian or external service can self-onboard or pass only a point-in-time custody check: confirmatory evidence binds a current independently reviewed custody-authority entry, fresh activation and closure receipts, uninterrupted immutable-policy or independently anchored audit coverage, disclosed control/recovery/telemetry/billing principals, and authoritative no-overage proof; and
20. Viji’s prompt and documentation contributions remain substantive while Arya retains core implementation ownership.

Performance improvement is a possible scoped outcome, not an assumption. If a skill or delegated strategy provides no practical benefit, Mahabharata Council must prefer inline execution and publish that result.

## 21. Research and normative references

- [Google Research: Towards a science of scaling agent systems](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/)
- [Anthropic: How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)
- [Anthropic: Multi-agent systems research](https://www.anthropic.com/research/multiagent-systems)
- [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)
- [Realistic skill-selection study](https://arxiv.org/abs/2604.04323)
- [Agentic Benchmark Checklist](https://arxiv.org/abs/2507.02825)
- [AbstentionBench](https://arxiv.org/abs/2506.09038)
- [Abstention-aware evaluation incentives](https://www.nature.com/articles/s41586-026-10549-w)
- [Apple: Correlated LLM evaluation panels](https://machinelearning.apple.com/research/correlated-llm-evaluation-panels)
- [OpenAI: Trustworthy third-party evaluations](https://openai.com/index/trustworthy-third-party-evaluations-foundations/)
- [Inspect AI evaluation logs](https://inspect.aisi.org.uk/eval-logs.html)
- [Inspect Evals: AbstentionBench correction history](https://ukgovernmentbeis.github.io/inspect_evals/evals/safeguards/abstention_bench/index.html)
- [OpenAI deprecations](https://developers.openai.com/api/docs/deprecations)
- [Ollama OpenAI compatibility](https://docs.ollama.com/api/openai-compatibility)
- [Ollama tool calling](https://docs.ollama.com/capabilities/tool-calling)
- [Ollama structured outputs](https://docs.ollama.com/capabilities/structured-outputs)
- [Ollama model details](https://docs.ollama.com/api-reference/show-model-details)
- [llama.cpp server reference](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md)
- [vLLM OpenAI-compatible server](https://docs.vllm.ai/en/latest/serving/online_serving/openai_compatible_server/)
- [vLLM tool calling](https://docs.vllm.ai/en/latest/features/tool_calling/)
- [LM Studio OpenAI compatibility](https://lmstudio.ai/docs/developer/openai-compat)
- [LM Studio offline operation](https://lmstudio.ai/docs/app/offline)
- [Codex subagents](https://developers.openai.com/codex/subagents/)
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents)
- [Kiro custom agents](https://kiro.dev/docs/custom-agents/)
- [GitHub Copilot custom-agent configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Hugging Face model-card metadata](https://huggingface.co/docs/hub/model-cards)
- [OSI Open Source AI Definition](https://opensource.org/ai/open-source-ai-definition)

Implementation planning resumes only after this written addendum is reviewed and approved.
