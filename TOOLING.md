# Tooling and setup — Syllenda

## Recommended stack

| Need | Tool | Reason |
|---|---|---|
| Website + server-side logic | **Next.js App Router** | React UI and HTTP handlers in one TypeScript project; no separate Express server needed initially |
| Language | **TypeScript** | Clear types for events, tasks and time intervals |
| Styling | **Tailwind CSS** | Fast styling for a clean, responsive UI |
| Interactive calendar | **FullCalendar React** | Week/day views and editable event interactions |
| Persistence | **SQLite + Prisma** | Lightweight local database and typed data access |
| Calendar import (V1) | **ical.js** | Parse iCalendar `.ics` and recurring events |
| Unit testing | **Vitest** | Test the scheduler without running a browser |
| End-to-end testing (later) | **Playwright** | Verify form → plan → calendar workflows |
| Source control | **Git + GitHub Issues/Projects** | One issue/branch/PR per incremental feature |
| Editor | **VS Code** | Familiar TypeScript workflow |
| UI sketches (optional) | **Figma or pencil-and-paper** | Sketch the week view before coding |

**Version caution (November 2026):** FullCalendar v7 has different package imports from v6 tutorials; follow the official v7 React docs. For Prisma + SQLite, use an ORM release whose SQLite support is stable and compatible with your runtime, rather than copying a random installation snippet.

## Initial setup

Requirements: Node.js and npm, Git, a GitHub account and a code editor.

For a fresh empty workspace:

```bash
npx create-next-app@latest syllenda --typescript --tailwind --eslint --app
cd syllenda
npm run dev
```

The first actual app should show a simple page; postpone database installation until the data-model issue.

If you created a repository and pasted these docs into it first, scaffold Next.js elsewhere and move its generated files into the repo. Keep these documents instead of overwriting them.

## Installing features in stages

- **S-03:** Install and configure FullCalendar using the current official React docs.
- **S-04/S-05:** Introduce TypeScript models and simple in-memory state.
- **S-06:** Set up SQLite + Prisma and migrate the schema; do not commit your local database.
- **S-08:** Add Vitest and scheduler test cases.
- **S-13:** Add ical.js and `.ics` fixtures (synthetic data only).
- **Later:** Add Playwright, authentication and external APIs only when needed.

## GitHub workflow

Create a repository called `syllenda` (suggestion), then milestones `M0` through `M4`. Use labels `frontend`, `backend`, `scheduler`, `automation`, `integration`, `testing`, `docs`, `good-first-issue` and `future`.

Create issues from [ISSUES.md](ISSUES.md) one by one. Example:

```bash
git switch -c feat/s-03-week-view
# implement + test
git add .
git commit -m "feat: add weekly calendar view"
git push -u origin feat/s-03-week-view
```

Open a pull request and merge after reviewing it. This practice is useful even for a solo portfolio project.

## References (official)

- Next.js: https://nextjs.org/docs/app/getting-started
- Next.js route handlers: https://nextjs.org/docs/app/getting-started/route-handlers
- FullCalendar React: https://fullcalendar.io/docs/react
- Prisma + SQLite: https://www.prisma.io/docs/orm
- ical.js: https://github.com/kewisch/ical.js
- Google Calendar `.ics` export: https://support.google.com/calendar/answer/37111?hl=en
- GitHub milestones: https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work
