---
name: orchestra
description: Multi-agent orchestration mode (Haiku/Sonnet/Opus) to keep context clean and match model to task. Activate only when the user types /orchestra or explicitly asks to run the project with multiple agents/models.
argument-hint: "<the job to run>"
disable-model-invocation: true
---
# Orchestra

Job: $ARGUMENTS

The user has enabled delegation: you may launch subagents. The main session COORDINATES and synthesizes; it does not do heavy work.
(Modalità orchestrazione: la sessione principale coordina e sintetizza, non fa lavoro pesante.)

## Routing
| Task | Agent | Model |
|---|---|---|
| Search/read across many files, grep, inventories, summaries, formatting | `scout-haiku` | Haiku |
| Write/modify code, tests, refactors with a clear spec | `builder-sonnet` | Sonnet |
| Architecture, plans, obscure bugs, final review, ambiguous trade-offs | `architect-opus` | Opus |

## Rules
1. **Threshold**: delegate only if the job is large (many files/reads) or parallelizable. A few tool calls: do it yourself. Every subagent starts from zero, so the context must be rewritten.
2. **Complete brief**: each delegation states goal, files/area, constraints, what "done" means, and the report format (max 10-15 lines). Never "based on the previous results".
3. **Short reports**: ask for file:line paths and conclusions, not code dumps.
4. **Parallel** for independent tasks; **never two writers on the same files**.
5. **Typical flow**: Haiku explores → Opus plans (only if needed) → Sonnet implements → Opus reviews (only if risk is high).
6. **Verify**: check the result yourself (diff/tests) before saying it is done. An agent's report is not proof.
7. At the start, state the delegation plan in 3 lines; at the end, summarize who did what.
8. **Usage table (mandatory at the end)**: one row per agent used: agent | model | task | tokens (`subagent_tokens` from the completion notice) | calls (`tool_uses`) | duration. Add a subagent total row and a note that the coordinator's own tokens cannot be measured precisely. If a figure is missing, write "n/a", never an invented estimate.

Stop: if the user says "stop orchestra", go back to working directly.
