# Mahabharata Council: System Design

**Status:** Approved design, awaiting written-spec review

**Date:** 2026-08-21

**Repository:** `callmearya/mahabharata-council`

**License:** Apache License 2.0

**Tagline:** Evidence-first agents for engineering and knowledge work.

## 1. Executive summary

Mahabharata Council is an independent, cross-platform system of bounded subagents for engineering and knowledge work. It uses character-inspired functional identities from the Mahabharata without claiming to simulate the characters, reproduce the epic's complete moral judgment, or confer religious authority. A typed, host-neutral source model compiles into native definitions for Claude Code, Codex, GitHub Copilot in Visual Studio Code, and Kiro. A small host-neutral runtime validates task, approval, handoff, budget, receipt, and lineage contracts in managed mode. The repository ships generated bundles, so a user can install a release without first building the compiler.

The system's differentiator is not the theme alone. It combines:

- minimal, risk-sensitive delegation rather than a ceremonial fixed pipeline;
- provenance-preserving evidence and a durable Lineage Ledger;
- explicit exit, rollback, resource, and human-approval controls;
- first-class native adapters for four hosts;
- safe project-local installation and generated-artifact drift checks; and
- a preregistered, zero-incremental-cost benchmark with raw receipts and formal paired statistics.

The initial public release must not claim to outperform Compound Engineering or any host default until the corresponding claim passes the benchmark rules in this document. Inconclusive and negative results are publishable outcomes.

## 2. Product goals and non-goals

### 2.1 Goals

1. Provide a coherent council of reusable agents for both software engineering and evidence-heavy knowledge work.
2. Preserve the same contract semantics across Claude Code, Codex, Copilot/VS Code, and Kiro while truthfully distinguishing runtime-enforced, host-enforced, and prompt-advisory behavior.
3. Make every consequential output auditable through evidence packets, execution receipts, dissent, uncertainty, and lineage.
4. Default to the smallest useful delegation graph and impose explicit budgets on parallelism, review cycles, context, and tool use.
5. Install and update without modifying global credentials, global editor settings, or unrelated user files.
6. Evaluate real end-to-end outcomes with deterministic graders, paired conditions, public raw artifacts, and honest statistical claims.
7. Treat cultural grounding as a maintained engineering concern with provenance labels and stereotype safeguards.
8. Remain usable at zero incremental spend by relying only on already-included subscription quotas and free local tooling.

### 2.2 Non-goals

- Reproducing Compound Engineering, Superpowers, or another prompt collection under Mahabharata names.
- Claiming a single authentic or exhaustive interpretation of the Mahabharata.
- Simulating divinity, omniscience, prophecy, surveillance, or infallible moral authority.
- Replacing host authorization, security boundaries, or human accountability.
- Training or fine-tuning a foundation model in the first release.
- Publishing a package to npm in version 1; direct GitHub releases and checked-in bundles are sufficient.
- Optimizing for benchmark scores by exposing sealed tests to prompt authors.
- Claiming that direct native invocation enforces controls that only managed runtime mode can enforce.
- Using paid API calls, pay-as-you-go fallback, or automatic billing changes.

## 3. Design principles

### 3.1 Bounded modern inference

Each persona is a modern functional interpretation inspired by selected episodes and scholarly or textual sources. Every generated agent prompt must state that it is bounded, fallible, and not a simulation of the named figure. Character identity is a memory aid for responsibilities, never a reason to override evidence, permissions, or user intent.

### 3.2 Evidence before confidence

Outputs distinguish four categories: source, direct observation, inference, and uncertainty. Citations and machine-verifiable receipts take precedence over rhetorical certainty. Unsupported superiority language is a CI failure.

### 3.3 Minimum sufficient council

Simple work remains with the host agent. Complex work activates only the specialists needed for the task and risk tier. The default ceiling is three parallel specialists and two review cycles. Expanding either ceiling requires an explicit reason recorded in the run receipt.

### 3.4 Dissent is data

Risk or integrity concerns are preserved through synthesis. A coordinator may resolve a disagreement for action, but must record the dissent, its evidence, and why the chosen course prevailed.

### 3.5 Human authority and reversibility

The user retains final authority. Agents cannot silently expand permissions, reinterpret an unsafe request as authorized, or take irreversible action merely because a persona suggests decisiveness. Risky work requires exit and rollback conditions.

### 3.6 Progressive disclosure

Prompts load metadata and the smallest relevant instruction set first. Full evidence packets, specialized skills, and historical learnings load only when routed. This constrains context use and reduces irrelevant prompt influence.

### 3.7 Native portability, not lowest-common-denominator prompts

A canonical contract defines invariant intent. Platform adapters express that intent with native files, tools, permission controls, and handoff mechanisms. Every control is labeled `runtime-enforced`, `host-enforced`, or `prompt-advisory` for each pinned target. Managed mode fails closed when a required runtime or host capability is absent. Direct native mode remains useful, but documentation and receipts expose any advisory-only control rather than claiming equivalent enforcement.

## 4. Council roster

Canonical identifiers are ASCII and stable. User-facing names may include IAST diacritics. Generic aliases are available for users who prefer function-first naming, but aliases do not replace canonical IDs.

| Canonical ID | Display name | Generic alias | Primary responsibility | Required boundary |
|---|---|---|---|---|
| `krishna-orchestrator` | Kṛṣṇa Orchestrator | Coordinator | Frame tasks, route the minimum council, reconcile outputs, and issue the final synthesis | No omniscience, divinity claim, concealed manipulation, or irreversible action without approval |
| `draupadi-requirements` | Draupadī Requirements Advocate | Requirements Advocate | Test authorization, standing, prerequisites, affected parties, constraints, and acceptance criteria | No victimhood or anger caricature; do not reduce the figure to one episode |
| `vyasa-evidence` | Vyāsa Evidence Steward | Evidence Steward | Build evidence packets, trace provenance, synthesize sources, and maintain lineage | Separate source, observation, inference, and uncertainty; no claim to final or absolute truth |
| `arjuna-executor` | Arjuna Executor | Scoped Executor | Perform precise, authorized actions and verification within a clear target | Pause on material ambiguity; do not valorize action over safety or evidence |
| `vidura-risk` | Vidura Risk Counselor | Risk Counselor | Identify ethical, privacy, permission, conflict, rollback, and governance risks | Advisory dissent stays visible; risk analysis cannot manufacture authority |
| `yudhishthira-deliberator` | Yudhiṣṭhira Deliberator | Decision Deliberator | Compare tradeoffs, uncertainty, second-order effects, and decision criteria | Explicitly fallible; no automatic equation of status with correctness |
| `bhima-challenger` | Bhīma Challenger | Adversarial Challenger | Stress assumptions, reproduce failures, test load and edge cases, and challenge weak evidence | Non-destructive by default; destructive tests require isolated fixtures and authorization |
| `sanjaya-observer` | Sañjaya Observer | Run Observer | Record chronological telemetry, tool receipts, state changes, and observed outcomes | Clearly label observed versus inferred; no surveillance or omniscience framing |
| `bhishma-legacy` | Bhīṣma Legacy Analyst | Compatibility Analyst | Surface standards, compatibility, institutional memory, migration cost, and lock-in | Tradition is evidence, not a veto; expose the cost of preserving legacy choices |
| `gandhari-consequences` | Gāndhārī Consequence Advocate | Impact Advocate | Analyze human impact, irreversible loss, remedies, and post-incident consequences | No disability stereotype or claim that impairment implies insight or ignorance |
| `yuyutsu-escalation` | Yuyutsu Integrity Escalation | Integrity Escalation | Provide an independent channel for principled objection and escalation | Dissent is not disloyalty; escalation must cite a concrete integrity concern |

