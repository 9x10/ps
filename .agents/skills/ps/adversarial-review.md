# Adversarial review

Use this playbook for a standalone review and before declaring an implemented change ready. The coordinator runs the panel and returns the verdict. An agent assigned as a reviewer follows the [reviewer brief](reviewer.md) and returns its own report.

## Prepare the candidate

A standalone review is read-only unless the user authorizes edits. Keep cleanup suggestions from read-only review in the findings.

For changed code, run the [simplification pass](playbooks.md#simplify-the-change) before final verification and independent review. Apply the same preparation to each revised code candidate. Use post-pass checks that already satisfy final verification for that candidate and tested base. Reuse recorded preparation only when it covers the same candidate, tested base, and current task and edit scope. Require evidence that the review followed applicable preparation before relying on a verdict for readiness or merge. If a code-changing PR already shows ready while that evidence is missing, report the visible state and treat the PR as unready until verified.

## Freeze the review scope

Record the original goal, acceptance criteria, fixed base, exact candidate, and checks already run. Use commit IDs when possible. For uncommitted work, capture a complete snapshot that includes new files. Make that same base and candidate available to every reviewer and to the reruns. Use separate scratch state for probes that need writes or running services.

## Run an independent panel

Start at least two fresh reviewer agents that did not write the change. Give each the [shared worker brief](worker.md#worker-brief) and [reviewer brief](reviewer.md), with identical intent, base and candidate, and raw check evidence. Assign different areas to probe, such as observable behavior and failure or recovery paths. For high-risk changes, give one reviewer a specific focus on the risk. Keep author conclusions and peer findings out of the initial briefs.

Use separate contexts even if reviewers run sequentially. When delegation is unavailable, start fresh Codex sessions from verified candidate checkouts and supply the brief on stdin. Use a read-only sandbox for enforced write restrictions, for example `codex -a never exec --sandbox read-only --ephemeral -`. Use isolated test checkouts for probes that need to write. Mark the panel incomplete if fewer than two independent reviewers complete their probes or [any required review focus lacks evidence](worker.md#account-for-every-required-slice).

## Blast-radius evidence

For a risky change, use the [blast-radius check](blast-radius.md) to identify the required assumptions and verify the evidence on the prepared candidate. Assign one panel reviewer to trace an indirect dependency or risky edge independently and run an authorized counterexample probe. The same run can count for the independent live rerun if it also satisfies that requirement. Keep missing, refuted, or stale required evidence visible in the verdict.

## Required-check changes

Use the [required-check gate](verification.md#protect-required-checks) on PR candidates, including test- or CI-only changes. If a required check is weakened, assign its original rerun to a non-author. Keep the rerun and explicit approval as required evidence. A reviewer can also do the independent live rerun by completing both assignments. Record original failures and the scope of any approved change in the verdict.

## Independent live rerun

Before a PR that changes observable behavior can be ready, assign one of the independent panel reviewers to rerun the main affected user flow. Choose the flow and its expected outcome from the acceptance criteria and diff. Give the reviewer the exact PR head and tested base, plus the [verification recipe](verification.md) or equivalent steps for driving the real entry point in an authorized test environment.

The reviewer who runs the flow must have had no part in writing the change. Require their own successful run and observations even when the rest of the panel finds no issues. Record the reviewer, flow, head, tested base, commands or actions, and expected and observed results with evidence. If the reviewer observes unexpected behavior, rerun it as a reported failure. If a current passing rerun is missing or blocked, keep the PR out of ready.

Before opening a behavior-changing PR as ready or marking it ready, read the live head and base and match them to the rerun evidence. Use a revision guard where supported. Check the revisions again after the action. If a behavior-changing PR already shows ready but lacks a current passing rerun, treat it as unready. Return it to draft when authorized and supported. Otherwise report the visible ready status and what prevents correcting it.

## Rerun each reported failure

For every reported failure, have the coordinator or a fresh verifier other than the original reporter repeat its exact command or steps. Restore the same relevant input and test state on the same candidate. Check that the probe is within scope and uses an authorized environment before running it. Record the rerun and what it observed.

- Confirm a finding when the rerun reproduces the claimed failure and that failure violates the goal, acceptance criteria, or an applicable contract.
- Drop a finding when a valid rerun disproves the claim or the reproduced result does not violate a requirement. Record why. A single passing run leaves a credible intermittent failure unverified unless other evidence explains it.
- Mark a report unverified when the rerun cannot be completed or the result cannot establish the outcome. Record the missing input, access, or evidence.

For a confirmed PR failure that might already exist on the base, [compare it with the tested base](pr-triage.md#attribute-the-failure) before deciding whether it blocks the change. A proven unrelated base failure can be a follow-up when it is outside the acceptance criteria and required gates.

Rerun every report before consolidating findings that share a root cause. Code traces can guide a probe. Confirmation requires a run.

## Return the verdict

Report the base and candidate, panel coverage, blast-radius evidence when required, independent live rerun evidence when required, confirmed findings and follow-ups with rerun evidence, dropped findings and reasons, and anything unverified.

- `CHANGES_REQUIRED` when at least one confirmed blocker remains, including a verified counterexample to a required safety assumption. Identify incomplete checks separately.
- `INCONCLUSIVE` when there are no confirmed blockers and the panel is incomplete, required preparation is missing, a required check or independent live rerun has not been shown to pass, required blast-radius proof is unproven or stale, the [required-check gate](verification.md#protect-required-checks) is unresolved, or a reported failure remains unverified.
- `PASS` when required preparation is recorded, the panel is complete, required checks and any required independent live rerun passed, required blast-radius evidence is current and supports the safety assumptions, the [required-check gate](verification.md#protect-required-checks) is satisfied, every reported failure has a valid rerun, and no confirmed blockers or unverified reports remain.

For a standalone review, end with the verdict. During implementation, fix confirmed blockers, prepare the revised candidate before final verification, and run a fresh panel. Refresh the review if the tested base changes.
