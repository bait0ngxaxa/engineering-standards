# Agent Engineering Contract

**Standard:** Universal Agent Engineering Standard v1.0.0  
**Scope:** All coding agents operating in this repository.  
**Project adapter:** `docs/engineering/PROJECT_PROFILE.md` (must be completed for this repository).

This document defines how work is performed. The project profile defines what this project uses. Read only the relevant project documents for the task; do not load an entire documentation tree for a minor change. More specific repository instructions may add constraints but must not silently weaken security or delivery protections.

## 1. Non-negotiable principles

- **Security, privacy, and data integrity are constraints, not trade-offs.** Within those boundaries prioritize correctness, maintainability, performance, then delivery speed.
- Understand the current behavior, requirement, affected boundary, and acceptance criteria before changing code. Read the relevant source and tests; do not infer behavior from names alone.
- Diagnose from evidence. Distinguish confirmed root causes from hypotheses and explain any remaining uncertainty.
- Make the smallest cohesive change that satisfies the requirement. Preserve unrelated behavior, public contracts, user work, and compatibility unless an approved change explicitly requires otherwise.
- Do not silently invent product policy, permissions, data meanings, or undocumented third-party API behavior. Consult the applicable accepted decisions or request a decision when a sensitive ambiguity blocks safe implementation.
- Verify external library APIs and version-dependent behavior through installed documentation, official docs, or other trustworthy current evidence when material; do not perform redundant research for established behavior.
- No unnecessary frameworks, dependencies, complexity, feature flags, or speculative optimization. Never disguise an incomplete implementation as production-ready.

## 2. Task intake and impact assessment

- Confirm scope, intended behavior, acceptance criteria, and which code paths or users may be affected. For simple changes this may be brief; for cross-cutting or high-risk changes document a small plan before implementation.
- Before modifying an established business rule, check relevant approved requirements/ADRs and identify affected roles, resources, data, API/transport paths, background jobs, and existing tests.
- Identify risks appropriate to the task: access control, privacy, integrity, migration, backward compatibility, retry, race, failure recovery, performance, and deployment.
- If instructions conflict, follow the latest explicitly confirmed authorized requirement within non-negotiable safety constraints, then accepted project decisions, then the project profile. Treat existing code/tests as behavior evidence, not unquestionable product authority. State unresolved conflicts rather than guessing.

## 3. Implementation and architecture

- Follow the existing project architecture and dependency direction documented in the project profile. Do not transplant a framework-specific architecture from another repository.
- Keep transport/request handling, policy/authorization, application behavior, and persistence responsibilities separated where applicable. Reuse existing domain logic instead of duplicating rules across UI, APIs, jobs, or integrations.
- Preserve source-level typing, validation, and error contracts appropriate to the language. Avoid unchecked casts, arbitrary type escapes, placeholder logic, and edits to generated/vendor code merely to make checks pass.
- Validate untrusted inputs at system boundaries. Do not trust client-supplied roles, ownership, resource scope, or authorization decisions.
- Do not leak stack traces, internal system details, credentials, sensitive data, or excess personally identifiable information in responses, logs, tests, or artifacts. Use safe diagnostics and data minimization.
- Preserve the encoding, line endings, and localized content of existing files. New text should default to UTF-8 without BOM unless project conventions specify otherwise; inspect diffs for accidental bulk formatting/encoding changes.

## 4. Authorization and data protection

- For every **protected read and write**, authenticate the actor and authorize the specific operation against the target resource and applicable scope on a trusted server-side boundary. A role name or a hidden UI control alone is insufficient. Deny on unresolved or insufficient scope.
- Apply least privilege and preserve tenant/organization/owner boundaries and delegated-access constraints. Prevent data exposure through lists, exports, search, cache keys, file URLs, logs, and background jobs as well as detail endpoints.
- Check authorization again when the operation requires a fresh permission decision; prevent time-of-check/time-of-use gaps when policy and persistence must be consistent.
- Include negative authorization and privacy tests when access-sensitive behavior changes (wrong actor, wrong tenant, revoked relationship, missing scope, or comparable cases).
- Never use production secrets or real user datasets for development, tests, or untrusted PR CI. Never bypass security controls to satisfy a test.

## 5. Stateful operations and compatibility