The core deliberately excludes personas derived from Karṇa, Śakuni, and Duryodhana. Common prompt mappings for these figures too easily collapse complex traditions into grievance, deception, or villain stereotypes. Future additions require the same cultural, safety, and utility review as the core roster.

### 4.1 Abhimanyu Gate

`abhimanyu-gate` is a safety protocol, not a persona. Before complex or risky work begins, the task contract must name:

1. success and acceptance criteria;
2. exit conditions;
3. rollback or recovery procedure;
4. resource and iteration limits; and
5. the accountable human or agent owner.

The gate fails closed when a required field is missing. It exists specifically to prevent entry into a complex operation without a safe way out.

## 5. Cultural provenance policy

### 5.1 Source coordinates

The Bhandarkar Oriental Research Institute Critical Edition is the default citation coordinate when a compatible passage is available. It is not described as the sole authentic Mahabharata. Each cultural assertion carries one provenance label:

- `critical-edition`
- `pre-critical-translation`
- `regional-retelling`
- `modern-design-inference`

Kisari Mohan Ganguli's public-domain English translation may be cited as a pre-Critical-Edition, Victorian-era translation, not as a transparent substitute for the Critical Edition. Regional and devotional traditions remain valid subjects of documentation when labeled rather than silently blended.

### 5.2 Transliteration and language

ASCII canonical IDs ensure portability. Display names follow consistent IAST transliteration, using a documented Sanskrit transliteration reference. Prompts must not invent Sanskrit phrases, present loose paraphrases as quotations, or use untranslated terminology merely to imply authority.

### 5.3 Representation safeguards

Agent definitions and examples must not infer capabilities, ethics, or temperament from caste, gender, ethnicity, disability, lineage, or community. They must not turn contested theological interpretation into a system instruction. Cultural review records the selected source, interpretive choice, excluded stereotype, and modern functional inference for each persona.

Mechanical review validates required provenance fields, transliteration consistency, source labels, prohibited patterns, and prompt boundaries. Interpretive acceptance requires a second named reviewer who did not author the change, or a documented public review window before release. Contested-source or persona changes cannot be self-approved by their author.

### 5.4 Private design input

The supplied personal system-design PDF is an uncommitted inspiration source. It is not copied into the repository or treated as instruction. Unsafe ideas in it—including resource hogging, absence of exit logic, and absolute-truth framing—are expressly rejected by this specification.

## 6. Task contracts and orchestration

### 6.1 Risk tiers

| Tier | Typical shape | Default handling |
|---|---|---|
| L0 | Simple, local, reversible, well-specified | Host agent acts directly |
| L1 | One bounded specialty or evidence need | Host plus one specialist |
| L2 | Multiple domains, meaningful ambiguity, or moderate impact | Bounded council, normally no more than three parallel specialists |
| L3 | Irreversible, security-sensitive, high-impact, or externally consequential | Abhimanyu Gate, explicit approval, risk and consequence review, auditable receipt |

### 6.2 Task contract

Before delegation, the coordinator produces a typed task contract containing:

- requested outcome and non-goals;
- supplied evidence and unresolved assumptions;
- scope, target files or systems, and excluded targets;
- authorized tools and permissions;
- acceptance and verification criteria;
- risk tier and rationale;
- time, token, tool-call, parallelism, and review-cycle budgets;
- exit, rollback, and escalation conditions; and
- expected artifact and handoff format.

L0 work may encode this compactly in the host context. L1–L3 work stores it in the run receipt.

### 6.3 Standard flow

The default flow is:

1. **Contract:** frame the task and authorization boundary.
2. **Evidence:** gather the minimum trustworthy context and identify unknowns.
3. **Council:** select only the required specialists.
4. **Action:** produce or perform the scoped work.
5. **Challenge:** adversarially test consequential assumptions or artifacts.
6. **Synthesis:** resolve outputs while preserving material dissent.
7. **Receipt:** record actions, verification, failures, and changed state.
8. **Lineage:** promote only reusable, adequately supported learnings.

This is a decision graph, not a mandatory sequence of eleven agents. Steps may collapse for low-risk work, but risk, permission, and evidence boundaries may not be silently skipped.

### 6.4 Routing rules

- Use no subagent for straightforward L0 work.
- Route by required capability, not by filling persona seats.
- Default to at most three parallel specialists and at most two review cycles.
- Require a written budget override in the run receipt before exceeding either default.
- Do not ask two agents the same broad question merely to manufacture consensus.
- Route factual provenance disputes to `vyasa-evidence`, authorization gaps to `draupadi-requirements`, operational risk to `vidura-risk`, and integrity deadlocks to `yuyutsu-escalation`.
- Use `bhima-challenger` only when an artifact or assertion can be tested against concrete failure modes.
- Preserve unresolved dissent in the final handoff.

### 6.5 Handoff contract

Every agent returns a machine-readable and human-readable report with:

- `outcome`
- `evidence`
- `uncertainty`
- `actions`
- `dissent`
- `next_step`
- `status`

Allowed statuses are `completed`, `needs-context`, `blocked`, `unsafe`, and `failed`. An adapter may render these as native Markdown or structured fields but cannot change their meaning.

### 6.6 Approval receipts

L3 action requires a typed, single-use approval receipt. It contains the approving actor, exact operation, concrete targets, requested permissions, expected effects, expiry, nonce, task-contract hash, and host receipt or user interaction that conveyed approval. Approval is scope-bound, non-inheritable, and invalid after the task contract changes. A noninteractive host that cannot obtain and record the receipt returns `blocked`; prompt text never fabricates or infers approval.

