# Jobsite Schedule Board

Bancroft Construction's shared job schedule. See `README.md` for features and one-time Supabase setup.

- Live site: https://bancroft-construction.github.io/jobsite-schedule/
- Hosting: GitHub Pages, published from the `main` branch. A push to `main` goes live within a minute
  or two, so there is no separate staging copy.

## Making changes

1. Edit the source, never `index.html` directly:
   - `src/app.html`: the board (views, scheduling engine, imports, exports, calendar).
   - `src/platform.html`: the sign-in screen and the Supabase database layer.
2. Rebuild: `powershell -ExecutionPolicy Bypass -File build.ps1`. This writes `index.html`.
3. Commit the source files and the rebuilt `index.html` together, then push to `main`.

## Data and sign-in

- Data is in Supabase, table `public.sched_docs`. Only signed-in users can read or write it.
- Everyone signs in with one shared Supabase login, set in `config.js`, and then types their name for
  the change history. The anon key in `config.js` is meant to be public.
- Supabase does not grant new tables automatically in this project. Grant them to `authenticated`
  explicitly, and never to `anon`.
- Never commit a `migration/` folder or real exported job and billing data. `.gitignore` excludes them.

## Calendar colors (Bancroft Carpentry only)

On the project whose name contains "Bancroft Carpentry", calendar bars, on-site chips, notes and the
calendar PDF are colored by carpenter: Lucas is orange, Jason is green, and both together are blue.
Names are read from the task's subcontractor and name, with the contact used only when neither has a
name. The rule is in `crewOf()` in `src/app.html`.

## Calendar colors (Paul and Shannon, every project)

On every project, a task whose name starts with "Paul" is Mist (#8aa4b8), one that starts with "Shannon"
is Rose (#d4879a), and one whose name has both of them is Sage (#87a67f). Only the task name is read.
The rule is `paulShannonOf()` in `src/app.html`; on Bancroft Carpentry the Lucas/Jason colors win.
Tasks that match neither rule keep their status colors.

## Highlight colors (picked by hand)

Any task, milestone, phase, note or deadline can be given a highlighter color from the **Highlight**
dropdown in its drawer (10 colors, list `HL` in `src/app.html`, stored as field `hl`). It only marks the
name, like a highlighter pen, in the Gantt, task list, calendar, look-ahead and day view. It never
changes dates, status colors or the crew colors above. It's for the project manager's own color-coding.

## Related site

The Staffing Planner (https://bancroft-construction.github.io/staffing-planner/) exports
`schedule-board.csv`, which this board reads with **Import from planner** (`projectsFromSheets`).

## Brand colors and chart colors

The board uses the Bancroft brand palette (Marble #FFFCFA, Stone #E7E3DD, Copper #8A5D3B, Granite #252928,
Slate #ACACAC) in the `:root` variables of `src/app.html`. The Gantt and calendar panels are the exception:
they carry class `chart-std`, which keeps them white with traditional status colors (gray not started,
blue in progress, green complete, amber on hold, red critical/late, red today line) in light and dark mode.
The PDF export (`drawGanttPDF`, `drawCalendarPDF`) uses the same colors.
