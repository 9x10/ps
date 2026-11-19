---
name: ps
description: Use the ps workflow when the user invokes $ps or explicitly asks for ps playbooks, design before coding, blast-radius checks, verification maps and live checks, delegation, pre-review simplification, adversarial review, supervising and resuming long-running work, PR and CI triage, PR orchestration, auditing unattended runs, learning from recurring failures, or blind evaluation of skill changes.
---

# ps

Read the bundled [engineering workflow](workflow.md). Follow the current project's applicable `AGENTS.md` files and use its `PROJECT.md` or other project guidance when present. Otherwise, discover setup and checks from the project's code, manifests, and CI. Select the matching [playbook](playbooks.md), state the goal and how you will know it is met, then follow that section.

| Request | Playbook |
| --- | --- |
| Explain, diagnose, or recommend without editing | [Investigate](playbooks.md#investigate) |
| Repair incorrect behavior | [Fix a bug](playbooks.md#fix-a-bug) |
| Find what a change could break or test what makes it safe | [Blast radius](blast-radius.md) |
| Compare designs, prototype a choice, or plan a risky change | [Design before coding](design.md) |
| Add or change behavior | [Build a feature](playbooks.md#build-a-feature) |
| Improve structure while preserving behavior | [Refactor](playbooks.md#refactor) |
| Improve latency, throughput, or resource use | [Improve performance](playbooks.md#improve-performance) |
| Establish or refresh a repeatable way to test an app flow | [Verification](verification.md) |
| Map important user flows or audit saved checks | [Verification map](verification.md#build-a-verification-map) and [audit](verification.md#audit-the-verification-map) |
| Assess changes to a PR's required checks | [Required-check gate](verification.md#protect-required-checks) |
| Delegate implementation or inspect multiple required slices | [Shared worker brief](worker.md) |
| Simplify changed code before review | [Simplification pass](playbooks.md#simplify-the-change) |
| Try to break an existing change and verify the failures | [Adversarial review](adversarial-review.md) |
| Verify bot feedback, triage failed CI, or classify a PR failure | [PR and CI triage](pr-triage.md) |
| Prepare, update, or follow an existing PR | [Follow through on a PR](playbooks.md#follow-through-on-a-pr) |
| Checkpoint, pause, or resume long-running work | [Checkpoint and resume](resume.md) |
| Watch CI or merges, check worker progress, or recover a stalled assignment | [Active supervision](supervision.md) |
| Drive a PR queue or stack, or resume a PR run | [Long-running PR orchestration](orchestration.md) |
| Audit an unattended run or ask what decisions need your input | [Audited handoff](handoff.md) |
| Capture durable lessons from a run | [Reflect](learning.md#reflect) |
| Prevent a recurring mistake with a proven check | [Correct recurring mistakes](learning.md#correct-recurring-mistakes) |
| Compare a skill change against its original blind | [Evaluate a skill change blind](learning.md#evaluate-a-skill-change-blind) |

For runtime proof, use the [verification playbook](verification.md) to run or refresh a project recipe with the existing tools. For delegation, use the [shared worker brief and coverage rules](worker.md).

Coordinate an [adversarial review](adversarial-review.md) with at least two independent reviewers, then rerun every reported failure before accepting it. Require a passing [independent live rerun](adversarial-review.md#independent-live-rerun) before marking a behavior-changing PR ready. If assigned as a reviewer, follow the [reviewer brief](reviewer.md) and return your observations to the coordinator.

Use [checkpoint and resume](resume.md) when work may span sessions. For an explicit pause, apply the [worker hold rules](resume.md#pause-at-a-safe-boundary). For a resume request, read the named record first, check for [unreleased holds](resume.md#release-held-work), and route by its scope. Use [orchestration](orchestration.md) when starting authorized PR delivery or reconciling an existing PR or pending forge action.