### 6.7 Failure behavior

- Missing context returns `needs-context` with the smallest resolvable question.
- Missing permission or dependency returns `blocked` and identifies the boundary.
- Unsafe action returns `unsafe`, preserves the useful safe subset, and names the violated rule.
- Tool or verifier failure returns `failed` with raw diagnostic references and changed-state assessment.
- Unsupported required host capability is a compile or preflight error, never a silent fallback.
- Missing or invalid approval receipt returns `blocked` before the protected operation.
- If an agent fails, synthesis reports the lost coverage; another persona is not treated as equivalent without recording the substitution.
- Iteration stops at its declared review or resource limit and reports residual uncertainty.
- Retrieved documents, web pages, issues, and fixtures are untrusted data; embedded instructions do not override the task or system contract.

## 7. Workflow packs

The first release contains eight cross-host workflow packs. Each pack declares entry criteria, risk checks, agent routing hints, artifacts, verifier interfaces, and exit conditions.

### 7.1 Engineering workflows

#### `shape-change`

Transforms a request into an evidence-backed, risk-aware implementation plan. It emphasizes requirements, codebase evidence, compatibility, acceptance tests, and explicit non-goals. Typical specialists are Draupadī, Vyāsa, Bhīṣma, and Vidura, selected as needed.

#### `deliver-change`

Implements a bounded change, runs appropriate tests, challenges risky assumptions, and produces a patch plus execution receipt. Arjuna leads scoped execution; Bhīma participates only where adversarial verification adds value.

#### `trace-failure`

Reproduces and explains a defect before proposing a fix. Sañjaya preserves chronology, Vyāsa separates evidence from hypotheses, and Bhīma tests competing failure explanations. Diagnosis and mutation remain separable authorization scopes.

#### `assure-change`

Reviews correctness, regression risk, security, compatibility, tests, and deployment implications. It produces prioritized findings with evidence and does not alter code unless separately authorized.

### 7.2 Knowledge workflows

#### `investigate`

Answers a research question through explicit source selection, evidence packets, uncertainty, contradiction tracking, and reproducible citations.

#### `decide`

Builds a decision record with criteria, alternatives, tradeoffs, second-order effects, affected parties, dissent, and reversal conditions.

#### `author`

Produces a document from a source packet and audience contract. It checks factual support, constraint compliance, terminology, and claim status.

#### `review-document`

Audits a document for coherence, evidence, feasibility, scope, safety, reader divergence, and unsupported claims. It reports findings rather than rewriting unless revision is authorized.

## 8. Canonical contract and compiler architecture

### 8.1 Source of truth

Canonical definitions are data plus prompt fragments, validated by JSON Schema and TypeScript types. The source tree is organized by responsibility:

```text
agents/                 canonical persona contracts and shared fragments
workflows/              routing graphs and workflow contracts
skills/                 portable Agent Skills
schemas/                versioned agent, workflow, receipt, and lineage schemas
runtime/                managed-mode validation, approvals, budgets, receipts, and lineage writes
adapters/               host-specific compilers and capability matrices
generated/              deterministic release bundles and manifests
lineage/                schemas, examples, and curation rules
eval/                   preregistration, tasks, fixtures, graders, runner, analysis, results
docs/                   architecture, culture, safety, platforms, benchmarks, prior art
tests/                  compiler, adapter, installer, behavior, golden, and security tests
.github/                 CI, issue forms, pull-request template, and native Copilot output
```

Canonical agent fields include:

- stable ID, display name, alias, version, and provenance entries;
- purpose, activation conditions, non-goals, and forbidden behavior;
- input and output schemas;
- required and optional capabilities;
- tool, permission, context, token, parallelism, and iteration budgets;
- task-contract, handoff, dissent, and failure behavior;
- prompt fragments and progressive-disclosure resources;
- supported workflow roles;
- adapter overrides with an explicit reason; and
- behavioral tests and golden cases.

Contracts use strict JSON files and prompt fragments use Markdown. JSON is UTF-8, Unicode NFC, LF-terminated, duplicate-key-free, and canonicalized with RFC 8785 JSON Canonicalization Scheme before hashing. Prompt fragments are UTF-8, Unicode NFC, LF-terminated, and composed by ordered named section references rather than an executable template language. Schemas have stable `$id` values and semantic versions; forward migrations are explicit, tested transformations and unsupported downgrade attempts fail.

### 8.2 Capability negotiation

Each adapter publishes a versioned capability matrix. Controls are first classified by enforcement source:

- `runtime-enforced`: checked by the Council runtime before or after a host operation;
- `host-enforced`: guaranteed by a documented, pinned host feature and verified by conformance tests; or
- `prompt-advisory`: expressed in instructions but not technically enforceable by the runtime or host.

Capabilities are then classified as:

- `required`: compilation fails when unsupported;
- `emulated`: adapter implements equivalent behavior and its conformance test;
- `optional`: bundle remains valid but reports reduced functionality; or
- `host-only`: enhancement that cannot alter canonical safety semantics.

Compiler diagnostics name the agent, capability, target, enforcement source, and remediation. A release cannot mark managed mode supported unless runtime doctor checks and the required-capability conformance suite pass. Native-only support is labeled separately and cannot inherit managed-mode safety claims.

### 8.3 Native output targets

Target versions and exact schemas are pinned in `adapters/targets.lock`. The version-1 compiler produces five surface profiles across four hosts:

| Target ID | Native agents | Skills/resources | Repository guidance |
|---|---|---|---|
| `claude-code@2026-08-21` | `.claude/agents/*.md` | `.claude/skills/**` | `CLAUDE.md` imports shared root guidance |
| `codex@2026-08-21` | `.codex/agents/*.toml` | `.agents/skills/**` | Root `AGENTS.md` is canonical repository guidance |
| `copilot-vscode@2026-08-21` | `.github/agents/*.agent.md` | `.github/skills/**` | Generated agent metadata plus root guidance |
| `kiro-cli@2026-08-21` | `.kiro/agents/*.json` | `.kiro/skills/**`, with resources enumerated explicitly | Generated steering/resource references |
| `kiro-ide@2026-08-21` | `.kiro/agents/*.json` | `.kiro/skills/**`, with resources enumerated explicitly | Generated steering/resource references plus IDE discovery and permission metadata |

