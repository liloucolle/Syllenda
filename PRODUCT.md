# Product brief — Syllenda

## One-sentence pitch

Syllenda is a smart planner for students that converts classes, assignments and recurring responsibilities into realistic study sessions around their existing schedule.

## Problem

A timetable shows *where students must be*, but not *when they will do the follow-up work*. Students repeatedly estimate, schedule and reschedule small tasks; these often get postponed until a deadline is close.

## Target user

Initially: one university student with a fairly structured timetable and a mix of weekly routines, lectures, homework and exams. Expand to other students only after the core workflow works for one person.

## Jobs to be done

- **When I add a class**, I want Syllenda to propose a short review session, so I don't forget to consolidate my notes.
- **When I receive an assignment**, I want to specify its duration and due date, so Syllenda can find time before it's due.
- **When a task is long**, I want to choose between one block and multiple chunks, so I can match how I prefer to work.
- **When I have a weekly routine**, I want to set it once, so it appears in future plans.
- **When I am overloaded**, I want Syllenda to tell me what cannot fit, not silently delete tasks or schedule work at night.
- **When plans change**, I want to adjust or lock scheduled blocks, so I remain in control.

## Core concepts

| Concept | Meaning | Example |
|---|---|---|
| Calendar event | A pre-existing busy interval, normally fixed | Lecture 09:00–11:00 |
| Task | Something to complete, with a time budget | Physics homework, 120 min, due Friday |
| Routine / template | A rule that creates tasks | Groceries every Saturday; review after each lecture |
| Study block | An allocated interval that contributes toward a task | Physics homework, Wed 17:00–18:00 |
| Preferences | The user's acceptable scheduling windows and constraints | Weekdays 08:00–21:00, max 3 h study/day |

## User journey: first release

1. Student opens Syllenda and sees a week view.
2. Student enters existing classes and other commitments.
3. Student configures hours during which tasks may be scheduled.
4. Student creates a task with a title, 90-minute estimate, due date, and `Split into sessions` selected.
5. Student clicks `Generate plan`.
6. Syllenda proposes, for example, three 30-minute sessions in free slots before the deadline.
7. Student approves, edits or rejects the suggestion.
8. If no workable solution exists, Syllenda lists the remaining unscheduled time.

## Release scope

### V0 — working proof of concept

- Manual entry of busy events; responsive week view.
- User settings: timezone and allowed scheduling hours/days.
- Manual tasks: title, duration, due date, split policy.
- Deterministic schedule generator; proposed vs. accepted blocks.
- No overlaps; no booking outside allowed hours; explicit unscheduled items.
- Simple persistence so refreshing the page does not erase data.

### V1 — useful personal study assistant

- Review-after-class rule, configurable per class/category.
- Weekly recurring tasks.
- `.ics` timetable import with de-duplication and recurrence handling.
- Mark complete; move or lock sessions; regenerate remaining work.
- Basic tests for edge cases and timezone behavior.

### V2 — connected and intelligent

- Live calendar connection (Google Calendar first, if desired) with explicit user permissions.
- AI-assisted task creation from natural language or course material.
- Suggested time estimates and revision cadence, always reviewable by the user.
- Optional accounts, multi-device sync, notifications and installable PWA.

## Outside the initial scope

- Native iOS/Android apps.
- Automatically reading students' emails or private files.
- AI agents that autonomously change external calendars.
- Group scheduling, collaboration, subscriptions or complex optimization.

## Acceptance criteria for the first usable version

- Two fixed calendar events cannot be overlapped by generated work.
- A 120-minute splittable task gets 120 minutes of planned work, or a visible unscheduled remainder.
- A single-block task is never split without permission.
- No task block starts after its deadline or outside permitted hours.
- Running `Generate plan` twice without changes does not duplicate blocks.
- A user can accept, edit and remove proposed/accepted blocks.
- Data survives page refresh.

## Design questions to decide later

- Should review happen within 24, 48 or 72 hours of a class by default?
- What counts as an appropriate chunk (e.g. minimum 25 minutes)?
- Should the scheduling engine prioritize upcoming deadlines, task priority or variety?
- Should import be a one-time snapshot or a subscription to a timetable feed?
- What is the default treatment of lunch, travel time and personal events?

**Current assumptions:** no auto-booking of existing commitments; 30-minute default review; preview before saving proposed blocks; no silent overbooking.
