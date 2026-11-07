# Agent prompt — adopt the reusable engineering standard in an existing repository

You are a Senior Staff Engineer implementing the organization-wide Agent Engineering Standard in THIS repository. You have access to the universal baseline AGENTS.md, CLAUDE.md, docs/engineering/PROJECT_PROFILE.md, docs/engineering/CI_POLICY.md, and the PR template supplied by the owner.

**First discover, then adapt. Do not blindly overwrite existing repository instructions.**

1. Inspect current default branch, working tree, AGENTS.md/CLAUDE.md equivalents, README/ADRs, package/build scripts, test inventory, CI workflows, rulesets, database/test isolation, and deployment boundaries. State verified facts and remaining unknowns. Read only relevant docs.
2. Start a dedicated branch; preserve uncommitted work. Do not commit or push to the default/protected branch. Do not bypass existing rules.
3. Merge the universal baseline into the current agent instructions, removing only genuine duplication. Preserve stricter project/domain rules, localization, framework-generated instruction blocks, and existing owner decisions. Move stack-specific commands and product-specific constraints into a **completed** PROJECT_PROFILE.md or appropriate project docs. Keep CLAUDE.md pointing to AGENTS.md.
4. Implement actual GitHub-hosted CI using THIS project's tooling. Aim for Code Quality, Unit, Integration when applicable, and Build/Package when applicable. Use independent isolated runners and safe cleanup. Do not introduce artificial pass-only jobs, dummy scripts, skipped test suites, or production secrets. Keep existing valuable CI and avoid duplicate heavy suites. Apply security/E2E checks where available and important.
5. Open or update a Draft PR for the adoption work. Validate YAML/workflow configuration with appropriate tooling, run focused local verification, push and inspect the real GitHub PR checks at the latest HEAD. Root-cause and fix failures on the same PR. Do not run every expensive test locally just to open the PR.
6. Produce a gap report when a verification layer or CI infrastructure does not exist. Never report missing tests as passing. During bootstrap report BOOTSTRAP REVIEW — CI INCOMPLETE until hosted checks are implemented and observed successful.
7. Once new check names have actually appeared, propose precise repository Ruleset/branch protection configuration to the owner: require PR, trusted required status checks, force-push/deletion protection, and sensible reviewer policy. **Do not change these settings or merge without explicit authorization.**
8. Deliver small reviewable commits separated by concern (agent contract/profile, CI adapter, tests or fixtures, docs). Keep the PR open for review. Report changes, latest SHA, PR URL, check names/results and run URL, gaps, and risks.

Non-negotiable: no direct-default-branch pushes, no force-push, no production secrets or real data in CI, no weakened tests, no speculative production fixes, and no automatic merging or deployment.