These IDs name compatibility profiles frozen on the design date. `targets.lock` records the concrete executable or extension version, schema/documentation revision, discovery command, and supported version range validated during adapter implementation; releases never resolve versions dynamically. `CLAUDE.md` imports root `AGENTS.md` rather than duplicating mutable project rules. Kiro CLI and Kiro IDE are separately tested first-class targets: the IDE profile adds extension discovery, permission, subagent invocation, and doctor conformance instead of inheriting CLI results. Additional surfaces such as Copilot CLI or Kiro Markdown agents require distinct target IDs and conformance results rather than silent reuse of these bundles.

### 8.4 Deterministic generation

Compilation must be hermetic for a given source commit, compiler version, and target version. Release trees live under `generated/<target-id>/` and preserve the exact native relative paths. A per-file manifest—not directory ownership—contains every source and output hash. A self-install step may copy only manifest-listed outputs into repository-native paths such as `.github/agents/`, alongside hand-written CI files. CI regenerates all bundles and fails on drift. Generated files carry a header identifying the source and prohibiting manual edits.

### 8.5 Runtime division

The compiler, installer, managed runtime, and native evaluation launcher use Node.js with TypeScript. Managed mode validates task contracts, approval receipts, budget counters, handoff/status payloads, run receipts, and Lineage Ledger writes around native host calls. Native mode invokes the generated host agent directly and is explicitly limited to controls marked host-enforced or prompt-advisory. Statistical analysis uses Python with a lockfile and pinned scientific dependencies. Release bundles include generated host files, checksums, schemas, runtime artifacts, and installer artifacts so end users do not need Python or a TypeScript toolchain to install them.

## 9. Safe installation and lifecycle

The installer is project-local by default and accepts an explicit target directory. Its commands are `plan`, `install`, `update`, `uninstall`, and `doctor`.

### 9.1 Install transaction

1. Resolve and validate the target as a concrete project directory; reject broad roots, case-fold aliases, traversal, and unsafe links.
2. Detect hosts and existing Council-owned files.
3. Produce a dry-run plan showing creates, replacements, conflicts, and unsupported targets.
4. Verify mandatory release provenance and bundle checksums against the expected repository, workflow identity, source ref, and source commit.
5. Create ignored, mode-`0700`, same-filesystem transaction storage and a write-ahead journal.
6. Immediately before each mutation, repeat root-containment and component-by-component no-follow checks; reject symlink swaps, unexpected hardlinks, and concurrent installer ownership.
7. Back up every replaced owned file with its mode, then write through same-filesystem temporary files and atomically rename when supported.
8. Persist a per-file ownership manifest with original and installed hashes, permissions, verified source commit, and build identity.
9. Verify installed hashes and run target-specific runtime and host doctor checks.
10. On interruption or failure, `doctor` resumes the idempotent write-ahead recovery before any new transaction. An unrecoverable mismatch fails closed and preserves the journal and backups for explicit repair.

### 9.2 Ownership and conflict rules

- Never overwrite an unowned file without explicit opt-in.
- Never remove a file whose current hash differs from the installed ownership manifest.
- Update only files owned by the active manifest; surface local modifications as conflicts.
- Uninstall removes unchanged owned files and reports modified owned files for manual resolution.
- Never modify GitHub authentication, Git credential helpers, global Git config, subscription settings, or global editor configuration.
- Never install into a home directory or broad filesystem root through an unresolved variable or glob.
- Never place transaction storage where it can be committed; installation preflight verifies the storage path is ignored.

## 10. Lineage Ledger

The Lineage Ledger turns work into reusable, reviewable evidence and promotes only supported conclusions, without storing private reasoning.

### 10.1 Record classes

1. **Run receipts:** tamper-evident, sanitized records of task contracts, actions, artifacts, verification, dissent, and every terminal status.
2. **Evidence packets:** source metadata, excerpts within licensing limits, observations, hashes, and confidence.
3. **Decisions:** alternatives, criteria, choice, dissent, consequences, and reversal conditions.
4. **Curated learnings:** reusable conclusions promoted from one or more receipts through review.

### 10.2 Learning schema

A reusable learning contains scope, evidence references, confidence, source commit, invalidating conditions, owner, review date, and expiry date. Conflicting learnings coexist as linked records; the system never silently overwrites one with another. Only adequately supported conclusions are promoted, but failed, blocked, unsafe, and incomplete receipts remain part of the evidence history.

### 10.3 Retrieval

Retrieval is metadata-first. The agent selects records by task scope, repository, version, confidence, and freshness before loading full bodies. Expired or contradicted records remain discoverable but are visibly ineligible for silent reuse.

### 10.4 Privacy and security

Receipt IDs are content hashes, each receipt references its predecessor where a sequence exists, and supersession creates a new record instead of mutation. Release checkpoints attest the published receipt set so later alteration is detectable; repository history alone is not described as immutable.

The ledger excludes secrets, credentials, raw environment dumps, private account emails, absolute home paths, hidden chain-of-thought, and unredacted private test material. Sanitization occurs before durable writes whenever the host stream permits. Unavoidable raw capture is quarantined outside project and synchronized directories in mode-`0600` ephemeral storage, excluded from application backup manifests, and never content-hashed into the public ledger. An active-run heartbeat protects live capture; `finally` cleanup deletes it when possible, and runner startup plus `doctor` scavenge abandoned entries after a one-hour TTL. If the process or machine remains unavailable, deletion occurs on the next startup or doctor run; the TTL is not misrepresented as a wall-clock guarantee while no cleanup process exists. Publication or scanner failure attempts immediate destruction before emitting only a sanitized diagnostic receipt. Unclassified transcript fields or scanner hits fail closed. The ledger stores concise evidence and decision rationales, not latent reasoning transcripts. Learning promotion cannot rewrite identity, permission, cultural-safety, or system instructions.

## 11. Evidence and claims model

### 11.1 Claim registry

Every public comparative or performance statement is registered with a stable evidence ID and one status:

- `unverified`
- `mechanically-verified`
- `pilot`
- `confirmatory`

The record identifies the exact claim, population, host, baseline, task set, model/client versions, metric, analysis, evidence artifacts, limitations, and expiration trigger. README badges and prose reference registry IDs.

### 11.2 Claim linter

Public claim sections use structured markers that reference registry IDs; the site and README tables are generated from those records. CI rejects missing, expired, or status-incompatible markers. A prose scan for language such as “better,” “faster,” “more reliable,” and “state of the art” is defense-in-depth, with tested false-positive allowlists rather than a claim of exhaustive natural-language detection. Release prose also requires human review. Mechanically verified claims are limited to facts such as adapter conformance or deterministic test outcomes.

## 12. MahaBench-PoC-1

### 12.1 Research questions and estimands

