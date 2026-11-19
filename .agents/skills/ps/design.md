# Design before coding

Use this playbook for a requested design or before implementing a risky change. Apply it when the work changes a public contract across callers, needs a coordinated rollout across components, moves ownership of shared state, requires a production data migration, changes authorization, introduces a hard-to-reverse operation, or depends on an uncertain design choice with significant consequences. Include any risks marked by the project. Use the regular task playbook for a small, well-understood change. If the risk emerges during implementation, pause the affected work and use this playbook before continuing.

## Compare approaches

1. State the observable goal, affected callers and entry points, contracts to preserve, and the failure you most need to prevent. Inspect the current code and name the owners of affected data and state. Capture any relevant baseline from the pre-change revision.
2. Compare at least two credible approaches when possible, including the smallest viable change. Sketch the data shape, API or module boundaries, and ownership where they matter. Compare compatibility, failure and recovery, verification cost, and the cost of reversing the decision. If the project's constraints leave one viable approach, record which alternatives were ruled out and why.
3. List the uncertainties that could change the choice of approach. For each one that evidence can settle, run the smallest disposable probe in an authorized scratch environment. Define the question, representative inputs, and decision criterion first. Use the same inputs and criteria when comparing alternatives. Record the exact steps and observations, and keep prototype artifacts out of the implementation. Rule out an approach that fails a required criterion unless a revised version passes the probe. If required evidence is unavailable, record what would settle the question, leave the affected choice unresolved, and block only the work that depends on it.
4. Record the decision or blocker, evidence, and remaining tradeoffs in the handoff or an existing Markdown plan or run record. Make reversible decisions within scope. Ask the user only for a product choice the evidence cannot settle or an action that needs additional authority.

For the chosen approach, use the [blast-radius check](blast-radius.md#trace-the-safety-assumption) to name its critical safety assumptions and plan the probes. Test assumptions that could change the choice with the disposable probes above. Carry the final candidate checks into the phase plan.

## Plan and verify phases

Split the chosen approach into small changes, each with an observable result. Put the most consequential unknown or prerequisite early. For each phase, record:

- The outcome, affected code or contracts, and prerequisites. Use the [delegation playbook](worker.md) for delegated phases.
- The starting state, pinned revision or verified prerequisite, and any comparison baseline.
- The exact check or flow, expected result, and evidence to retain. Choose a check another agent can run without later phases. Use the existing [verification playbook](verification.md).
- What to do if a check fails or is blocked, including any safe recovery needed for a partial change.

Record the phase plan and each check before editing product code for the risky change. For a design-only request, return the comparison and phase plan and stop there. When implementation is already in scope, start with phases whose prerequisites are satisfied.

Run each phase's check and record its observed result before treating the phase as verified or starting work that depends on it. Continue independent in-scope phases when their own prerequisites are satisfied. Parallel writers need separate checkouts and disjoint edit ownership. When a design or prerequisite changes, update the plan and rerun affected checks before relying on them.

After the phases pass, verify the integrated result against the original goal. Follow the normal [engineering workflow](workflow.md) for review and delivery, and [PR orchestration](orchestration.md) when phases are delivered as a queue or stack.
