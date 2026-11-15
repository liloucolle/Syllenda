# GitHub issue backlog — Syllenda

These are **proposed** GitHub issues, not yet created on GitHub. Use the titles and checklists below when creating them. IDs are planning IDs (S-01 etc.), not GitHub's auto-generated `#123` issue numbers.

## M0 — Foundation

### S-01 — Initialize repository and Next.js application
**Labels:** `good-first-issue`, `docs`  
**Goal:** Have a clean app running locally and tracked on GitHub.

- [ ] Create/initialize the `syllenda` repository, Next.js App Router, TypeScript and Tailwind.
- [ ] Keep the project docs in the repo and make the landing page display “Syllenda”.
- [ ] Confirm `npm run dev` works and commit the scaffold.
- [ ] Add `.env*`, real `.ics` files and local database files to Git ignore rules.

**Done when:** the site loads locally and the repository contains no private calendar data.

### S-02 — Build the basic application layout
**Labels:** `frontend`  
**Goal:** Create a usable shell for the calendar and task panel.

- [ ] Add a header/navigation and an empty task sidebar.
- [ ] Add desktop and mobile-friendly layouts.
- [ ] Use sample text and clear empty states; no backend necessary.

**Done when:** the shell adapts cleanly to narrow and wide screens.

### S-03 — Show a weekly calendar with sample events
**Labels:** `frontend`  
**Goal:** Visualize a student timetable.

- [ ] Add a FullCalendar week view using the current React integration.
- [ ] Render sample classes and other fixed commitments.
- [ ] Differentiate existing events visually from future generated study blocks.

**Done when:** example events display at the correct days and times.

## M1 — Manual planner

### S-04 — Define task and calendar-event types
**Labels:** `backend`  
**Goal:** Keep event/task data predictable before introducing persistence.

- [ ] Define TypeScript types for `CalendarEvent`, `Task`, `PlannedBlock` and `Preferences`.
- [ ] Add a few synthetic demo events and tasks.
- [ ] Document fixed events vs. movable blocks and single vs. split tasks.

**Done when:** sample data is type-checked and used by the UI.

### S-05 — Add forms for manual events and tasks
**Labels:** `frontend`  
**Goal:** Replace hard-coded data with editable inputs.

- [ ] Create, edit and delete fixed calendar events.
- [ ] Create a task with title, estimated minutes, deadline and single/split option.
- [ ] Validate positive durations and valid date/time fields.

**Done when:** entered events/tasks appear immediately (initially in memory).

### S-06 — Persist calendar events, tasks and preferences
**Labels:** `backend`  
**Goal:** Do not lose data on refresh.

- [ ] Add Prisma and SQLite, using a version with supported SQLite setup.
- [ ] Define the initial database schema and migration.
- [ ] Implement server-side create/list/update/delete for tasks and events.
- [ ] Keep database files and secrets out of Git.

**Done when:** refreshed pages show previously saved tasks and events.

### S-07 — Implement a deterministic scheduling engine
**Labels:** `scheduler`  
**Goal:** Turn free time into proposed task blocks.

- [ ] Compute available intervals around fixed and locked events.
- [ ] Enforce allowed hours, deadlines, and no overlaps.
- [ ] Place a single-block task only when one suitable opening exists.
- [ ] Split allowed tasks into chunks and return an unscheduled remainder if necessary.
- [ ] Keep scheduler independent of database and UI code.

**Done when:** a function maps test fixtures to reproducible planned blocks.

### S-08 — Unit-test the scheduling engine
**Labels:** `testing`, `scheduler`  
**Goal:** Make scheduler behavior trustworthy.

- [ ] Test exact-fit, insufficient-space, and conflicting-event cases.
- [ ] Test one-block vs. split behavior.
- [ ] Test deadlines, allowed hours and reruns without duplication.
- [ ] Document expected behavior on partial scheduling.

**Done when:** automated tests pass and cover the core constraints.

### S-09 — Preview and confirm generated plans
**Labels:** `frontend`, `scheduler`  
**Goal:** Give the user control over scheduling.

- [ ] Add a `Generate plan` button and display proposals differently.
- [ ] Show unscheduled tasks and how much time could not be placed.
- [ ] Allow accepting or rejecting proposals.
- [ ] Prevent regenerating duplicate accepted blocks.

