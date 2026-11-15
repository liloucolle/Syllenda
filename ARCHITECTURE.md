# Architecture notes — Syllenda

This is a **starting design**, not a fixed contract. Implement the smallest useful parts first and revise after building the prototype.

## One application, not two

Use Next.js for the React interface and server-side handlers. A separate Express backend would add deployment and maintenance work without a clear benefit for this first personal project.

```text
Browser (Next.js / React)
  ├─ Calendar week view
  ├─ Task editor
  ├─ Preferences
  └─ "Generate plan" + preview
          │
          ▼
Next.js server-side handlers
  ├─ Event / task CRUD
  ├─ Scheduling service (pure TypeScript functions)
  ├─ Routines + import service (later)
  └─ Database access (Prisma + SQLite)
```

## Suggested folder organization (after scaffolding)

```text
src/
  app/                 # Pages and request handlers
  components/          # Calendar, forms, task cards
  lib/
    scheduling/         # Pure planning functions + tests
    db/                 # Prisma access
    calendar-import/    # .ics handling, later
  types/                # Shared TypeScript types
prisma/                 # Schema and migrations
docs/                   # Product and engineering notes
```

## Data model (conceptual, not Prisma syntax)

- **CalendarEvent:** `id`, `title`, `startsAt`, `endsAt`, `kind`, `isFixed`, optional `sourceId` and `externalUid`. Represents existing classes and other commitments.
- **Task:** `id`, `title`, `totalMinutes`, `dueAt?`, `earliestStartAt?`, `priority`, `splitMode`, `minimumChunkMinutes?`, `status`, optional `originTemplateId` and `originEventId`.
- **PlannedBlock:** `id`, `taskId`, `startsAt`, `endsAt`, `status` (`proposed`, `accepted`, `completed`), `isLocked`.
- **TaskTemplate:** `id`, `title`, `durationMinutes`, `ruleType` (`weekly`, `after_class`), rule parameters and `isEnabled`.
- **Preferences:** `timezone`, active days, daily scheduling windows, maximum planned minutes per day, buffer minutes and preferred chunk size.
- **ImportSource** (later): identity and last update time for an imported timetable.

**Important:** Store timestamps consistently (for example as UTC instants), preserve the user's IANA timezone (e.g. `Europe/Paris`), and render local times correctly. Recurrences need explicit timezone and daylight-saving handling.

## Scheduling engine V0

Input: fixed events, accepted/locked blocks, unscheduled tasks, user preferences, and planning horizon.

Output: proposed blocks, unscheduled minutes by task, and warnings.

1. Convert busy events and accepted/locked blocks into occupied intervals.
2. Generate candidate free intervals within allowed hours, after each task's earliest start, and before its due date.
3. Sort tasks by deadline, then priority (an intentionally simple V0 heuristic).
4. For each task, attempt to allocate its remaining minutes:
   - **Single block:** require one continuous interval long enough for all remaining minutes.
   - **Splittable:** allocate sessions respecting the minimum/preferred chunk size; allow a shorter final remainder if configured.
5. Insert temporary reservations so later tasks cannot reuse the same time.
6. Return proposed blocks; if some time cannot fit, return an explicit remainder, not a false success.
7. Only persist proposed sessions once the user confirms the preview.

**Scheduling rules:** existing events are read-only, no overlaps, no past slots, no out-of-hours placement, no deadline violations, optional buffers, and no duplication across repeated planning runs.

Keep this algorithm in a **pure function** such as:

```ts
planSchedule(input: PlanningInput): PlanningResult
```

It should not query the database or call AI services. This makes it easy to test with tiny, deterministic timetables.

## Automation rules V1

- `AfterClassReview`: for each eligible class occurrence, create one review task (default 30 minutes; target completion within 48 hours).
- `WeeklyRoutine`: create a task occurrence on a chosen weekday, with duration, preferred window and optional due time.
- Record an occurrence identity (e.g. `templateId + sourceEventOccurrenceId`) so generating tasks twice does **not** create duplicates.
- Plan only within a bounded horizon (such as the next two weeks), not indefinitely.

## Import V1

- Accept user-uploaded `.ics` files.
- Parse VEVENT data and bounded recurrences; handle cancellations/exceptions where available.
- Preserve source UID + occurrence identity for import deduplication.
- Give imported events a different visual style; treat them as fixed.
- On re-import, update the matching source events rather than duplicating them.
- Do not assume `.ics` import provides live synchronization.

## AI V2: advisory, not autonomous

Potential AI action: `"Prepare my probability homework for Thursday; probably 2 hours"` → structured task proposal with title, estimate, due date and split setting. The user checks and confirms the proposal before it is saved. The LLM **does not** place blocks directly; it calls the validated task-creation layer, and the deterministic engine finds slots.

## Testing priorities

- Two classes with a 30-minute gap; task requires 45 minutes → cannot place there.
- An exact 30-minute opening → a 30-minute task fits once.
- A single-block 90-minute task and only 3 × 30-minute gaps → unscheduled.
- A splittable 90-minute task and 3 × 30-minute gaps → schedule all three.
- Due date tomorrow with no available hours → warn.
- Running planning twice → no duplicate accepted blocks.
- An edited/locked block → remains where the user placed it.
- Week crossing a daylight-saving transition → no hour shifts or overlaps.

## Data privacy

Never commit `.env`, calendar exports, SQLite database files or secrets. Do not send calendar details to an LLM without a clear opt-in, and only send the minimum information needed. Authentication and access controls are required before storing multiple users' real personal calendars on a public server.
