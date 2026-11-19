# Task playbooks

## Investigate

Write down the question you need to answer. Follow the behavior from a real entry point through the relevant code and state. Run a read-only check or a disposable reproduction when it can distinguish competing explanations. Report what happened, the evidence that supports the explanation, and any open question. Keep the investigation read-only unless edits were requested.

## Fix a bug

Capture a failing example on the base when feasible. Record what you expected and what happened. Trace the example to its root cause. Make the smallest repair and rerun the same example through the affected entry point. Add a regression check if the existing suite misses a meaningful failure. If the original failure cannot be reproduced, say what the available evidence does establish and keep any missing required proof explicit.

## Build a feature

Translate the request into observable acceptance criteria and identify the consumers and contracts that change. Implement a runnable slice through the real entry point. Verify the intended result and a meaningful failure or boundary case. Inspect existing callers and data for compatibility before expanding the change.

## Refactor

Name the behavior and contracts to preserve. Capture a baseline through the affected callers, then make the smallest structural change that earns its cost. Run the same checks with the same inputs against the candidate. Separate intentional behavior changes so their acceptance criteria and proof stay clear.

## Improve performance

Choose the metric, input, and success threshold. Capture a baseline and candidate under comparable conditions. Repeat noisy measurements and report the range and relevant environment details. Confirm the workload still produces valid results. If the data cannot distinguish a change from noise, mark the result inconclusive.

## Simplify the change

Inspect the changed code and the callers or contracts it affects before review. Keep the pass within the task's scope. For a read-only review, record concrete suggestions and leave the candidate intact.

- **Unnecessary wrappers.** Trace a forwarding layer through its callers. Remove it only if the simpler path preserves its interface and responsibilities, including validation, lifecycle, and observable side effects.
- **Duplicated logic.** Look for the same rule implemented in multiple places. Reuse its existing owner or give it one clear implementation when the rule is genuinely shared.
- **Avoidable mutable state.** Prefer a local value or a value derived from its source when state does not need to persist or be shared. Check ordering, retries, and concurrency before removing state that may affect behavior.

Before making an authorized cleanup, establish the behavior of the pre-cleanup candidate for the affected cases. Use existing checks or a targeted real probe and preserve the task's intentional behavior changes. Keep the smallest cleanup that makes the changed code easier to trace. Preserve unrelated edits and report larger redesigns separately.

Rerun the [affected checks](verification.md) after the pass on the exact code that will be reviewed. Use comparable inputs and include affected callers or phase checks. If a cleanup fails or its behavior cannot be verified, restore only the edits made in this pass and verify the restored candidate. Report any missing required proof before claiming readiness. Record what changed or was kept, whether the task allowed edits, the final candidate and tested base, and the observed checks. Use these results for final verification before review.

## Review

Use the [adversarial review playbook](adversarial-review.md) to coordinate reviewers, rerun reported failures, and return a verdict on the exact candidate.

## Follow through on a PR

Identify the PR, tested base, head, required checks, review evidence, and authorized endpoint. Check for a changed head or base before reusing evidence. Use [PR and CI triage](pr-triage.md) to verify feedback, classify failures, and decide whether a CI retry is warranted. After changes, [prepare the candidate for review](adversarial-review.md#prepare-the-candidate), then run final checks and a fresh adversarial review. Before accepting older evidence for readiness, confirm that review followed applicable preparation on the current candidate and tested base. Apply the [blast-radius readiness rules](blast-radius.md#use-the-result-for-delivery) for risky PRs, including when the forge already shows Ready. Use real CI and review status for delivery decisions. Apply the [required-check gate](verification.md#protect-required-checks) before counting changed checks toward readiness. Follow the [independent live rerun and readiness rules](adversarial-review.md#independent-live-rerun) for a behavior-changing PR, including when the forge already shows it as ready. Stop at the requested endpoint or report the precise blocker. Use the [merge gate](orchestration.md#merge-gate) before any merge and [orchestration](orchestration.md) for queues, stacks, or a resumed run. Push, merge, and deploy only within the user's authorization.
