# Check a change's blast radius

Use this playbook for risky changes under [design before coding](design.md), or when asked what a change could break. Keep a standalone blast-radius review read-only. Use authorized test data and isolated state for runtime probes.

## Trace the safety assumption

1. Read the diff, its callers, and the behavior it promises to preserve. Pin the candidate and tested base by revision or complete snapshot. Before implementation, trace the proposed boundary and plan the probe, using a disposable prototype when the design depends on the answer.
2. State the smallest set of assumptions the change needs to be safe. For each, name the affected consumer, what you expect to observe, and a concrete case that would make the assumption false. Prioritize cases that can materially change user behavior or safety.
3. Follow the impact beyond direct callers. Check the relevant data formats, persisted records, flags, job dispatch, timing, and deployment version skew. Where the result depends on a library, inspect the pinned version and any local patch. Cite the code, configuration, or contract that makes the indirect path reachable.

## Try to disprove it

1. Use the project's [verification tools or recipes](verification.md#prove-the-recipe) to build the smallest authorized test that could expose the counterexample. Drive the real entry point if practical. Otherwise exercise the actual affected code and its relevant dependencies. Use test doubles only outside the contract under test.
2. Run the probe on the prepared candidate. Confirm what revision or version each participant actually runs. Verify that the probe reached the affected path and observe the relevant output or side effect. Arrange the triggering inputs, ordering, mixed versions, or previously persisted data when the assumption depends on them. Where practical, use a known failing control to confirm the probe can detect the unsafe outcome. Compare with the same inputs on the tested base when a failure needs [attribution](pr-triage.md#attribute-the-failure).
3. Record the revisions, effective configuration, versions, fixture, steps, expected unsafe result, and what happened. Treat a missed path, a substitute check, or an unavailable test environment as missing proof. Refresh affected evidence if the candidate, tested base, or an input the result depends on changes.

## Record the result

- **Disproved** means the real probe showed a counterexample. Record the changed behavior and who it affects. Fix the issue or revise the design and verify the revised safety assumption when authorized.
- **Supported** means the probe exercised the intended path and held under the tested conditions. Record the conditions tested. If a material condition the safety assumption depends on remains untested, leave that required proof incomplete.
- **Unproven** means evidence is missing or inconclusive. Give the next concrete probe or prerequisite. Static inspection can explain the risk but leaves the runtime result unproven.

## Use the result for delivery

Before accepting a saved review or marking a risky PR ready, verify that the required evidence covers the live head, tested base, and relevant test inputs. Recheck those facts after changing readiness. Keep the PR out of ready and merge while a required assumption is disproved or lacks current support. If the PR shows Ready without current evidence, report that state and treat it as unready. An authorized delivery run may return it to draft when supported. Use the existing [independent review](adversarial-review.md) and [merge gate](orchestration.md#merge-gate) before delivery.