**Done when:** the student can generate, review and save a conflict-free plan.

### S-10 — Add scheduling preferences
**Labels:** `frontend`, `scheduler`  
**Goal:** Respect the student's real availability.

- [ ] Configure weekday availability windows and timezone.
- [ ] Configure preferred session length, buffers and daily limit.
- [ ] Feed preferences into the scheduler and tests.

**Done when:** Syllenda does not plan outside configured hours or daily limits.

## M2 — Personal automation

### S-11 — Create a review task after each class
**Labels:** `automation`  
**Goal:** Automatically make time to revisit lecture notes.

- [ ] Add an opt-in `Review after class` rule with default 30 minutes.
- [ ] Generate a review task tied to each eligible class occurrence.
- [ ] Set a configurable review deadline (default 48 hours later).
- [ ] Ensure rerunning the rule never duplicates the same occurrence.

**Done when:** one class creates one review task, not two.

### S-12 — Support weekly recurring routines
**Labels:** `automation`  
**Goal:** Plan groceries and administrative tasks automatically.

- [ ] Let the user define a weekly task, weekday and estimated time.
- [ ] Generate instances for a bounded upcoming window.
- [ ] Avoid duplicate instances on repeated generation.

**Done when:** a weekly shopping rule produces one task per chosen week.

## M3 — Import and control

### S-13 — Import timetable events from `.ics`
**Labels:** `integration`  
**Goal:** Stop entering every lecture by hand.

- [ ] Accept a local `.ics` upload and preview recognized events.
- [ ] Parse normal events, timezone information and bounded recurrences.
- [ ] Preserve unique source identities and avoid duplicates on re-import.
- [ ] Treat imported events as fixed and display import errors clearly.

**Done when:** a sample `.ics` timetable produces the expected events twice without duplication.

### S-14 — Complete, move and lock study sessions
**Labels:** `frontend`, `backend`  
**Goal:** Make planned work manageable rather than static.

- [ ] Mark a study block completed and update task remaining work.
- [ ] Move or delete a study block; optionally lock it in place.
- [ ] Never silently move an explicitly locked block.

**Done when:** user edits survive refresh and remain respected by later plans.

### S-15 — Replan unfinished work safely
**Labels:** `scheduler`  
**Goal:** Adapt when the schedule changes.

- [ ] Recompute availability after a changed event or block.
- [ ] Preserve completed and locked blocks.
- [ ] Identify any remaining unscheduled work.
- [ ] Add regression tests covering rescheduling.

**Done when:** Syllenda proposes alternatives without erasing past work or breaking locks.

## M4 — Future: intelligent and connected

### S-16 — Suggest tasks from natural language with AI
**Labels:** `future`, `integration`  
**Goal:** Reduce repetitive task entry.

- [ ] Convert a short natural-language message into a structured task draft.
- [ ] Validate dates, durations and ambiguous requests.
- [ ] Require explicit user review before saving; do not send unrelated calendar details to the model.

**Done when:** a natural-language request becomes an editable task proposal.

### S-17 — Connect Google Calendar with explicit permissions
**Labels:** `future`, `integration`  
**Goal:** Read changing external commitments without repeated imports.

- [ ] Design sign-in and consent flows; request minimum necessary scopes.
- [ ] Read authorized events and map source identities.
- [ ] Decide clearly whether Syllenda may write back to external calendars.
- [ ] Handle expired authorization and revoked consent.

**Done when:** a changed authorized event is reflected without duplicating other events.

### S-18 — Prepare for external users and deployment
**Labels:** `future`, `backend`  
**Goal:** Turn a personal prototype into a secure hosted product.

- [ ] Implement authentication, user ownership and access control.
- [ ] Move to a suitable hosted database and configure backups.
- [ ] Add privacy messaging, secrets management and a basic deployment pipeline.
- [ ] Test user isolation before making real calendars accessible online.

**Done when:** a second user cannot access the first user's information.

## Recommended order

Start with **S-01 → S-02 → S-03**. Then build **S-04 → S-05 → S-06 → S-07 → S-08 → S-09 → S-10**. Start M2 only once scheduling preferences work. Avoid touching the AI or live Google Calendar issues before the manual planner works.
