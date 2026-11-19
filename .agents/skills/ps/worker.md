# Delegate work with a shared brief

Use this playbook for delegated implementation, investigation, or verification. The coordinator assigns and accounts for all required slices. Workers own their slice and the evidence they return. Use the [reviewer brief](reviewer.md) for independent review and let the [blind evaluation rules](learning.md#evaluate-a-skill-change-blind) govern trial and judge prompts. Keep blind trial tracking with the coordinator.

## Assign the work

1. Define the finish condition and list every required slice and its dependencies before dispatch. Track them in a Markdown checklist or the existing [run record](resume.md#start-and-checkpoint). Give each slice a stable name and one accountable owner. Add newly discovered required slices to the checklist when identified.
2. Pin the starting point for each slice. Give parallel writers separate worktrees or checkouts and disjoint ownership of paths or shared state. Use separate scratch state for checks that can interfere. Dispatch slices whose prerequisites are satisfied.
3. Give each worker the absolute path to this playbook and a self-contained [brief](#worker-brief). For supervised work, checkpoint a distinct assignment attempt ID and proposed owner before dispatch. Activate the attempt in a complete checkpoint after confirming the worker identity and resolving any competing dispatch. If delegation is unavailable, cover authorized slices sequentially where feasible. Report any coverage or independence requirement that remains unmet.

## Worker brief

- **Goal and assignment** — run goal, slice name, owner, expected outcome, dependencies, and the applicable project and role instructions.
- **Pinned starting point** — repository, checkout, exact commit or complete snapshot including intended work in progress, and any prerequisite revisions. Name the candidate to verify when it differs from the starting point.
- **Ownership and allowed actions** — owned code, output or inspection scope, shared areas, and actions permitted by the user. State read-only intent or permitted edits and local checks. List any allowed commit, push, or PR actions explicitly. For checkpointed work, include the run record path and [hold rules](resume.md#pause-at-a-safe-boundary). State when the assignment has no run record.
- **Evidence to return** — acceptance criteria, checks or inspections, expected results, and where to save or link proof. State the endpoint and any required handoff.

Before acting, confirm the assigned checkout, pinned inputs, ownership, and applicable holds. For checkpointed work, read the current run record using the [recovery rules](resume.md#resume-from-a-fresh-session). Stop the affected work and report to the coordinator if required state is unavailable, a pin differs, a prerequisite is missing, or an unexpected shared edit affects the slice. Continue the affected work after the coordinator verifies or corrects the assignment and clears the blocker. Follow project instructions and stay within scope. Keep findings outside your slice visible in the handoff.

For supervised work, include the assignment attempt ID in the brief. Give descendants the same run record and attempt ID, and register their worker IDs and scopes with the coordinator before they write. Report meaningful progress with a checkable result, or give the exact dependency or decision you are waiting for.

Before starting a new batch of writes or resuming after a wait or interruption, read the run record. Write only within your recorded scope under the current active attempt, as its owner or registered descendant. Treat a stopping or superseded attempt, an ambiguous newer handoff, or a possible hold as blocked for writes. Report preserved work and pending actions when stopped.

Return the slice and worker ID, actual starting revision or snapshot, and candidate tested or artifact produced. Give the commands or inspection steps, expected and observed results, evidence links, and remaining limitations. Report what was completed, what failed, and what was blocked. Include uncommitted work or pending actions that matter for pickup.

## Account for every required slice

Match each result to its assigned slice, owner, starting point, and tested candidate. Inspect the artifact and evidence against the brief before accepting it. Recheck affected evidence when integration or a changed prerequisite invalidates it. Resolve worker state before replacing a writer or taking over an overlapping slice. For supervised assignments, use [stall recovery](supervision.md#recover-a-stalled-assignment). Continue independent work where its prerequisites hold.

Report a disposition for every required slice. Record accepted results, failed checks or findings, blockers, and any missing, stale, or unverified reports. Keep required slices in view until they have acceptable evidence or the user changes the scope. An inspection can complete with supported findings when findings are the deliverable. A required check must pass when the requested endpoint depends on a pass. Require acceptable evidence for every slice required by the endpoint before reporting full coverage or completed delivery, marking a candidate ready, or merging. Keep independent work moving when its own prerequisites are met.