The benchmark has two domain-specific Codex comparisons and never pools them into one superiority number:

1. **Engineering:** Does Mahabharata Council improve deterministic task reward over a pinned public Compound Engineering release on the declared Codex model, client, and engineering task population?
2. **Knowledge:** Does Mahabharata Council improve deterministic task reward over a clean Codex native/default condition on the declared model, client, and offline knowledge-task population?

Compound Engineering may be run on knowledge tasks as an exploratory condition, but those results cannot support a headline comparison because engineering is its documented center of gravity. Kiro and Copilot experiments test transportability against exact pinned native/default conditions. Claude receives conformance coverage and a live experiment only when an existing subscription can cover a complete declared block at no incremental charge.

Each estimand is the equal-weighted mean difference over its tested task families, averaging three fresh stochastic repetitions per arm and task. Claims remain bounded to the tested host, client, model, conditions, task population, and reward definition.

### 12.2 Development, custody, and sealed test sets

- **DEV:** eight public tasks, four engineering and four knowledge, used for prompt development and harness validation. DEV results never support performance claims.
- **PILOT/CAL:** 12 independent non-TEST families per domain, run with two repetitions per arm after a condition freeze, used for feasibility, conservative variance calibration, and pilot evidence only.
- **TEST engineering:** 24 independent task families.
- **TEST knowledge:** 24 independent task families backed by hash-pinned, redistributable offline source packets.
- **Reserve:** a predeclared encrypted reserve is used only when oracle validation rejects a task before either experimental arm runs.
- Each TEST family has hidden deterministic checks prevalidated by an oracle run in the test-custody lane.
- The encrypted TEST and reserve archives plus plaintext archive and fixture hashes are committed before experimental runs. Holdout content is revealed only after results and exclusions are frozen.

Prompt tuning and TEST custody are incompatible roles. The prompt lane may author agent prompts, experiments, DEV tasks, and general rubric structure. A named custodian who did not author or tune condition prompts controls TEST plaintext, task-specific graders, and oracle validation. Prompt, compiler, adapter, baseline configuration, and grader-interface hashes freeze before the prompt lane receives any TEST content or outcome. Oracle diagnostics stay in the custody lane and cannot feed prompt changes.

Before oracle execution, the encrypted manifest fixes machine-checkable rejection codes and thresholds—fixture corruption, grader nondeterminism beyond the declared tolerance, license failure, oracle inability to reach the declared feasible score, or source-packet hash mismatch—and a one-to-one reserve mapping within each stratum. A rejection automatically selects the mapped reserve item; the custodian cannot choose among replacements. Every rejected item, code, measurement, and substitution is published after reveal. If the mapped reserve also fails, the block does not begin. After any experimental arm begins, there are no replacement tasks and no prompt changes; an invalid family makes the confirmatory block incomplete.

Before holdout freeze, the condition is frozen and the separate PILOT/CAL set is executed. Its report measures wall time, quota consumption where observable, failure rate, p95 resource use, and task-level variance. It includes a 20% resource reserve and demonstrates that one domain's 144-session confirmatory block can complete within 14 consecutive days without expected client/model drift or incremental charges. A preregistered simulation uses the one-sided 95% upper confidence bound for task-level variance, plus a published intra-task-correlation sensitivity range. Confirmatory execution requires at least 80% power at two-sided α = 0.025 to detect a minimum practically important reward difference of 0.10 under that conservative bound. If feasibility or power fails, TEST remains unopened and the project publishes only `pilot` evidence. PILOT/CAL results cannot be upgraded post hoc or enter confirmatory analysis. Any prompt or condition change after calibration requires a new version and a disjoint PILOT/CAL set before another power decision.

A task family is the independence unit. Families cannot share a base repository commit, source packet, generator lineage, or dominant expected failure mechanism. Provenance-graph and similarity checks enforce this rule. Any unavoidable sharing defines a cluster; randomization, resampling, and effective N move to the cluster level, and confirmatory analysis still requires 24 independent clusters. The power report models the declared cluster structure.

Tasks are independently licensed for redistribution and designed as families rather than superficial prompt variants. Engineering tasks use fixed repositories or fixtures, polyglot code, terminal workflows, regressions, and constraints. Knowledge tasks require exact answers, CSV or structured deliverables, evidence packets, and constraint-aware plans with machine-verifiable properties. Confirmatory knowledge runs use identical offline source packets in both arms; live-web retrieval is a separately labeled exploratory transportability test. The task manifest publishes source, license, seed, mutation method, and verifier after reveal.

### 12.3 Conditions and replication

#### Engineering comparison

- Arm A: Mahabharata Council in managed mode.
- Arm B: an exact Compound Engineering release commit using its documented top-level self-routing entry point.
- 24 task families × 3 fresh repetitions × 2 arms = 144 Codex sessions.

#### Knowledge comparison

- Arm A: Mahabharata Council in managed mode.
- Arm B: Codex native/default behavior in a clean profile with no Council, Compound Engineering, user agent, or user skill customization.
- 24 task families × 3 fresh repetitions × 2 arms = 144 Codex sessions.

Every condition manifest freezes the complete installed-file tree and hashes, entry prompt, top-level request wrapper, model and fallback policy, client, reasoning effort, tools, permissions, context limit, time and resource budgets, environment allowlist, and fixture state. Model fallback is disabled. Both arms receive the identical user task and the same degree of routing assistance: each condition self-routes from its documented entry point. No arm receives manual task-to-workflow selection.

Within each repetition, the two arms run adjacently from independently reset copies of the same fixture. The published seed fixes task order and one assignment bit per task family. That bit selects one of two complementary three-repetition order sequences: `A-B, B-A, A-B` or `B-A, A-B, B-A`. This balances order while making the task family the exact randomization unit used by inference. Each repetition is a fresh host session with no cross-run memory except the condition artifacts declared in its manifest.

#### Secondary hosts

Kiro CLI and Copilot for VS Code each use a frozen, stratified 12-family subset—six engineering and six knowledge—with two fresh repetitions per arm, for 48 sessions per surface. Each Council condition is compared with an exact clean default condition identified by client version, surface, config hashes, model/fallback policy, and installed files. These results are transportability evidence and are not pooled with Codex. Kiro CLI performance does not imply Kiro IDE performance. Claude live evaluation follows the same manifest rules if it is run.

### 12.4 Native runner, isolation, and zero-incremental-cost policy

`maha-eval` invokes real subscription-backed host CLIs rather than an LLM simulator. Each run receives a disposable task workspace with Git remotes removed and an isolated supported host profile. The process environment is constructed from a credential allowlist. It cannot push, create issues, mutate GitHub account state, change billing, or change everyday host configuration. Concurrency defaults to one to reduce throttling and temporal imbalance.