- Define idempotency for retryable or side-effecting operations when relevant. Use transactions and database constraints to enforce integrity and prevent concurrency races where applicable.
- Schema, storage, or data-contract changes require impact assessment for existing records, migration/backfill, rollout compatibility, failure recovery, and any irreversible action. Do not rewrite applied migrations merely to pass tests.
- Never run destructive data operations against shared, staging, or production systems without the task's explicit authorization, verified environment identity, and an appropriate recovery plan.
- Handle timeouts, unavailable dependencies, partial failures, and duplicate delivery according to the operation's real semantics; retries must be bounded and safe.

## 6. Verification: local fast feedback, hosted full CI

- Choose tests proportionate to the change. During development run relevant focused tests and cheap static checks; add meaningful happy-path and failure-path coverage for behavior changes.
- Use only commands verified in `docs/engineering/PROJECT_PROFILE.md` or current project scripts. Never assume Node, Python, Docker, a database, or a test command exists in every repo.
- Do **not** require a complete local test suite, integration suite, or production build before opening a PR. Hosted CI is the standard full-regression gate. Run full checks locally only for a justified debugging, risk, or user-requested reason; report what actually ran.
- For integration tests, use only approved isolated services and verified disposable datasets; ensure cleanup runs even when tests fail, without hiding the original failure.
- Tests must verify behavior, not merely implementation details. Do not skip tests, weaken assertions, raise timeouts, add retries, or change production logic just to obtain green CI.
- Investigate CI failures from exact logs. Isolate and reproduce the relevant failure when practical, classify confirmed defects versus environmental/flaky symptoms, fix the evidenced cause, and push a new commit.
- Treat **only checks for the current PR head** as merge evidence. Explicitly distinguish PASS, FAIL, PENDING, SKIPPED, and NOT CONFIGURED. CI is not a substitute for code review, security assessment, E2E/UAT, or production release approval.
- If a project does not yet have hosted CI, use the bootstrap process in the CI policy to introduce it on a PR. Report the missing gate truthfully; never represent absent checks as passed.

## 7. Mandatory branch, PR, and review workflow

- For any task that **changes repository files**, work on a dedicated task branch; never start routine modifications on the protected/default branch. Preserve existing uncommitted work when switching or branching.
- **Never directly commit or push to `main` or the project's protected/default branch** for ordinary agent tasks, including documentation, dependency updates, tests, and one-line fixes.
- Deliver every repository change through a PR against the project's designated base branch. A Draft PR may be opened before full CI finishes; keep known failures visible.
- Fix CI and review findings on the same task branch/PR unless a separate scope is explicitly justified. Never use direct-default-branch pushes as a fallback for broken CI or unavailable PR tooling.
- Do not auto-merge, force-push, change branch rules/bypass actors, or deploy without explicit task-specific authorization. Only authorized maintainers approve the final merge, subject to configured repository rules.
- Read-only analysis, reviews, and explanations do **not** require creating a branch or PR.
- If branch/PR/hosted CI capabilities are unavailable, report the blocker and supported evidence. A repository bootstrap PR is allowed to introduce the missing CI, but no claim of full CI certification is permitted until real checks have run.

**Standard delivery sequence:** Understand → Implement + focused local verification → Review diff → Commit on task branch → Push → Draft PR → Full hosted CI → Fix evidenced failures → Review/approval → Merge through PR.

## 8. Required delivery evidence

For changes to repository files, report concisely:

- Scope/behavior changed, significant invariants and any accepted decisions affected.
- Branch, commit SHA, PR URL, and files changed.
- Local checks actually executed and their results; checks intentionally delegated to CI.
- Latest-HEAD hosted CI run URL, required gate conclusions, and relevant test/build totals.
- Remaining limitations, unverified areas, risks, and any requested owner decision.

Use status labels accurately: **CI PENDING**, **READY FOR REVIEW**, **REQUEST CHANGES**, or **BLOCKED**. READY FOR REVIEW requires applicable configured CI gates successful for the current PR head, no known blocking defect, and still requires human review. During CI bootstrap, explicitly report **BOOTSTRAP REVIEW — CI INCOMPLETE** until those requirements are met. Do not claim completion for unpushed local changes.

## 9. Project-specific instructions

`docs/engineering/PROJECT_PROFILE.md` provides the actual stack, architecture references, business invariants, commands, environment boundaries, CI mapping, and deployment ownership. Update it when project facts change. Follow applicable subdirectory instructions without weakening this contract. Keep operational runbooks and technology-specific procedures out of this generic contract.

See `docs/engineering/CI_POLICY.md` for the cross-project CI adoption and enforcement contract.
