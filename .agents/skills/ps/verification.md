# Repeatable app verification

Use this playbook to establish, reuse, or refresh a recipe for a real product flow. Apply it during implementation when that behavior needs proof, or on its own when the user asks for a verification recipe. For a verification-only task, keep edits to verification documentation. Use the [verification map](#build-a-verification-map) for coverage across important flows and the [audit](#audit-the-verification-map) to keep their checks current.

## Find the flow and tools

- Pick one observable flow at the affected entry point, such as a UI action, API request, CLI invocation, or public library call. Name the input, expected result, and what would count as a failure. If a map exists, use it to identify the main flow and any others affected by changes to the product, setup, drivers, commands, or fixtures.
- Look for an existing recipe in project guidance, including `PROJECT.md` or `VERIFICATION.md`, and for known commands in docs, manifests, CI, fixtures, and test tooling. Prefer the project's existing setup and drivers. Use test data and an authorized environment.
- Reuse a suitable recipe by running it against the current candidate. If the entry point, commands, setup, fixture, or expected result may have drifted, [classify the failure](#classify-a-failing-check) before updating the recipe.

## Prove the recipe

1. Prepare a repeatable starting state. Record the working directory, prerequisites, test data, and reset steps. For a service, record how to start it and establish readiness. Use a fresh or isolated checkout when state or concurrent work could affect the result.
2. Drive the real entry point with exact commands or manual steps. Record the expected and observed result, including the relevant exit code, response, output, or visible behavior. Include a relevant boundary or failure case when it is part of the requested behavior. An expected rejection counts as a pass only when the observed rejection matches it.
3. Capture evidence before cleaning up. Record the tested commit or snapshot and any settings that affect reproduction. Stop or clean up only resources created for the check. Keep a redacted result or durable evidence reference that survives cleanup.
4. Save or mark a recipe as proven only after the documented sequence succeeds end to end. If a required step cannot be completed, report the failure or missing prerequisite and the evidence you have. Keep draft steps clearly unverified.

## Save it for the next agent

Add the proven recipe to existing project verification docs. If there is no suitable document and the task authorizes edits, create `VERIFICATION.md` in the target project. Keep findings in the handoff for read-only investigations or reviews. Use Markdown for the recipe and reference the project's existing tools and scripts.

Use one short entry per flow:

- **Flow and revisions tested** — what it demonstrates and the commit or snapshot where it ran
- **Setup and readiness** — working directory, dependencies, fixture, and reset or start steps
- **Run and assert** — exact commands or actions with expected results
- **Observed evidence** — relevant output, exit code, or artifact reference and what it proves
- **Cleanup and limits** — resources to reset, applicable environments, and any untested cases

## Build a verification map

Inventory important user goals from the project's docs, routes, commands, jobs, and public callers. Prioritize common and high-impact flows. List surfaces separately when a UI, CLI, API, or job can fail independently. Record which source paths, shared contracts, and recipe dependencies each flow uses so later changes can find all affected entries.

Keep the map in existing verification docs or in `VERIFICATION.md` in the target project when edits are authorized. Link to existing recipes instead of duplicating them. For each flow, record the user goal and entry point, setup and fixtures, exact check and observable result including meaningful side effects, cleanup, and the last proven revision with evidence. Include the environment or access required. Run the in-scope checks using the [recipe procedure](#prove-the-recipe). Keep blocked or unrun flows visible as unverified until their checks succeed.

## Audit the verification map

1. Compare the map with the current source and documented user contracts. Look for missing, duplicate, or outdated entries, including alternate user surfaces. Update affected entries during routine changes. Audit every mapped flow in the requested scope when the user asks for a map audit.
2. Run each mapped or newly discovered flow in the requested scope through its real entry point on the intended revision using the [recipe procedure](#prove-the-recipe). Confirm readiness and use known test state. After a surprising result, check the runtime and reset test state before relying on another run. Record blocked flows and continue with independent checks.
3. [Classify gaps and failures](#classify-a-failing-check) before editing the map. Update only verification Markdown within the authorized scope. Prove any revised recipe end to end before marking it current. Keep the last proven revision when the new check is blocked. Remove entries only when there is evidence the user flow was intentionally retired.
4. Report coverage on the tested revision, including passed flows, stale checks, product failures or confirmed regressions, and anything blocked or unverified. List what changed in the map and link to retained evidence.

## Classify a failing check

Establish the expected behavior from the task or an applicable documented contract. Preserve the failing output. Confirm you are exercising the intended revision, fixture, permissions, and environment. Where possible, use a separate direct probe of the same user flow with the same relevant conditions so the saved recipe is not the only source of evidence.

- **Stale check.** The recipe, fixture, or driver is demonstrably outdated. Prove the product still meets its intended behavior through the real surface before calling the issue check-only. Preserve the expected behavior unless authoritative evidence shows an intentional change.
- **Product failure.** A valid live probe shows that the current product violates the expected behavior. Call it a regression when a comparable baseline succeeds. When only the baseline is unavailable, report the product failure and leave regression status unverified. Report both findings if an outdated check and a product failure are each established. Keep the expected check visible and report the failure.
- **Blocked or unverified.** Access, environment, candidate identity, or valid evidence of current behavior is missing. Record the missing requirement and the result you could observe. A stale recipe can be established while product behavior remains unverified.

## Protect required checks

Compare the effective required checks on the tested base and PR head before accepting verification evidence. Use the project's requirements, task acceptance criteria, and forge protections to identify the checks that matter. Trace changes to assertions, limits, fixtures, test selection, allowlists, workflow conditions, and failure handling. A check is weaker if it can accept a result the original would reject or can skip cases the original required. Treat unresolved changes to enforcement as unverified.

If a PR would ship or rely on a weaker required check:

1. Pin the original check, including its meaningful configuration, inputs, and dependencies, from the revision that enforced it. Have a reviewer who did not author the change rerun that original check against the exact PR candidate in an isolated, authorized environment. Keep the original enforcement intact. Verify the runtime is the candidate. A run against the base alone, a substitute weaker probe, or a skipped or setup-failed rerun leaves the requirement unverified.
2. Record the PR, head, tested base, original check revision, reviewer, exact steps and observations, and which cases the weaker check would accept or skip. Store the evidence and approval in the run record or verification handoff outside the candidate revision. Show the user the original result, including any valid failure, and the proposed loss of coverage. Address unrelated failures through the normal verification gates.
3. Require explicit user approval after the evidence is available, covering this PR, head and tested base, the specific weakening, and any original failure the user accepts. Verify approval against the current request or an accessible prior user instruction and record its source and scope. A general task or merge authorization, a worker report, and green CI are insufficient. Approval cannot substitute for a valid rerun. Repeat the proof and obtain current approval if the head, tested base, or weakening changes. Treat a withdrawal as a blocker.

Keep the PR out of ready and withhold merge while the required rerun or approval is missing or stale. Recheck the live head and base against the evidence and approval before opening a ready PR or marking it ready. Use a revision guard where supported. Re-read the live head, base, and readiness after the action. If the forge shows Ready without a satisfied gate, including after a revision change, report its visible state, treat the PR as unready, and return it to draft if authorized and supported. A passing run of the weakened check is still required if relying on it, and skipped or disabled checks cannot count as passes. Keep forge protections and approval requirements in force. In a stack, account for any weakening inherited from an unmerged parent before relying on it. Approval of another PR does not cover the current one.
