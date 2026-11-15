# Roadmap — Syllenda

Build progressively. **A working, testable prototype beats an ambitious unfinished AI integration.**

| Milestone | Outcome | Issues | Completion test |
|---|---|---|---|
| **M0 — Foundation** | Running website with a sample week | S-01–S-03 | Open the website and see example classes |
| **M1 — Manual planner** | Tasks are stored and scheduled | S-04–S-10 | Enter tasks and generate a conflict-free plan |
| **M2 — Personal automation** | Rules create review and weekly tasks | S-11–S-12 | One lecture creates one review task; reruns do not duplicate |
| **M3 — Import and control** | Read a timetable file and adapt plans | S-13–S-15 | Import `.ics`; complete/replan tasks safely |
| **M4 — Smart/connected app (later)** | AI input and calendar sync | S-16–S-18 | All external changes require confirmation |

## M0 — Foundation

Use fake events first. Focus on learning the interface, data flow and how changes appear in a weekly calendar.

## M1 — Manual planner

The first real milestone. A user can input existing commitments and a task, preview suggested time blocks, save them and come back later. Test scheduling logic separately from the UI.

## M2 — Personal automation

Introduce templates: review after class, then fixed weekly routines. Avoid a full recurrence-rule engine initially; start with explicit weekly weekday rules and a limited planning horizon.

## M3 — Import and control

Import academic timetables from `.ics`, protect fixed commitments, and give the user the ability to complete, move and replan tasks. Support repeated import without duplicates.

## M4 — Smart/connected app

Add AI to interpret intentions, then optional OAuth calendar connection, accounts and a hosted version. Consider a PWA before native mobile apps.

## Engineering rhythm

1. Pick **one** GitHub issue; keep its scope small.
2. Create a dedicated branch (`feat/s-07-planner`).
3. Build and test that behavior.
4. Open a pull request with a screenshot and a short test summary.
5. Merge and choose the next issue.

Revisit this roadmap after M1. It is deliberately flexible.