Preflight records client and model identifiers, fixture and condition hashes, disk capacity, executable versions, authentication readiness, and the strongest available evidence about included quota and billing mode. It does not scrape credentials. Kiro headless execution may require a user-created `KIRO_API_KEY`; the harness reports that prerequisite rather than extracting it. Copilot installation or authentication is similarly explicit. Paid API credentials are excluded from the runner environment.

This is a zero-incremental-cost policy, not a promise that every client exposes authoritative quota data. A run block starts only when an authoritative client/account signal or explicit user attestation establishes that the complete block plus reserve is covered by an existing subscription without pay-as-you-go charges. The runner never enables billing, accepts an overage, adds a payment method, or falls back to a metered API key.

The run stops only for:

- an overage or pay-as-you-go signal;
- insufficient included quota to finish the current fixed block plus reserve;
- a model, client, condition, or fixture version change;
- more than two consecutive condition-independent infrastructure failures; or
- user cancellation.

It never stops early because one arm appears superior. A stopped or partial block remains published operational evidence but cannot support a confirmatory performance claim. Evidence releases are staged: engineering confirmatory first, knowledge confirmatory second, then Kiro CLI and Copilot transportability. Each stage has its own preflight and fixed environment. Live benchmark completion does not gate a conformance-only public release; that release is labeled performance-unverified until a complete evidence block is published.

### 12.5 Failure taxonomy and intention-to-treat rules

The preregistration assigns every terminal event before holdout access:

- Agent-caused timeout, crash, malformed output, tool misuse, refusal contrary to the task contract, or resource exhaustion is a valid task outcome and receives its deterministic score, including zero when no checks pass.
- A condition-independent harness failure is an event outside both agents after fixture validation and before a scorable result, such as host service unavailability or runner corruption. It invalidates the complete paired repetition, never one arm alone.
- Failure classification uses machine error codes and blinded adjudication without condition labels or task scores. Every adjudication and excluded pair is public.
- A predeclared retry schedule may repeat the complete pair after a qualifying harness failure. It never replaces an agent failure.
- No task is removed or replaced after an experimental arm begins. Fewer than three valid paired repetitions for any of the fixed 24 families makes that domain block descriptive only.

### 12.6 Endpoints and statistics

Deterministic graders emit exact rational reward on `[0,1]`; equal-weight subchecks define the denominator. For each task family, the analysis averages the three paired arm differences. The domain endpoint is the equal-weighted mean of the 24 task-family differences.

Each domain reports:

- paired mean reward difference;
- a 95% hierarchical percentile-bootstrap interval from 100,000 draws, resampling task families and then repetition pairs within selected families, using the committed seed;
- a two-sided exact randomization test over all `2^24` assignments of the 24 task-level bits and their complementary order sequences, under the sharp no-condition-effect null;
- Hedges-corrected paired standardized effect `g_z = (1 - 3/91) × mean(d) / sd(d)` over the 24 task-level differences; and
- exact-rational win/tie/loss counts, where a tie is a zero task-level difference.

Engineering and knowledge are two confirmatory hypotheses, so their sign-flip p-values use Holm family-wise correction at α = 0.05. A supportive strict-pass endpoint marks an arm successful for a family when at least two of three repetitions pass every check, then uses exact paired McNemar analysis across the 24 family-level binary pairs.

Time, available token estimates, tool calls, retries, and context use are reported separately as efficiency measures; no arbitrary composite mixes them with correctness. Secondary-host hypothesis tests use Holm correction within their declared family or remain explicitly descriptive.

A positive domain performance claim requires all of:

1. the 95% bootstrap interval lower bound is above zero;
2. the Holm-adjusted confirmatory p-value is below 0.05;
3. all 24 fixed independent task families have three valid intention-to-treat repetition pairs;
4. oracle, condition-manifest, isolation, and infrastructure checks are valid;
5. no unresolved material harness or grader defect exists; and
6. wording is restricted to that domain's tested host, model, baseline, task population, and endpoint.

Otherwise the result is labeled inconclusive or negative and still published. No pooled engineering-plus-knowledge or “better everywhere” claim is allowed.

### 12.7 Grading

Primary graders are deterministic tests, schema checks, exact match, file-state assertions, and constraint checks. Confirmatory knowledge graders operate entirely on the pinned offline packet and expected structured artifacts. A blinded human preference study may be reported as secondary qualitative evidence using randomized labels and a documented rubric. An LLM judge cannot be the primary grader.

Inspect AI may be used for scoring and log analysis, not as the generation condition. A promptfoo exporter is optional. No unlicensed prompt, evaluator, or harness code is copied from Superpowers Quorum or other prior art.

### 12.8 Run artifacts and publication

Each run emits a sanitized, hash-linked evidence directory containing:

- `run.json` and the exact request;
- sanitized event log and standard output/error;
- available publishable host transcript fields;
- before-state manifest and produced patch or artifacts;
- verifier output, including JUnit when supported;
- task score and subscore explanation;
- SHA-256 hashes of prompts, condition, task, fixture, model/client identity, and publishable artifacts;
- quota/billing evidence available through supported client interfaces or the recorded user attestation; and
- infrastructure, oracle, failure-classification, and retry status.

Streams pass through sanitization before durable storage where supported. Unavoidable raw capture follows the one-hour-TTL, heartbeat-protected quarantine and next-startup/doctor scavenging rules in §10.4 and never enters public hashes or backups. Publication is allowlist-based, fails on unclassified fields or secret-scanner findings, attempts immediate quarantine destruction on failure or cancellation, and emits a sanitized manifest recording cleanup status and which artifacts were included. Artifacts exclude secrets, account emails, absolute home paths, private reasoning, and unrevealed holdout content. The analysis script reconstructs every published table from public artifacts.

## 13. Testing and continuous integration

### 13.1 Test layers

1. **Schema tests:** valid and invalid canonical contracts, version migrations, and provenance labels.
2. **Compiler unit tests:** deterministic rendering, capability negotiation, diagnostic precision, and stable manifests.
3. **Adapter golden tests:** native output snapshots for all four hosts.
4. **Runtime and conformance tests:** task and approval receipts, budgets, required handoff fields, statuses, permission behavior, dissent retention, enforcement classification, doctor checks, and unsupported-capability failures.
5. **Installer transaction tests:** dry-run, conflict, backup, rollback, update, modified-file uninstall, invalid target, concurrent invocation, and kill-at-every-step recovery.
6. **Behavior tests:** routing minimality, Abhimanyu Gate, prompt-injection resistance, resource ceilings, evidence labeling, and cultural boundaries.
7. **Token-budget tests:** prompt and resource sizes remain within declared ceilings.
8. **Security tests:** path traversal, symlink swap, hardlink, case-folding, backup leakage, secret redaction, hostile archive, untrusted document instructions, release-attestation failure, and manifest tampering.
9. **Benchmark harness tests:** fixture reset, arm isolation, ordering, stop rules, grader determinism, artifact hashing, and statistical reconstruction.
10. **Documentation tests:** links, source labels, generated platform matrix, and claim-registry references.

