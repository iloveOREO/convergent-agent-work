---
name: convergent-agent-work
description: Keep extended, iterative, or multi-workstream agent tasks convergent when repeated tool calls, review/fix cycles, subagent coordination, or context growth risk duplicating work. Do not invoke for ordinary short tasks merely because they use multiple tools.
metadata:
  short-description: Keep extended agent workflows focused and convergent
---

# Convergent Agent Work

Complete the requested outcome thoroughly while making each additional action reduce uncertainty, change the artifact, or verify a material risk. This skill is guidance for convergence, not a fixed tool, token, time, or subagent limit.

Do not abandon work, skip required checks, or claim completion merely to save usage. When a workflow starts repeating, change its shape instead of lowering the quality bar.

## Maintain a compact working ledger

Track only the state needed for the next decision:

- the user's acceptance criteria and explicit constraints;
- confirmed findings, deduplicated by root cause;
- files or external objects materially changed;
- validations already run and the exact state they covered;
- unresolved risks and the next evidence that can resolve each one.

Update this ledger after a material change or finding. Reuse it instead of rereading the whole conversation, replaying large outputs, or asking another agent to reconstruct the same state.

## Require incremental value

Before another tool batch, subagent, retry, or review pass, identify internally:

1. What unresolved question will this answer?
2. What new evidence should it produce?
3. How could that evidence change the next action?

If there is no concrete answer, do not run the step. Reuse existing evidence, make the supported decision, or narrow the check to the remaining uncertainty. Do not spend a model turn merely to gain reassurance.

Numbers such as tool-call count, elapsed time, context size, or agent count are warning signals, not automatic stop conditions. Continue while the work is making meaningful progress; restructure when actions become repetitive.

## Use tools economically

- Batch independent read-only checks in one call when their outputs can be interpreted together.
- Prefer targeted searches, bounded log excerpts, and structured queries over full-file or full-log dumps.
- Do not rerun an unchanged command unless relevant inputs changed, the earlier result was incomplete, freshness is material, or controlled repetition will measure nondeterministic behavior.
- After a failure or timeout, form a specific hypothesis before the next retry. For an external mutation with an ambiguous result, first reconcile the authoritative state. Retry only when the mutation is confirmed absent or safely idempotent; otherwise stop and report the uncertainty before making another write.
- Group related edits into coherent patches. Reinspect the resulting diff rather than repeatedly reopening unchanged files.
- Start with focused validation, then run the complete checks required by repository policy or by the actual risk of the change.
- Do not rerun a broad validation suite after a change that cannot affect it, unless project instructions explicitly require that suite.
- Summarize large outputs immediately into the working ledger and retain links, paths, identifiers, or narrow excerpts needed for follow-up.

## Delegate without multiplying the same work

Use subagents only for concrete, independent workstreams that can proceed in parallel or require a genuinely different risk focus.

- Assign non-overlapping scopes and state the expected artifact or decision.
- For a long parent task, use `fork_turns: "none"` or the smallest useful positive turn count and provide a focused context packet. Avoid `fork_turns: "all"` unless the inherited history is both small and necessary.
- Prefer one capable reviewer per risk surface over several broad reviewers examining the same diff.
- Reuse an existing agent with a targeted follow-up when it already owns the relevant context.
- Wait on active agents in a bounded group rather than polling each agent repeatedly. Do useful local work while independent work runs.
- When agents disagree, adjudicate from code, tests, logs, or primary sources. Do not spawn another general reviewer merely to break the tie.
- Interrupt or retire agents whose scope is obsolete, duplicated, or already resolved.

## Make reviews converge

Begin with a risk-based review that covers the material change. Parallel reviewers are appropriate only for distinct surfaces such as correctness, security, compatibility, or deployment behavior.

When a review finds an issue, preserve the user's requested action boundary. For review-only work, record the evidence and report the finding without modifying artifacts or external state. When implementation is authorized:

1. Record the root cause and affected surface.
2. Fix the issue coherently.
3. Run the most relevant validation.
4. Recheck the changed surface or unresolved finding, preferably with the same reviewer.

Do not restart a full review after every small fix. Expand review scope only when the fix crosses a new risk boundary, invalidates earlier evidence, introduces substantial new code, or the user explicitly requests another exhaustive pass.

Treat these patterns as a convergence warning:

- creating reviewers named `v2`, `v3`, `final`, `release`, `ultimate`, or similar for substantially the same scope;
- asking one agent to validate another agent's validation without a distinct unresolved risk;
- repeating the same tests or status reads against unchanged state;
- producing new review prose without new code, evidence, or a changed decision;
- continuing to search after the acceptance criteria and risk-proportionate verification are already satisfied.

On a convergence warning, perform a convergence reset:

- deduplicate findings and preserve only evidence-backed issues;
- identify what changed since the last valid review;
- reduce the next check to that delta and any still-open risk;
- decide from the resulting evidence whether to finish, continue implementation, or hand off remaining work.

For a nontrivial review with no blocker, give a short explanation of what was checked and why it appears sound. Reserve a bare `LGTM` for genuinely trivial, self-evident changes.

## Control context through summaries and handoffs

- Carry decisions and evidence summaries forward, not raw command transcripts.
- Keep subagent prompts self-contained and small; include only the relevant diff, files, constraints, and known findings.
- When the user adds a materially different objective, separate it conceptually from the current work and avoid replaying unrelated history into every action.
- If a long task's accumulated context is dominating each continuation, compact the working ledger into a concise summary and continue from it rather than ending the task.
- Use a fresh-task handoff only when the user requests it, the current environment cannot continue safely or reliably, or a genuine blocker requires external input. Finish the current safe unit before handing off.
- A handoff must state completed work, current state, verified evidence, unresolved risks, and the exact next action. It must not disguise incomplete work as complete.

## Finish on evidence, not reassurance

Finish when the requested outcome and acceptance criteria are met, required verification has passed, and remaining uncertainty is disclosed in proportion to risk.

Do not add a ceremonial final review after those conditions are satisfied. Conversely, do not skip a required check or provide a shallow answer because the workflow has become expensive. The remedy for waste is better scoping, batching, reuse, and handoff—not reduced correctness.
