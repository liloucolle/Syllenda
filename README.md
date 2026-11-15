# Syllenda

**Your studies, perfectly scheduled.**

> Project status: **planning / early development**. The features below describe the product we intend to build; they are not yet implemented.

## The idea

Students already have a calendar full of lectures, labs, deadlines and commitments. The missing piece is finding time for the work *around* those events: reviewing notes, completing homework, running errands and preparing for exams.

**Syllenda turns those intentions into realistic time blocks.** It combines existing events, task estimates, deadlines and personal preferences to propose a study plan that students can inspect and adjust.

## Example

- Monday, 10:00–12:00: Calculus lecture (existing event).
- Rule: *After each lecture, review notes for 30 minutes within the next 48 hours.*
- Task: *Complete problem set* (2 hours total, due Thursday, can be split).
- Recurring task: *Grocery shopping* (1 hour each Saturday).

Syllenda finds available time, proposes review sessions and task blocks, and explains when something cannot fit. It **never moves existing calendar commitments without permission**.

## Planned core features

- **See a calendar:** week view with existing commitments and generated study sessions.
- **Create tasks:** title, time estimate, due date, priority and single-block vs. split sessions.
- **Set preferences:** working hours, days off, maximum study load, break/buffer time and preferred session length.
- **Generate a plan:** find free time before deadlines without overlap, then preview the proposed blocks.
- **Create routines:** weekly chores and a review task triggered by each class.
- **Import a timetable:** start with manual events, then add `.ics` uploads; live calendar synchronization comes later.
- **Stay in control:** edit, lock, complete or reschedule study blocks.

### What makes Syllenda different?

A normal calendar asks, *“When do you want to do this?”* Syllenda asks, *“What needs to get done, and when can it realistically fit?”*

## MVP (first usable version)

The first version will let one user:

1. Enter classes and other busy calendar events manually.
2. Add a task with duration and deadline.
3. Choose **one continuous block** or **multiple sessions**.
4. Set basic available hours.
5. Click **Generate plan** to preview free time blocks.
6. Confirm the plan, edit a block, and see any unscheduled tasks.

No AI model, external calendar account, login system or mobile app is required for the first version. The scheduling logic will be deterministic and testable.

## Proposed technology

- **Web app:** Next.js (App Router), React, TypeScript and Tailwind CSS.
- **Calendar UI:** FullCalendar for React.
- **Database:** SQLite, accessed through Prisma (pin a compatible stable version when setting up).
- **Tests:** Vitest for scheduling logic; Playwright later for end-to-end flows.
- **Calendar import (later):** `.ics` files parsed with ical.js.
- **AI (later):** natural-language task suggestions, with user confirmation.

Why a website first? One TypeScript codebase can cover both the interface and backend, works well on desktop and mobile browsers, and can later become an installable PWA. See [Tooling](docs/TOOLING.md).

## Project documents

- [Product brief and requirements](docs/PRODUCT.md)
- [Architecture and scheduling rules](docs/ARCHITECTURE.md)
- [Progressive roadmap](docs/ROADMAP.md)
- [GitHub issue backlog](docs/ISSUES.md)
- [Tools and setup](docs/TOOLING.md)

## Getting started

Syllenda has not been scaffolded yet. When you start issue **S-01**, create the Next.js project:

```bash
npx create-next-app@latest syllenda --typescript --tailwind --eslint --app
cd syllenda
npm run dev
```

Open `http://localhost:3000`.

If you already created a repository containing these docs, run the scaffolding command **in a temporary directory** and move the generated application files into the repository (without overwriting your README/docs). Then commit the initial scaffold.

## Development approach

Work on **one small GitHub issue at a time**. Make a branch, implement one behavior, test it, and open a pull request (even if you are the only contributor). Keep the application usable after each milestone.

Suggested first issue: **S-01 — Initialize repository and Next.js app.**

## Guiding principles

1. **Trust over magic:** suggestions are visible and editable.
2. **Never double-book:** existing calendar events and locked blocks are respected.
3. **Don't hide failure:** clearly flag work that cannot fit before its deadline.
4. **Rules before AI:** build a reliable scheduling core before asking an LLM to make suggestions.
5. **Privacy by default:** calendars may contain sensitive personal details; never commit real calendar exports or database files.

## License

Not decided yet. Until a license is explicitly added, this repository is not automatically open source for reuse.