### 13.2 CI gates

Pull requests must pass formatting, type checking, unit and integration tests, generated-drift checks, all adapter golden tests, installer safety tests, behavior tests, claim lint, secret scanning, dependency/license checks, and documentation validation on supported operating systems. Release CI additionally builds bundles, verifies reproducibility, generates checksums, an SBOM, and a third-party `NOTICE`/attribution manifest, runs managed-runtime and platform doctor checks, and creates mandatory GitHub OIDC-backed artifact attestations. Installation verifies that provenance is bound to `callmearya/mahabharata-council`, the approved release workflow and ref policy, and the recorded source commit; a missing or invalid attestation is fatal.

No CI job performs live subscription benchmark runs on untrusted pull requests. Live evaluations are manually dispatched from a trusted commit with a recorded environment manifest.

## 14. Security, privacy, and threat model

Primary threats include prompt injection through retrieved content, permission escalation, destructive tool use, credential leakage, unsafe installer paths, compromised release artifacts, cross-run contamination, benchmark data leakage, and persona framing that suppresses dissent.

Controls include:

- clear instruction/data boundaries and hostile-input tests;
- least-privilege host capabilities and explicit L3 approval;
- isolated destructive tests and reversible transactions;
- checksums plus mandatory repository/workflow-bound artifact attestations, ownership manifests, SBOM, and third-party attribution;
- credential and path redaction in receipts;
- disposable benchmark workspaces with remotes removed;
- encrypted holdout and post-run reveal;
- independent integrity escalation and dissent preservation; and
- a public `SECURITY.md` with private vulnerability-reporting instructions.

Host limitations are documented rather than masked. The system cannot guarantee that every third-party host enforces prompt-level restrictions as an operating-system sandbox; users receive an accurate capability and risk matrix.

## 15. Repository documentation and public comparability

The public repository includes:

