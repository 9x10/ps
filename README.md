# ps

A Markdown workflow for Codex CLI.

## Install once

Make the [ps skill folder](.agents/skills/ps) available at `~/.agents/skills/ps`. Codex discovers user skills there across local projects. On Windows, link it from this repository with PowerShell:

```powershell
$skills = Join-Path $env:USERPROFILE '.agents\skills'
New-Item -ItemType Directory -Path $skills -Force | Out-Null
New-Item -ItemType Junction -Path (Join-Path $skills 'ps') -Target (Join-Path (Get-Location).Path '.agents\skills\ps') | Out-Null
```

Run those commands from this repository and keep its folder at the same path. If you prefer an installation independent of this checkout, copy the whole skill folder to the same user location.

## First run

1. Open a terminal in the project you want to work on and run `codex`.
2. In the Codex prompt, type `$` and select `ps`. You can use `/skills` to check that it’s available. If it’s missing after installation, restart Codex.
3. After `$ps`, describe the outcome, what should be preserved, and where Codex should stop.

```text
$ps Fix export cancellation. A cancelled export must leave no partial file. Preserve successful exports. Stop after local verification and adversarial review.
```

Use the same `$ps` entry point for every task. It picks a playbook for investigation, bugs, features, design before coding, refactoring, performance, repeatable verification, adversarial review, PR follow-through, long-running orchestration, or learning from repeated failures.

```text
$ps Adversarially review this branch against main for export regressions. Rerun each reported failure and report confirmed findings.
```

To save a check that future agents can run, ask for a proven recipe:

```text
$ps Verify the export flow with this project's existing tools and save a repeatable recipe.
```

Codex records the recipe in existing verification docs or `VERIFICATION.md` after a successful run. To cover several important flows, use the [verification map](.agents/skills/ps/verification.md#build-a-verification-map). An audit reports stale checks, product failures, and blocked checks separately.

```text
$ps Map this project's important user flows and save checks proven with its existing tools.
$ps Audit the verification map. Update proven Markdown recipes and report product regressions separately.
```

Before reviewing changed code, Codex runs a [simplification pass](.agents/skills/ps/playbooks.md#simplify-the-change) for unnecessary wrappers, duplicated logic, and avoidable mutable state. It reruns affected checks on the code it sends to review. For a change, expect the checks Codex ran and a verdict on the exact candidate. At least two independent reviewers try to break it. The coordinator reruns every reported failure before accepting it. A behavior-changing PR stays out of ready until a reviewer who did not write the change reruns the main affected user flow and records a pass on that head and tested base. Missing required evidence or an incomplete panel makes a clean verdict inconclusive. A PR that weakens a [required check](.agents/skills/ps/verification.md#protect-required-checks) needs a non-author's rerun of the original on the candidate and your explicit approval before it can be marked ready or merged. If you want a PR or CI follow-through, include that endpoint in your brief.

## Design risky changes

For risky changes, `$ps` compares approaches, uses disposable prototypes to settle material unknowns, and verifies each phase before dependent work starts. A [blast-radius check](.agents/skills/ps/blast-radius.md) traces indirect dependencies and tests the assumption that keeps a risky change safe. Ask it to stop after the plan when you want to review the design first.

```text
$ps Plan the switch to the new export format. Preserve existing API callers, prototype the compatibility approach, and stop after the phase plan.
```

## Delegate work

`$ps` gives each worker a [shared brief](.agents/skills/ps/worker.md) with a pinned starting point, clear ownership, allowed actions, and the evidence to return. The coordinator accounts for every required slice in the handoff.

```text
$ps Inspect the export, reporting, and archive packages in parallel. Give each package an owner and report evidence for every package. Keep it read-only.
```

## Pause and resume long work

For work that spans sessions, `$ps` saves verified phases, decisions, unfinished edits, and the next step in a [Markdown run record](.agents/skills/ps/resume.md). An explicit pause puts the affected workers on hold. Codex records their confirmations, unconfirmed stops, and any pending actions. To release the hold in a fresh session, ask Codex to resume and give it the record path.

```text
$ps Migrate the export callers in verified phases. Keep a run record and stop after local verification.
$ps Pause the export migration and hold its workers.
$ps Resume .ps/runs/export-migration.md
```

## PR queues and stacks

```text
$ps Drive PRs #12 and #15 to reviewed and green. Stop at ready.
$ps Resume .ps/runs/export-stack.md. Finish verification and merge PRs #12 and #15 in dependency order when ready.
```

With [PR and CI triage](.agents/skills/ps/pr-triage.md), Codex verifies bot feedback, retries a likely CI flake once when authorized, and checks whether a failure was already on the PR's base.

During long runs, Codex uses available tools to [watch CI and merges and recover stalled workers](.agents/skills/ps/supervision.md). Watching after a session exits requires a host wake mechanism.

A [merge requires independent verification of the actual landing candidate](.agents/skills/ps/orchestration.md#verify-the-landing-candidate). If the forge cannot enforce that, Codex stops at reviewed and ready once the usual readiness checks pass.

Codex gives you a Markdown run record with decisions, PR state, evidence, and next steps. Use its path in your resume request. Specify merge authority only for the PRs you want landed.

## Get a decision-only handoff

At the end of an unattended run, an independent reviewer checks the [run record and evidence](.agents/skills/ps/handoff.md). Codex gives you the decisions that still need you, each with a recommendation, evidence, and risk. Routine results stay in the run record.

```text
$ps Audit .ps/runs/export-stack.md and give me only the decisions that need me.
```

## Learn from failures

`$ps` captures durable lessons in the handoff and runs a blind comparison when a change alters reusable skill behavior. You can also ask for any of the three plays directly.

```text
$ps Reflect on this run and capture lessons that would help next time.
$ps Find recurring export mistakes and add the smallest check that catches them. Show it failing and passing.
$ps Evaluate my review skill change blind against the previous version on the same task.
```