- `README.md` with bounded claims, quick start, supported hosts, and evidence status;
- architecture and data-flow documentation;
- full agent, workflow, handoff, and failure contracts;
- generated platform capability matrix and version policy;
- cultural source, transliteration, and representation policy;
- threat model, privacy, permission, and installer safety documentation;
- benchmark preregistration, task manifest, statistical plan, raw results, and reconstruction commands;
- claim registry and CI policy;
- prior-art comparison and clean-room differentiation;
- limitations, known failures, and negative/inconclusive results;
- `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `CHANGELOG.md`, `CITATION.cff`, license, and release checksums; and
- issue forms for bugs, cultural concerns, benchmark challenges, and new persona proposals.

## 16. Prior art and independent differentiation

The project documents, links, and credits relevant work without copying protected prompts or implying endorsement.

| Project/specification | Relevant strength | Council distinction |
|---|---|---|
| [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) | Mature engineering workflows, progressive disclosure, durable learnings, PR mechanics, and behavioral eval practice | Independent typed contracts; first-class Kiro; engineering plus knowledge work; cultural governance; minimal council; formal cross-host paired statistics and claim registry |
| [Superpowers](https://github.com/obra/superpowers) | Disciplined skills and test-driven workflows across several coding hosts | Native four-host Council adapters, evidence/lineage contracts, knowledge workflows, and dedicated benchmark governance |
| [GitHub Spec Kit integrations](https://github.com/github/spec-kit/blob/main/docs/reference/integrations.md) | Useful integration and distribution patterns | Council-specific compiler, safe ownership-aware installer, and behavioral contracts |
| [Agent Skills specification](https://agentskills.io/specification) | Portable progressive-disclosure skill format | Uses skills for portable knowledge while keeping host-specific tools, permissions, and orchestration in adapters |
| [Superpowers Quorum](https://github.com/prime-radiant-inc/superpowers-evals) | Real CLI evaluation lab across tools | No code or prompts reused; Council preregisters replication, paired inference, quota stops, isolation, and public claim thresholds |
| [Kesh Cursor Mahabharat Agents](https://github.com/udayjadhav/kesh-cusror-mahabharat-agents) | Existing Cursor-focused character-agent concept | Eleven bounded roles rather than a large fixed ceremonial pipeline; four native targets; typed contracts; knowledge work; Lineage Ledger; cultural provenance; formal evidence |

Compound Engineering's public test and postmortem practices are treated as a serious baseline, not a straw man. Its condition is pinned, documented, and routed before holdout access. The Council README must avoid implying that a feature-count difference is a performance result.

## 17. Authorship and GitHub operating model

The repository lives under `callmearya`. Authorship reflects substantive ownership rather than an artificial 50–50 commit count.

### 17.1 `callmearya` ownership

- architecture and canonical schemas;
- compiler and platform adapters;
- installer and lifecycle safety;
- evaluation infrastructure and statistical implementation;
- CI, integration, releases, and final release coordination.

After the condition prompts are frozen, `callmearya` may act as TEST custodian and author task-specific holdout fixtures only if it has not authored or tuned those prompts. If that separation cannot be truthfully attested, a third named contributor who meets the rule must hold TEST custody.

### 17.2 `viji-saravanan` ownership

- agent contract language and prompt experiments;
- cultural grounding records and source review;
- public DEV tasks, candidate-pool design, and general rubric structure;
- examples, prompt documentation, and interpretive review.

Viji's work is delivered through a substantive branch and pull request with meaningful reviewable changes, not cosmetic or empty attribution commits. Viji does not receive sealed TEST content while prompt tuning remains open.

Authorship, committer identity, authenticated GitHub actor, and reviewer are separate recorded roles. A commit is attributed to `viji-saravanan` only for work Viji actually authored or explicitly directed and approved; switching credentials never manufactures authorship. Before a Viji remote workflow, the account owner must explicitly authorize the contribution, the account-specific GitHub noreply email must be verified while authenticated, and collaborator access must be accepted or a fork-based pull request used. AI-assisted work is described honestly in the pull request according to repository policy.

Repository-local author configuration is used for each branch. Before every remote mutation, the process verifies the exact remote owner/repository, branch, authenticated actor, and intended author/committer; it then verifies the resulting GitHub object. GitHub authentication switching uses the native macOS Git Account Switcher application only when remote operations are needed. The workflow allowlists only `callmearya` and `viji-saravanan`, does not select or modify any other configured account, records the original active state, avoids credential deletion or global Git changes, and restores and verifies the original application state afterward. If the app cannot expose or prove active/restored state, the remote mutation fails closed and waits for the account owner.

## 18. Delivery phases and release gates

### Phase 0: specification and provenance

Land this design, contribution ownership, source policy, prior-art record, threat model skeleton, and claim language policy. No performance claim is permitted.

### Phase 1: canonical core and generated bundles

Implement schemas, the eleven agent contracts, Abhimanyu Gate, eight workflow packs, managed runtime, compiler, five surface profiles across four host adapters, checked-in generated artifacts, and drift tests. Gate: all schema, runtime, golden, behavior, enforcement-label, and capability conformance tests pass.

### Phase 2: safe lifecycle and lineage

Implement journaled installer transactions, per-file ownership manifest, mandatory provenance verification, doctor recovery, receipt/evidence/decision/learning schemas, streaming redaction, and example ledgers. Gate: crash recovery, symlink/hardlink/case-fold, rollback, tamper, modified-file, injection, attestation, and privacy tests pass.

### Phase 3: public documentation and development evaluation

Complete the platform matrix, cultural policy, security docs, prior art, DEV tasks, deterministic graders, and harness dry runs. Gate: all DEV tasks reconstruct from clean public fixtures and no DEV number appears as a performance claim.

### Phase 4: conformance release

Publish the repository and signed/attested GitHub bundles after clean install/update/uninstall and managed/native doctor checks pass on every declared target. The claim registry labels the release performance-unverified; no live benchmark result is required to ship a transparent conformance release.

### Phase 5: sealed benchmark and evidence release

Freeze a condition version and run the disjoint PILOT/CAL feasibility and conservative power gate first. If it passes, assign a non-prompt TEST custodian and commit preregistration, encrypted 24-engineering/24-knowledge holdouts and deterministic reserves, archive hashes, exact baseline manifests, versions, independence audit, failure taxonomy, quota evidence/attestation, and analysis code. Execute three paired repetitions per task under the stop rules in staged engineering, knowledge, and transportability releases; reveal each eligible holdout, publish sanitized raw artifacts, and reconstruct analysis. If the gate fails, publish only the pilot and preserve TEST for a later adequately powered preregistration. Gate: separation-of-duties and evidence-integrity audits pass. Evidence-release wording is constrained by the claim registry whether results are positive, negative, inconclusive, or pilot-only.

## 19. Success criteria

The version-1 software release is successful when:

1. all eleven agents and eight workflows validate against versioned canonical schemas;
2. all five declared surface profiles across four hosts compile deterministically, disclose enforcement levels, and pass their required managed/native conformance suites;
3. generated drift is zero on a clean checkout;
4. project-local install, update, rollback, uninstall, and doctor pass conflict, crash, containment, provenance, and tamper tests;
5. managed-mode behavior tests demonstrate minimal routing, bounded loops, typed approval, explicit status, dissent retention, prompt-injection resistance, and Abhimanyu Gate enforcement, while native-mode advisory limitations are public;
6. cultural provenance exists for every persona and both mechanical and independent interpretive review pass; and
7. every public comparative statement resolves to a valid claim-registry record, including the performance-unverified status.

The MahaBench evidence milestone is successful when all fixed blocks and separation rules complete, every table reconstructs from public sanitized receipts without metered API usage, and negative or inconclusive evidence is published to the same artifact standard as positive evidence. Performance superiority is a possible result, not a software release criterion.

## 20. Resolved decisions

The following choices are fixed for the first implementation plan:

- Name: Mahabharata Council.
- Repository: `callmearya/mahabharata-council`.
- License: Apache-2.0.
- Canonical implementation: strict JSON contracts, a managed validation runtime, and deterministic native adapters with disclosed enforcement levels.
- Supported hosts: Claude Code, Codex, GitHub Copilot/VS Code, and Kiro.
- Distribution: committed generated bundles and direct GitHub releases.
- Core: eleven personas plus the non-persona Abhimanyu Gate.
- Scope: four engineering and four knowledge workflow packs.
- Memory: sanitized, content-addressed, tamper-evident Lineage Ledger receipts plus reviewed curated learnings.
- Evaluation: MahaBench-PoC-1 with a disjoint 12-family-per-domain PILOT/CAL gate, 24 confirmatory task families per domain, three paired TEST repetitions, deterministic primary grading, intention-to-treat rules, exact task-level inference, public sanitized evidence, and a zero-incremental-cost policy.
- Baselines: pinned Compound Engineering for Codex engineering; clean Codex default for knowledge; exact native defaults for secondary Kiro and Copilot transportability tests.
- Authorship: substantive foundation ownership by `callmearya`; prompt, cultural, DEV, example, and documentation ownership by `viji-saravanan`; sealed TEST custody separated from prompt work.
- GitHub safety: explicit account-owner authorization, verified account-specific identity, native account switching limited to the two intended accounts, repository-local Git identity, and no modification of other accounts, global credentials, or everyday app configuration.

## 21. Normative and research references

The implementation locks the exact documentation revisions it targets. The design baseline includes:

- [BORI Electronic Mahabharata: Critical Edition statement](https://bombay.indology.info/mahabharata/statement.html)
- [University of Hyderabad Sanskrit transliteration reference](https://sanskrit.uohyd.ac.in/manual/transliterate/)
- [Agent Skills specification](https://agentskills.io/specification)
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents)
- [VS Code custom agents](https://code.visualstudio.com/docs/agent-customization/custom-agents) and [subagents](https://code.visualstudio.com/docs/agents/run/subagents)
- [Kiro custom-agent configuration](https://kiro.dev/docs/cli/custom-agents/configuration-reference/) and [agent creation](https://kiro.dev/docs/cli/custom-agents/creating/)
- [GitHub commit attribution](https://docs.github.com/en/account-and-profile/concepts/email-addresses), [personal-repository collaboration](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/repository-access-and-collaboration/permission-levels-for-a-personal-account-repository), and [artifact attestations](https://docs.github.com/en/actions/concepts/security/artifact-attestations)
- [RFC 8785: JSON Canonicalization Scheme](https://www.rfc-editor.org/rfc/rfc8785)
- [Inspect AI](https://inspect.aisi.org.uk/)

Implementation work begins only after this written specification is reviewed and accepted, followed by a concrete execution plan.
